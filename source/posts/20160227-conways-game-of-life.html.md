---
title: Transcribing rather than Writing Conway's Game of Life
tagline: I consistently see folk writing code from the ground up using TDD techniques without considering the DRY principle.  Conway's Game of Life (GOL) is a good example of this where the the problem statement contains semantic facts which can simply be transcribed into code rather than developed in code.  This note demonstrates how much of GOL can be written without much effort.
date: 2016-03-01
release: no
keywords:
  - Java
  - Game of Life
---

# Transcribing Conway's Game of Life

Conway's Game of Life (GOL) is a problem that is often used in code retreats as it is sufficiently challenging to provoke wonderful discussion and debate, and sufficiently complex that it cannot reasonably be completed within the timeframe.

Having been on a number of code retreats I started to become uneasy with the discussions because the style of the code retreat was always centered around Test Driven Development (TDD) practice.  Having drunk from both the TDD and DRY Kool-Aid fountain my uneasiness was centered around the realisation that the GOL problem statement is actually an algorithm and, rather than exploiting the algorithm as a semantic fact, much of my effort was in building tests from the ground up and assembling the solution through composition.

The intent of this note is to take a different approach:

- Transcribe the algorithm in Java 8,
- Use interfaces to highlight and describe ancillary behavior that is implied by but external to the algorithm, and
- Use TDD to build this ancillary behavior
{: .default-ul }

A further desire was to use the vocabulary of the problem statement/algorithm at all times.  Doing this has the following benefits:

- It makes the code readable to non-technical folk, and
- Reduces cognitive dissonance between the problem space and the solution space
{: .default-ul }

## Conway's Game of Life

The problem statement and algorithm that I am using as the base for this solution is from [Wikipedia](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life):

> The universe of the Game of Life is an infinite two-dimensional orthogonal grid of square cells, each of which is in one of two possible states, alive or dead. Every cell interacts with its eight neighbours, which are the cells that are horizontally, vertically, or diagonally adjacent. At each step in time, the following transitions occur:
>
> - Any live cell with fewer than two live neighbours dies, as if caused by under-population.
> - Any live cell with two or three live neighbours lives on to the next generation.
> - Any live cell with more than three live neighbours dies, as if by over-population.
> - Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.
> {: .default-ul }
>
> The initial pattern constitutes the seed of the system. The first generation is created by applying the above rules simultaneously to every cell in the seed—births and deaths occur simultaneously, and the discrete moment at which this happens is sometimes called a tick (in other words, each generation is a pure function of the preceding one). The rules continue to be applied repeatedly to create further generations.

## Comments

Before we get started some comments:

- I am going to use a [literate style](https://en.wikipedia.org/wiki/Literate_programming) of
- Source is in [github](https://github.com/graeme-lockley/conways-game-of-life-java8)
{: .default-ul }

# Solution

> The universe of the Game of Life is an <span style="background-color: yellow">infinite two-dimensional orthogonal grid</span> of square cells, each of which is in one of two possible states, alive or dead.

From this GOL is played over a grid. Rather than attempting to use one of the standard Java collections and inheriting the abstraction that accompanies a specific collection type, I would rather start with an empty interface `Grid` and then populate it with methods based on usage.  At some point I can reassess and refactor this interface into a native collection but, for now, I would rather not force the abstraction.

~~~ java
interface Grid {
}
~~~

> The universe of the Game of Life is an infinite two-dimensional orthogonal grid of square cells, each of which is in one of <span style="background-color: yellow">two possible states, alive or dead</span>.

It follows that we require an enum called `CellState` which has the two elements `ALIVE` and `DEAD`.

~~~ java
enum CellState {
  ALIVE, DEAD;
}
~~~

> The universe of the Game of Life is an infinite <span style="background-color: yellow">two-dimensional</span> orthogonal grid of square cells, <span style="background-color: yellow">each of which</span> is in one of two possible states, alive or dead.

It further follows that I will need to be able get the state of a cell at a particular coordinate.  To this end introduce a coordinate structure value object.

~~~ java
class Coordinate {
  public final BigInteger x;
  public final BigInteger y;

  public Coordinate(BigInteger x, BigInteger y) {
    this.x = x;
    this.y = y;
  }
}
~~~

A couple of notes on this class and its implementation:

- This class creates value objects in that, once an instance is created, it can not be mutated and the field values themselves can not be mutated,
- I have made the elements of `Coordinate` to be `java.math.BigInteger` which is aligned with an *infinite* grid, and
- Some of you might be anxious that I have exposed the underlying state of `Coordinate` rather than creating a couple of getter methods.  Quite recently I have become comfortable with this style with the knowledge that, should I need to change the internal representation it would be simple to lean on the compiler and make these two fields private, add the getter methods and then fix all of the compilation errors.
{: .default-ul }

The `Grid` interface is expanded to include a method to inspect the `CellState` at a specific `Coordinate`.

~~~ java
interface Grid {
  CellState at(Coordinate coordinate);
}
~~~
