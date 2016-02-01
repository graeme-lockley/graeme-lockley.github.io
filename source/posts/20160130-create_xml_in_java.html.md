---
title: What is the best way to create XML in Java?
tagline: A very common capability that many of my Java programs require is the ability to create XML.  There are a couple of ways to do this but, more often that not, the techniques that I have seen suffer from a number of issues. In this note I show the common set of techniques that I have seen and then to offer my opinion on the trade-offs between the different techniques.
date: 2016-01-30
release: yes
keywords:
  - Java
  - Performance Comparison
  - XML
---

# What is the best way to create XML in Java?

A very common capability that many of my Java programs require is the ability to create XML.  There are a couple of ways to do this but, more often that not, the techniques that I have seen suffer from a number of issues:

- They are unable to reliably create XML.  I am not referring to namespaces or any other XML meta feature but rather the inability to consistently markup XML special characters like '<', '>' and '&'.
- More often than not, even though XML is created, there is no need to read the XML back into Java.  However I often see a marshalling/unmarshalling solution being used without considering trade-offs.
- Insufficient consideration to boiler-plate code - this typically happens when the code base grows organically and there is a reluctance to refactor because the code is (mostly) working.
{: .default-ul }

What I would like to show in this note is the common set of techniques that I have seen and then to offer my opinion on the trade-offs between the different techniques.

## Source Code

All of the code in this note can be found at [https://github.com/graeme-lockley/create-xml-in-java-note](https://github.com/graeme-lockley/create-xml-in-java-note).

# Background to the Business Problem

To make this note interesting I am going to extract a real example from an occurrence that I have encountered in my recent past.  One of the applications that I am intimately involved with is an application written around the year 2000.  It is still being used daily and has undergone numerous changes and enhancements over the course of it's lifespan.  This application remains strategic to the financial institution that built it.

One of the responsibilities of this application is to generate a financial movement instruction on behalf of a client.  The XML message that is used to communicate this movement of funds is called a `payment` - the following is an example of this message instructing the movement of funds from a current account to a 3rd party bank account.

~~~ xml
<payment>
  <creator
    application-name="MobileApp"
    client-id="GroupABC"
    correlation-id="98729872398734"/>
  <reference>PAY837137</reference>
  <when
    year="2015" month="01" day="01"
    hour="12" minute="32" second="67"/>
  <action-date
    year="2015" month="01" day="02"/>
  <currency>USD</currency>
  <amount>123.45</amount>
  <debit-leg
    application-name="CAHOST"
    account="10010012345"
    name="Bob Jones"
    reference="BO123"/>
  <credit-leg
    application-name="IBANK"
    sort-code="691216"
    account="10011234567"
    name="Stevie"
    reference="From Jones"/>
</payment>
~~~

Structurally the message is fairly self explanatory but here are a couple of notes for clarity:

- The `creator` communicates what application issued this message, on behalf of which client and the correlation ID of the instruction.
- The `reference` is the payment's human understandable reference number.
- The `when` is the date and time when the payment instruction was created.
- The `action-date` is date when the funds will be removed from the source account and deposited into the destination account.  This date can be different from `when` due to the different batch rules and cut-off times of different payment gateways.
- The `currency` and `amount` are self explanatory.
- The `debit-leg` and `credit-leg` refer to the accounts that serve as the source and destination of the funds transfer.
{: .default-ul }

The following should be noted:

- This message is beautifully ugly! The format was created in 1998 at a time when there were very few if any industry initiatives around financial services XML based message formats.  Strangely it's simplicity has the demonstrated advantage that it is easy to publish and consume `payment` messages across a host of different platforms.
-  I did not want to improve the format of this message because this is the current reality of the XML marshalling problem that serves as the background to this note.
{: .default-ul }

# Technical Problem

The technical problem faced is how to generate this XML from within Java.  In essence I need to create an implementation for the following interface.

~~~ java
package ideas.xml;

public interface MarshallPayment {
    String marshall(PaymentValue paymentValue);
}
~~~

The value object `PaymentValue` is defined as

~~~ java
package ideas.xml;

public class PaymentValue {
    public final String creatorApplicationName;
    public final String reference;
    public final String creatorClientID;
    public final String valueDate;
    public final String currency;
    public final String amount;
    public final String debitApplication;
    public final String debitSortCode;
    public final String debitAccountNumber;
    public final String debitAccountName;
    public final String debitPaymentReference;
    public final String creditApplication;
    public final String creditSortCode;
    public final String creditAccountNumber;
    public final String creditAccountName;
    public final String creditPaymentReference;
    public final String transactionDate;
    public final String transactionTime;
    public final String debitLedgerAccountNumber;
    public final String creditLedgeAccountNumber;
    public final String correlationID;

    public PaymentValue(
            String creatorApplicationName,
            ...) {
        this.creatorApplicationName = creatorApplicationName;
        ...
    }
}
~~~

As you can see the value object is implemented crudely in the style of a structure - every field is `public final`, the constructor initializes each field and there are no getter or setters.  Nonetheless this value object suffers from a number of flaws which I would usually correct with a refactor:

- Exploit Java's native data-types rather than leaving everything `String`,
- Introduce some basic data modeling - especially given that the debit and credit legs are orthogonal and this state could therefore be captured as a single value object type,
- This class is crying out for other value objects to capture elementary data-types, and
- Although not communicated in this class, some of these fields can be `null`.  I would therefore want to declare those fields as `Optional` and throw `NullPointerException` in the event that any attempt is made to initialize this class with illegal values.
{: .default-ul }

I have however elected to leave this class as is.  The reasons for this are it more accurately reflects the reality of software that has been left to decay over a period of time and this underlying class is not material in exploring the different XML marshalling techniques in this note.

# Solution Approaches

I have created numerous implementations for the `MarshallPayment` interface:

- **Native String Builder**

  This is the simplest implementation of all and assembles raw XML by appending characters into a `StringBuilder` with the shape and content of the desired output, and then invoking `toString` on the builder.

- **DSL with String Builder Visitor**

  Using a bespoke XML DSL that allows the XML message to be described in Java and then, using a `StringBuilder` visitor, create the desired result.

- **DSL with DOM Visitor**

  Using a bespoke XML DSL that allows the XML message to be described in Java and then, using a DOM based visitor, create the desired result.

- **FreeMarker Template**

  This implementation makes use of a [FreeMarker](http://www.freemarker.org) template to layout the XML.

- **JAXB**

  The JEE 'standard' approach - using an XSD definition for the message, generate the marshalling and unmarshalling code and then to use this code to create the desired output.
{: .default-ul }

The remainder of this section will highlight each of the different techniques through code extracts and provide some comments on the technique.

## String Builder Appender

This technique is the simplest of all - create a `StringBuilder`, append the XML content into the `StringBuilder` and then return the resultant string.

~~~ java
public class StringBuilderMarshallPayment implements MarshallPayment {
    @Override
    public String marshall(PaymentValue paymentValue) {
        StringBuilder marshallResult = new StringBuilder();

        marshallResult
                .append("<payment>")
                .append("<creator application-name=")
                .append("\"")
                .append(paymentValue.creatorApplicationName)
                .append("\"")
                .append(" client-id=")
                .append("\"")
                .append(paymentValue.creatorClientID)
                .append("\"")
                .append(" correlation-id=")
                .append("\"")
                .append(paymentValue.correlationID)
                .append("\"")
                .append("/>")
                .append("<reference>").append(paymentValue.reference).append("</reference>")
        ...
        if (paymentValue.creditSortCode != null) {
            marshallResult.append(" sort-code=").append("\"").append(paymentValue.creditSortCode).append("\"");
        }
        ...
        if (paymentValue.creditPaymentReference != null) {
            marshallResult.append(" reference=").append("\"").append(encode(paymentValue.creditPaymentReference)).append("\"");
        }
        ...
        marshallResult
                .append("/>")
                .append("</payment>");

        return marshallResult.toString();
    }
}
~~~

### Timing

Description | Duration | Relative Index All
------------|----------|-------------------
Native String Builder | 53ms | 1.00

The column *Relative Index All* in the above table is carried though across all of the timing tables to show the relative performance of each implementation against this implementation.

### Analysis

Pros:

- This is the simplest technique to get going quickly.
- Not constrained by XML syntax or even XML conventions.
- Fast - very fast giving it the relative index of 1.00.
{: .default-ul }

Cons:

- Marking up of special XML characters like `<`, `>` and `&` needs to be done explicitly.  In the above example note that the credit payment reference is encoded prior to appending.  In my experience this need for explicit marking up of content results in an enormous waste of time both from a testing perspective and production incidents where elements that needed to be marked up were not.
- The code does not match the shape of the XML but rather is a simple stream of instructions - this makes understanding and maintaining the code difficult.
- The developer needs to make sure that the string that is returned is well formed XML.  This results in greater effort and energy spent on unit tests to ensure that the XML is well formed.
{: .default-ul }

## DSL with DOM Visitor

There are two parts to this technique - firstly creating the XML into one or other structure using a DSL and then creating a visitor to traverse this structure and emit a string representation of that structure.

### XML DSL

I created a simple XML DSL builder which is in the package `ideas.xml.dsl`.  Rather than explaining how it works this is how it was used to create an `XMLElement` for the payment example.

~~~ java
public abstract class DSLMarshallPayment implements MarshallPayment {
  protected static XMLElement getXmlElement(PaymentValue value) {
    return document("payment",
      body()
        .addElement("creator",
          attributes()
            .add("application-name", value.creatorApplicationName)
            .add("client-id", value.creatorClientID)
            .add("correlation-id", value.correlationID))
        .addElement("reference", value.reference)
        .addElement("when",
          attributes()
            .add("year", value.transactionDate.substring(0, 4))
            .add("month", value.transactionDate.substring(4, 6))
            .add("day", value.transactionDate.substring(6, 8))
            .add("hour", value.transactionTime.substring(0, 2))
            .add("minute", value.transactionTime.substring(2, 4))
            .add("second", value.transactionTime.substring(4, 6)))
        .addElement("action-date",
          attributes()
            .add("year", value.valueDate.substring(0, 4))
            .add("month", value.valueDate.substring(4, 6))
            .add("day", value.valueDate.substring(6, 8)))
        .addElement("currency", value.currency)
        .addElement("amount", value.amount)
        .addElement("debit-leg",
          attributes()
              .add("application-name", value.debitApplication)
              .addIfNotNull("sort-code", value.debitSortCode)
              .addIfNotNull("account", value.debitAccountNumber)
              .addIfNotNull("name", value.debitAccountName)
              .addIfNotNull("reference", value.debitPaymentReference)
              .addIfNotNull("ledger-account-number", value.debitLedgerAccountNumber))
        .addElement("credit-leg",
          attributes()
            .add("application-name", value.creditApplication)
            .addIfNotNull("sort-code", value.creditSortCode)
            .addIfNotNull("account", value.creditAccountNumber)
            .addIfNotNull("name", value.creditAccountName)
            .addIfNotNull("reference", value.creditPaymentReference)
            .addIfNotNull("ledger-account-number", value.creditLedgeAccountNumber))
    );
  }
}
~~~

### DOM Visitor

In order to create a DOM visitor I created a visitor interface for the XML structure which looks as follows

~~~ java
public interface XMLVisitor {
    void visit(XMLElement xmlElement);

    void visit(XMLText xmlText);

    void visit(XMLBodyLiteral xmlBodyLiteral);

    String asString();
}
~~~

Finally I created an implementation for this interface which assembles a DOM object whilst visiting the underlying XML structure and transforms the DOM object into a string using builtin libraries.  This implementation looks as follows.

~~~ java
public class DOMVisitor implements XMLVisitor {
    private final Document doc;
    private Element currentElement = null;
    private static DocumentBuilder docBuilder;
    private static Transformer transformer;

    static {
        try {
            DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
            docBuilder = docFactory.newDocumentBuilder();

            TransformerFactory tf = TransformerFactory.newInstance();
            transformer = tf.newTransformer();
            transformer.setOutputProperty(OutputKeys.OMIT_XML_DECLARATION, "yes");
        } catch (ParserConfigurationException | TransformerConfigurationException ex) {
            throw new RuntimeException(ex);
        }
    }

    public DOMVisitor() throws ParserConfigurationException {
        doc = docBuilder.newDocument();
    }

    public void visit(XMLElement xmlElement) {
        Element previous = currentElement;
        Element newElement = doc.createElement(xmlElement.name());

        if (currentElement == null) {
            doc.appendChild(newElement);
        } else {
            currentElement.appendChild(newElement);
        }
        currentElement = newElement;

        for (XMLAttribute attr : xmlElement.attributes()) {
            currentElement.setAttribute(attr.name(), attr.value());
        }

        for (XMLBody body : xmlElement.bodyElements()) {
            body.visit(this);
        }

        currentElement = previous;
    }

    public void visit(XMLText xmlText) {
        if (currentElement == null) {
            throw new IllegalArgumentException("Unable to attach a text block at the root of an XML document");
        } else {
            final Text textNode = doc.createTextNode(xmlText.text());
            currentElement.appendChild(textNode);
        }
    }

    public void visit(XMLBodyLiteral xmlBodyLiteral) {
        if (currentElement == null) {
            throw new IllegalArgumentException("Unable to attach a text block at the root of an XML document");
        } else {
            final Text textNode = doc.createTextNode(xmlBodyLiteral.xmlString());
            currentElement.appendChild(textNode);
        }
    }

    public String asString() {
        try {
            StringWriter writer = new StringWriter();
            transformer.transform(new DOMSource(doc), new StreamResult(writer));
            return writer.getBuffer().toString();
        } catch (TransformerException ex) {
            throw new RuntimeException(ex);
        }
    }
}
~~~

The DOM visitor implementation is straightforward - it stores the root element and the current element and then, as it traverses the underlying XML structure it assembles a DOM representation of the XML document that the DSL created.  Finally the `asString` method then transforms the DOM object into a string and returns that.

### Timing

Description | Duration | Relative Index All
------------|----------|-------------------
DSL with DOM Visitor | 718ms | 13.55

### Analysis

Pros:

- Visually assembling the XML using this DSL aligns the code to the natural structure of the `Payment` message.  By looking at the Java code a developer is able to 'feel' the XML structure aiding understanding and therefore quality.
- The XML structure is independent of the visitor.  As will be shown next, the only change when using the *DSL with String Builder Visitor* is to create a new visitor - the DSL stays the same.
- The DSL can be applied to any XML document and is independent of `Payment`.
- No markup is necessary.
- Minimal boiler-plate code.
{: .default-ul }

Cons:

- Even-though the DSL is simple it does represent a new language that needs to be learnt and supported by the development team.
{: .default-ul }

## DSL with String Builder Visitor

This technique uses the same DSL described above but has a visitor implementation that uses the same `StringBuilder` technique that was illustrated the *Native String Builder*.

~~~ java
public class StringBuilderVisitor implements XMLVisitor {
    private final StringBuilder result = new StringBuilder();

    public StringBuilderVisitor() throws ParserConfigurationException {
    }

    public void visit(XMLElement xmlElement) {
        result.append("<").append(xmlElement.name());

        for (XMLAttribute attr : xmlElement.attributes()) {
            result.append(" ").append(attr.name()).append("='").append(XMLEncoder.encode(attr.value())).append("'");
        }

        if (xmlElement.bodyElements().isEmpty()) {
            result.append("/>");
        } else {
            result.append(">");
            for (XMLBody body : xmlElement.bodyElements()) {
                body.visit(this);
            }
            result.append("</").append(xmlElement.name()).append(">");
        }
    }

    public void visit(XMLText xmlText) {
        result.append(XMLEncoder.encode(xmlText.text()));
    }

    public void visit(XMLBodyLiteral xmlBodyLiteral) {
        result.append(xmlBodyLiteral.xmlString());
    }

    public String asString() {
        return result.toString();
    }
}
~~~

### Timing

Description | Duration | Relative Index All | Relative Index
------------|----------|------------------------------------
DSL with String Builder Visitor | 114ms | 2.15 | 1.00
DSL with DOM Visitor | 718ms | 13.55 | 6.30

### Analysis

The pros/cons are similar to *DSL with DOM Visitor* so I shall not repeat them but only highlight additions.

Pros:

- As illustrated through this piece of code and *DSL with DOM Visitor* the visitor is completely independent of the DSL itself and therefore `Payment`.  However this code is tightly dependent on the internal representation of an XML document.
- This is the fastest implementation which generates the XML off of an intermediate structure.  The only quicker implementation is the *Native String Builder*.
{: .default-ul }

Cons:

- The visitor implementation needs to make use of a markup method to ensure that special characters are correctly represented.  This encoding function needs to be built and tested.
{: .default-ul }

## FreeMarker Template

The technique employed here is to use a templating language to create the XML output and then, through the templating language, have the values extracted out of the passed value object during document generation.  The templating language used is [FreeMarker](www.freemarker.org).

The boiler-plate Java is limited to the following class although this class does make use of a utility function class `FreeMarkerUtils`.

~~~ java
public class FreeMarkerMarshallPayment implements MarshallPayment {
    @Override
    public String marshall(PaymentValue paymentValue) {
        Map<String, Object> dataModel = new HashMap<>();
        dataModel.put("payment", paymentValue);

        try {
            return FreeMarkerUtils.template(dataModel, "PaymentTemplate.ftl");
        } catch (TemplateException ex) {
            throw new RuntimeException(ex);
        }
    }
}
~~~

The magic with this technique happens in the template `PaymentTemplate.ftl`.

~~~ ftl
<#escape c as c?xml>
<payment>
    <creator application-name="${payment.creatorApplicationName}" client-id="${payment.creatorClientID}" correlation-id="${payment.correlationID}"/>
    <reference>${payment.reference}</reference>
    <when year="${payment.transactionDate[0..3]}" month="${payment.transactionDate[4..5]}" day="${payment.transactionDate[6..7]}" hour="${payment.transactionTime[0..1]}" minute="${payment.transactionTime[2..3]}" second="${payment.transactionTime[4..5]}"/>
    <action-date year="${payment.valueDate[0..3]}" month="${payment.valueDate[4..5]}" day="${payment.valueDate[6..7]}"/>
    <currency>${payment.currency}</currency>
    <amount>${payment.amount}</amount>
    <debit-leg
        application-name="${payment.debitApplication}"
<#if payment.debitSortCode??>
        sort-code="${payment.debitSortCode}"
</#if>
<#if payment.debitAccountNumber??>
        account="${payment.debitAccountNumber}"
</#if>
<#if payment.debitAccountName??>
        name="${payment.debitAccountName}"
</#if>
<#if payment.debitPaymentReference??>
        reference="${payment.debitPaymentReference}"
</#if>
<#if payment.debitLedgerAccountNumber??>
        ledger-account-number="${payment.debitLedgerAccountNumber}"
</#if>
    />
    <credit-leg
        application-name="${payment.creditApplication}"
<#if payment.creditSortCode??>
        sort-code="${payment.creditSortCode}"
</#if>
<#if payment.creditAccountNumber??>
        account="${payment.creditAccountNumber}"
</#if>
<#if payment.creditAccountName??>
        name="${payment.creditAccountName}"
</#if>
<#if payment.creditPaymentReference??>
        reference="${payment.creditPaymentReference}"
</#if>
<#if payment.creditLedgerAccountNumber??>
        ledger-account-number="${payment.creditLedgerAccountNumber}"
</#if>
    />
</payment>
</#escape>
~~~

### Timing

Description | Duration | Relative Index All
------------|----------|-------------------
FreeMarker Template | 831ms | 15.68

### Analysis

Pros:

- The Java boiler-plate code is minimal and limited to setting up the FreeMarker model and selecting a template.
- By instructing FreeMarker to treat all expressions as XML it is unnecessary to provide any further markup instructions.
{: .default-ul }

Cons:

- The `#if` statements were cumbersome and broke the flow of the template impacting readability and therefore maintainability.
- Needing to learn a templating language with its own expression language.
- The templating expression language does not allow the accessing of member values on model objects.  It is was therefore necessary to change `PaymentValue` to include getter methods over and above the `public final` member variables.
{: .default-ul }

## JAXB

This technique is the typical JEE style - create an XSD, generate the marshalling/unmarshalling code, populate the generated Java beans and then marshall the beans into a string.

The following is the XSD that was to describe `Payment` and what was used in the generation of a set of cooperating Java beans.

~~~ xml
<?xml version="1.0" encoding="UTF-8" ?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:element name="payment">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="creator" type="CreatorType" minOccurs="1" maxOccurs="1"/>
                <xs:element name="reference" type="xs:string" minOccurs="1" maxOccurs="1"/>
                <xs:element name="when" type="TimestampType" minOccurs="1" maxOccurs="1"/>
                <xs:element name="action-date" type="ActionDateType" minOccurs="1" maxOccurs="1"/>
                <xs:element name="currency" type="xs:string" minOccurs="1" maxOccurs="1"/>
                <xs:element name="amount" type="xs:string" minOccurs="1" maxOccurs="1"/>
                <xs:element name="debit-leg" type="TransactionLegType" minOccurs="1" maxOccurs="1"/>
                <xs:element name="credit-leg" type="TransactionLegType" minOccurs="1" maxOccurs="1"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
    <xs:complexType name="CreatorType">
        <xs:attribute name="application-name" type="xs:string" use="required"/>
        <xs:attribute name="client-id" type="xs:string" use="required"/>
        <xs:attribute name="correlation-id" type="xs:string" use="optional"/>
    </xs:complexType>
    <xs:complexType name="TimestampType">
        <xs:attribute name="year" type="xs:string" use="required"/>
        <xs:attribute name="month" type="xs:string" use="required"/>
        <xs:attribute name="day" type="xs:string" use="required"/>
        <xs:attribute name="hour" type="xs:string" use="required"/>
        <xs:attribute name="minute" type="xs:string" use="required"/>
        <xs:attribute name="second" type="xs:string" use="required"/>
    </xs:complexType>
    <xs:complexType name="ActionDateType">
        <xs:attribute name="year" type="xs:string" use="required"/>
        <xs:attribute name="month" type="xs:string" use="required"/>
        <xs:attribute name="day" type="xs:string" use="required"/>
    </xs:complexType>
    <xs:complexType name="TransactionLegType">
        <xs:attribute name="application-name" type="xs:string" use="required"/>
        <xs:attribute name="sort-code" type="xs:string" use="optional"/>
        <xs:attribute name="account" type="xs:string" use="optional"/>
        <xs:attribute name="name" type="xs:string" use="optional"/>
        <xs:attribute name="reference" type="xs:string" use="optional"/>
        <xs:attribute name="ledger-account-number" type="xs:string" use="optional"/>
    </xs:complexType>
</xs:schema>
~~~

The implementation of `MarshallPayment` using JAXB is straightforward.

~~~ java
public class JAXBMarshallPayment implements MarshallPayment {
    @Override
    public String marshall(PaymentValue paymentValue) {
        generated.CreatorType createApplication = new generated.CreatorType();
        createApplication.setApplicationName(paymentValue.creatorApplicationName);
        createApplication.setClientId(paymentValue.creatorClientID);
        createApplication.setCorrelationId(paymentValue.correlationID);

        generated.TimestampType when = new generated.TimestampType();
        when.setYear(paymentValue.transactionDate.substring(0, 4));
        when.setMonth(paymentValue.transactionDate.substring(4, 6));
        when.setDay(paymentValue.transactionDate.substring(6, 8));
        when.setHour(paymentValue.transactionTime.substring(0, 2));
        when.setMinute(paymentValue.transactionTime.substring(2, 4));
        when.setSecond(paymentValue.transactionTime.substring(4, 6));

        generated.ActionDateType actionDate = new generated.ActionDateType();
        actionDate.setYear(paymentValue.valueDate.substring(0, 4));
        actionDate.setMonth(paymentValue.valueDate.substring(4, 6));
        actionDate.setDay(paymentValue.valueDate.substring(6, 8));

        generated.TransactionLegType debitLeg = new generated.TransactionLegType();
        debitLeg.setApplicationName(paymentValue.debitApplication);
        debitLeg.setSortCode(paymentValue.debitSortCode);
        debitLeg.setAccount(paymentValue.debitAccountNumber);
        debitLeg.setName(paymentValue.debitAccountName);
        debitLeg.setReference(paymentValue.debitPaymentReference);
        debitLeg.setLedgerAccountNumber(paymentValue.debitLedgerAccountNumber);

        generated.TransactionLegType creditLeg = new generated.TransactionLegType();
        creditLeg.setApplicationName(paymentValue.creditApplication);
        creditLeg.setSortCode(paymentValue.creditSortCode);
        creditLeg.setAccount(paymentValue.creditAccountNumber);
        creditLeg.setName(paymentValue.creditAccountName);
        creditLeg.setReference(paymentValue.creditPaymentReference);
        creditLeg.setLedgerAccountNumber(paymentValue.creditLedgeAccountNumber);

        generated.Payment payment = new generated.Payment();
        payment.setCreator(createApplication);
        payment.setReference(paymentValue.reference);
        payment.setWhen(when);
        payment.setActionDate(actionDate);
        payment.setCurrency(paymentValue.currency);
        payment.setAmount(paymentValue.amount);
        payment.setDebitLeg(debitLeg);
        payment.setCreditLeg(creditLeg);

        try {
            JAXBContext context = JAXBContext.newInstance(generated.Payment.class);
            Marshaller m = context.createMarshaller();

            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, Boolean.FALSE);
            m.setProperty(Marshaller.JAXB_FRAGMENT, Boolean.TRUE);

            SchemaFactory schemaFactory = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);
            Schema schema = schemaFactory.newSchema(new File("target/classes/Payment.xsd"));

            StringWriter stringWriter = new StringWriter();
            m.marshal(payment, stringWriter);
            return stringWriter.toString();
        } catch (JAXBException e) {
            throw new RuntimeException(e);
        }
    }
}
~~~

Notes:

- The implementation shown above has been replaced with a class called `NaiveSchemaJAXBMarshallPayment`.  The reason for giving this implementation that name is:
	- At runtime much of the heavy lifting of the marshaller is taken up creating the marshaller as opposed to using the marshaller.  This implementation is then considered naive as a marshaller for `generated.Payment` is created every time an XML instance of `PaymentValue` is required.  The timings below show that this has significant overhead.
	- Even-though JAXB generates marshalling/unmarshalling code this code does not enforce any schema. To ensure schema validation it is necessary to explicitly associate the schema with the marshaller at runtime.

- This implementation has been split into 4 implementations to better understand the impact on performance:
    - **NaiveSchemaJAXBMarshallPayment**: Marshaller is created on each invocation and the generated XML is guaranteed to be compliant with the schema
    - **NaiveSchemalessJAXBMarshallPayment**: Marshaller is created on each invocation but the generated XML is not validated against any schema
    - **EfficientSchemaJAXBMarshallPayment**: A marshaller is created once and reused on each invocation and the generated XML is guaranteed to be compliant with the schema
    - **EfficientSchemalessJAXBMarshallPayment**: A marshaller is created once and reused on each invocation but the generated XML is not validated against any schema
{: .default-ul }

### Timing

Over 10,000 iterations the following times were observed:

Description | Duration | Relative Index All | Relative Index
------------|----------|--------------------|---------------
JAXB naive without schema enforcement | 58,862ms | 1,110.60 | 512.10
JAXB naive with schema enforcement | 67,327ms | 1,270.32 | 585.45
JAXB efficient without schema enforcement | 115ms | 2.17 | 1.00
JAXB efficient with schema enforcement | 972ms |  16.45 | 8.45

### Analysis

Pros:

- Externalising the structure of an XML document into an XSD is useful when needing to communicate the XML structure across multiple teams.
{: .default-ul }

Cons:

- The constructing and assembling of the data transfer object to be marshaled is tedious.
- The data transfer objects are anemic - seems like a missed opportunity.
- Validation is not automatically performed on the created XML and therefore optional.
- Setup of the JAXB generator.
- A naive marshaller imposes a significant overhead on execution.
- Marshaller reuse is necessary for handling load however marshallers are not thread-safe therefore requiring extra complexity to manage a marshaller pool in a multi-threaded application.
{: .default-ul }

# Comparison

The following table shows a comparison of all of the timings against the different techniques.

Description | Duration | Relative Index
------------|----------|---------------
Native String Builder | 53ms | 1.00
DSL with String Builder Visitor | 114ms | 2.15
DSL with DOM Visitor | 718ms | 13.55
FreeMarker Template | 831ms | 15.68
JAXB naive without schema enforcement | 58,862ms | 1,110.60
JAXB naive with schema enforcement | 67,327ms | 1,270.32
JAXB efficient without schema enforcement | 115ms | 2.17
JAXB efficient with schema enforcement | 972ms |  16.45

# Recommendation

Based on the timings and observations these are my recommendations:

- For creating ad hoc pieces of XML or simple documents to be localised within an application I would use a simple XML DSL like *DSL with String Builder Visitor*.  This approach is performant, has IDE support, simple to implement and learn, and, if the DSL is well designed, would be readable.
- For creating XML within a more structured environment where there is a strong TDD discipline, performance is critical I might consider *JAXB efficient without schema enforcement* otherwise I would recommend *JAXB efficient with schema enforcement*.
- I can think of no circumstances where I would use a templating approach like FreeMarker.
- I can think of no circumstances to use naive JAXB.
- Even-though *Native String Builder* is performant, it should only be used in exceptional conditions where performance is an absolute premium.  Even under those circumstances I would be reticent in applying this technique without a robust set of generative unit tests and experienced developers who have a deep working understanding of XML and the consequence of special characters.
{: .default-ul }

## Possible Further Work

During the preparation of this note the following was uncovered which warrants further effort:

- I was keen to perform a stylistic code comparison but quickly found that I did not have sufficient objective criteria to apply. Texts like Robert Martin's [Clean Code](https://www.goodreads.com/book/show/3735293-clean-code) can be evolved into a metrics framework, however this is more of a *shu-rule* where-as I gravitate more towards Kent Beck's *four rules of simple design* which is described in his book [Extreme Programming Explained](https://www.goodreads.com/book/show/67833.Extreme_Programming_Explained) and elaborated by Martin Fowler in his article [BeckDesignRules](http://martinfowler.com/bliki/BeckDesignRules.html).  Unfortunately this assessment of design is very human requiring domain context and *ri-mastery*. For example how do you write software that can confirm whether or not a method name "reveals intention"?  Nonetheless I think this is an area of further work to understand how to practically perform a mechanical stylistic code comparison when comparing techniques such as in this note.
- Build a super-simple library that can make marshall resource management effortless within a multi-threaded environment.
- Extract the XML DSL developed for this note into a shared library.
{: .default-ul }
