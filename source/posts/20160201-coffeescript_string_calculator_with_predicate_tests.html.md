---
title: From scratch property based testing with CoffeeScript
tagline: Property based testing seems hard especially when looking through the different libraries - the learning curve feels enormous.  This note shows the String Calculator Kata built using property based tests without any special framework - all done with a little bit of randomness and some higher-order functions.
date: 2016-02-01
release: yes
keywords:
  - CoffeeScript
  - Properaty Based Testing
  - Unit Testing
---

# Property Based Testing with CoffeeScript

Each Thursday a group of us get together to perform the [String Calculator Kata](http://osherove.com/tdd-kata-1).  This started out as an exercise in TDD and finding ways to extract efficiency out of our own development process.  Over time, however, our Thursday ritual evolved into a discovery of new languages and a learning of new techniques.  Having played with Scala I recently performed the kata using [ScalaTest](http://www.scalatest.org) and, more specifically, property based tests (PBT).  This turned out to be a fun exercise as it not only resulted in a fewer number of unit tests but I was also able to identify a number of boundary conditions which none of us had noticed with cherry picked test data.

My next attempt at this kata was using CoffeeScript and, more interestingly, I wanted to perform this kata using PBT.  Not having a testing framework like ScalaTest for CoffeeScript I set out to build one as part of the kata only to find that, with higher-order functions, this was a surprisingly simple task. I found that by knocking together tests in this way it was easy to explain to the audience what was happening and it made the exercise significantly more accessible - there was no mystery hidden away within a testing framework like ScalaTest.

## Background

The examples and code extracts covered here can be found at [https://github.com/graeme-lockley/string-calculator-coffeescript-pbt](https://github.com/graeme-lockley/string-calculator-coffeescript-pbt).  The JavaScript libraries used are [Mocha](http://mochajs.org) and [Chai](http://chaijs.com).

## Generators

The basic mechanism of PBT is to use a generator - a generator is a function which, when invoked, will return a random value based on the generator's constraints.  Incidentally when I say function within this context I am referring to a programming version of a function rather than the mathematical definition of a function.

As an example let's say we would like a function that will return a random element from a list.  The following code will do this:

~~~ coffeescript
oneOf = (xs) -> xs[Math.random() * xs.length]
~~~

However this code is not in the form of a generator in that it accepts a list of possible elements and then returns a random element from that list.  To convert this into a generator we return a function rather than an element.  So this code is then transformed as follows:

~~~ coffeescript
oneOf = (xs) -> () -> xs[Math.random() * xs.length]
~~~

Incidentally the need to return a random integer within a fixed range is so common that this code is usually expressed in terms of the helper `integerInRange`:

~~~ coffeescript
integerInRange = (min, max) -> Math.round(Math.random() * (max - min) + min)
oneOf = (xs) -> () -> xs[integerInRange(0, xs.length)]
~~~

So armed with this hint of insight let's work through the tests that were developed when performing the kata.


## Given an empty string should return 0

~~~ coffeescript
it 'Given an empty string should return 0', ->
  sc.add('').should.equal 0
~~~

This is the simple scenario with no opportunity for using PBT.


## Given a non-negative integer should return its value if less than 1001 otherwise 0

~~~ coffeescript
it 'Given a non-negative integer should return its value if less than 1001 otherwise 0', ->
  forAll(nonNegativeIntegers) (n ->
    result = if n < 1001 then n else 0
    sc.add(n.toString()).should.equal result
  )
~~~

In this test we have made reference to two functions which need to be defined - `forAll` and `nonNegativeIntegers`.

`forAll` is a function which, given a generator and an expression, will apply the generator's result to the expression a prescribed number of times.  The default implementation of this function is

~~~ coffeescript
  forAll = (gen) -> (predicate) -> predicate(gen()) for lp in [1..1000]
~~~

Using the idea that a generator is a function which, when invoked, will return a value, `nonNegativeIntegers` is defined as

~~~ coffeescript
  nonNegativeIntegers = () -> integerInRange(0, 10000)
~~~

In this definition `nonNegativeIntegers()` will return a random number between 0 and 10,000.

I was amazed at this point when I realised that PBT was simple and didn't require an enormous testing framework to get right.  All it needed was a little bit of randomness and higher-order functions.

## Given non-negative integers separated with a comma should return the sum of all numbers less than 1001

~~~ coffeescript
it 'Given integers separated with a comma should return the sum of all numbers less than 1001', ->
  forAll(nonEmptyListOf(nonNegativeIntegers)) ((ns) ->
    sc.add(xs.join(',')) should equal sum(ns)
  )
~~~

The additional functions that has been included in the above test is the generator `nonEmptyListOf` and a test utility function `sum`.  As the name suggests `nonEmptyList` is a generator that accepts a generator as argument and returns a function which, when called, will return a list of at least one element and no more than 10 elements with each element being generated from the originally passed generator.  Using the functions that we have already have these additional functions are defined as

~~~ coffeescript
nonEmptyListOf = (gen) -> () -> [1..integerInRange(1, 10)].map((x) -> gen())

sum = (ns) -> ns.filter((x) -> x < 1001).reduce(((x, y) -> x + y), 0)
~~~

## Given non-negative integers separated with a comma or newline should return the sum of all numbers less than 1001

~~~ coffeescript
it 'Given integers separated with a comma or newline should return the sum', ->
  forAll(nonEmptyList(nonNegativeIntegers)) ((ns) ->
    sc.add(mkString(xs, [',', '\n'])) should equal sum(ns)
  )
~~~

I introduced no extra PBT functions although I did have to define the helper function `mkString` which I've provided below:

~~~ coffeescript
mkString = (xs, seps) ->
  if xs.length is 0 then ''
  else if xs.length is 1 then xs[0].toString()
  else xs[0].toString() + oneOf(seps)() + mkString(xs[1..], seps)
~~~

You will however notice that I made use of `oneOf` which was previously defined.

## Given non-negative integers separated by a custom single character separator should return the sum of all numbers less than 1001

~~~ coffeescript
it 'Given non-negative integers separated by a custom single character separator should return the sum of all numbers less than 1001', ->
  forAll2(nonEmptyList(integers), separators) ((xs, sep) ->
    sc.add('//' + sep + '\n' + xs.join(sep)).should.equal sum(xs)
  )
~~~

The simple implementation of separators is

~~~ coffeescript
separators = () ->  String.fromCharCode(integersInRange(33, 127))
~~~

This implementation however does not work because not all characters within the character range are valid separators.  The obvious exclusions are the digits '0'-'9', dash '-' and the square brackets '[' ']'.  The following function, a filter, takes a generator and predicate and will continue to generate values until the predicate returns `true`.

~~~ coffeescript
genFilter = (gen, filter) -> (
    result = gen()
    while (!filter(result))
      result = gen()
    result
  )
~~~

The separators generator can now be expressed as

~~~ coffeescript
 separators = () -> genFilter(
   () -> String.fromCharCode(integersInRange(33, 127)),
   (ch) -> !(ch >= '0' && ch <= '9') && ch != '-' && ch != '[' && ch != ']')
~~~

I also needed to introduce the function `forAll2` which accepts two generators rather than `forAll`'s  solitary generator.  This function is then defined as

~~~ coffeescript
forAll2 = (gen1, gen2) -> (predicate) -> predicate(gen1(), gen2()) for lp in [1..1000]
~~~

## Given non-negative integers separated by multiple multi-character separator should return the sum of all numbers less than 1001

~~~ coffeescript
it 'Given non-negative integers separated by multiple multi-character separator should return the sum of all numbers less than 1001', ->
 forAll2(nonEmptyListOf(nonNegativeIntegers), nonEmptyListOf(stringSeparator)) ((ns, seps) ->
   sc.add('//[' + seps.join('][') + ']\n' + mkString(ns, seps)).should.equal sum(ns)
 )
~~~

For this test I needed to introduce the generator `stringSeparator` which is defined in terms of `separator`.

~~~ coffeescript
stringSeparator = () -> nonEmptyListOf(separator)().join('')
~~~

## Given integers separated by a comma or newline with at least one negative should throw an exception with the negative numbers in the exception message

~~~ coffeescript
it 'Given integers separated by a comma or newline with at least one negative should throw an exception with the negative numbers in the exception message', ->
  forAll(nonEmptyListOf(integers)) ((ns) ->
    if ns.filter((x) -> x < 0).length > 0
      try
        sc.add(mkString(ns, [',', '\n']))
        fail
      catch error
        error.should.equal('Negatives not allowed: ' + ns.filter((x) -> x < 0).join(','))
  )
~~~

For the final test, confirming that negative numbers will result in an exception, no further functions needed to be defined to facilitate the test.

## Conclusion

Oddly enough in my Scala code I don't use ScalaTest for property based testing anymore as it adds a layer of indirection which at times leaves me a little confused groping through the source to try and understand what actually went wrong.

As a further exercise I used a similar approach whilst performing the [kata in Java 8](https://github.com/graeme-lockley/string-calculator-java8-pbt).  More recently I was creating some value objects in Java 6 and found myself wanting to generate value objects to validate methods over these objects.  I then completed the [kata](https://github.com/graeme-lockley/string-calculator-java6-pbt) using Java 6 as an exercise to evolve the PBT boilerplate and see what the tests would look like without lambdas and real closures. Like before, I was amazed at the simplicity and readability of the resulting tests.  As I still write loads of Java 6 and Java 7 code I have taken the learnings from the Java 6 kata and dropped them into a library which I've placed on [github](https://github.com/graeme-lockley/pbt-java6) and is accessible through a maven repo.

In all circumstances I was surprised at how simple it was to create property based tests.

As a final note about PBT - it is making me a better developer.  When I started doing TDD I became a better developer because I needed to think harder how to isolate components in order to make them testable.  This forced me to layer my applications better and segregate responsibilities more clearly. PBT has forced me to understand my language and libraries better as it has a tendency to expose boundary conditions where I have been loose.
