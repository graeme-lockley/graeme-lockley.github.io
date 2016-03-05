---
title: Value objects and Java
tagline: I understand the theory and thinking behind value objects but always felt that the problems I worked with didn't their benefits.  In a number of recent projects I have started to use them and have found them to be delightful - they solve so many problems that I never realised where actual problems.  This note describes how I structure my value objects in Java.
date: 2016-02-27
release: yes
keywords:
  - Java
  - Value Objects
---

# Value Objects and Java

Recently I was involved in developing an application that had the following non-functional requirements:

* The application would receive a continuous stream of concurrent REST requests,
* Concurrent requests would be received against a shared state and would need to update the shared state,
* The inbound requests are modelled in the form of events with any event able to be raised irrespective of what state the application is in, and
* Down stream service providers were not guaranteed to be reliable.
{: .default-ul }

Although I am not going to describe the architecture of the application in any further detail it is worthwhile to note the following:

* Updating of the shared state was restricted to as small a piece of code as possible.  Factually this application had 100 files totalling just under 5,000 lines of Java source. Concurrency control was restricted to 3 files of which 2 were managing a shared dictionary whilst the third performed the true concurrency control.
* The module was layered using a hexagonal architecture - we did enjoy the anticipated benefits of a hexagonal architecture through an isolated business model, adaptors to describe dependency boundaries and ports managing the detail of inbound requests and outbound services.
* Apart from the 3 classes described above all other classes were modelled and developed as value objects.
{: .default-ul }


## Value Object Structure

One of our value objects was to record a user's Active Directory name.  Usually this value would have been held in a `String` and passed around the application as a `String`.  In this project we used the following class to represent this concept.

~~~ java
public class ADName {
	private final String userName;

	private ADName(String userName) {
		this.userName = userName;
	}

	public static ADName from(String userName) {
		return new ADName(userName);
	}

	public String asString() {
		return userName;
	}

	@Override
	public boolean equals(Object o) { ... }

	@Override
	public int hashCode() { ... }
}
~~~

A couple of comments on this style:

* The classes fields are always marked as final.  This allows the compiler to support us in ensuring that we do not accidentally update the field, and with tool support, confirm that the constructor assigns a value to each field.
* One or more static `from` methods are used as factory methods to create instances of the value object.
* The constructor is always made private and receives a value for each of the classes fields.  The reason for this is that instances of this class are created using the static `from` methods.
* We used `as` methods to expose the atomic elements of a value object.  We could have used a `toString` but found that the purpose of `asString` and `toString` are different - `asString` is aligned with the business domain whilst the `toString` is aligned to the developer and more often used for logging purposes.  We found that creating a strict usage for `toString` improved the quality of this method.
* We always created an `equals` and `hashCode` method as we could then treat value objects as key fields within maps.
{: .default-ul }


## Benefits

Following the completion of this project we noted the following benefits of using value objects.

* No locking - we never had to think about this and by isolating concurrency control the rest of the application was nothing more than a non-threaded deterministic application which we were able to subject to non-brittle unit testing.
* Easy to create and FAST - the `from` methods made object creation straightforward and by carefully selecting the class names were closely aligned with the ubiquitous language.
* Never being concerned with state mutation means never having to manage the consistency of an object's internal state other than at object creation.
* Given that Java is strongly typed there was never the problem of accidentaly assigning incompatible values to each other.  This would not have been the case if we had made the user's Active Directory name a `String`.
* During the development we had a change in requirement which resulted in us representing a business concept as two separate value objects rather than a single value object.  The refactoring was straightforward as we were able to lean on the compiler to identify the usage of the former value object and then to safely introduce the new classes.
{: .default-ul }

## Observations

* Worked really well - the value derived far exceeded the extra lines of code that was necessary to support.
* Splitting a value object into two values objects and being able to lean on the compiler was massive.
* Functionally things just worked - there was no go-live (or take-off) anxiety.
{: .default-ul }
