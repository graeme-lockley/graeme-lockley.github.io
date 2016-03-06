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

- I am going to use a [literate style](https://en.wikipedia.org/wiki/Literate_programming) of coding and attempt to show the code derivation in a way that allows the reader the follow my thinking, and
- All of the source is in [github](https://github.com/graeme-lockley/conways-game-of-life-java8)
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

> At each <span style="background-color: yellow">step in time, the following transitions</span> occur...
> The first <span style="background-color: yellow">generation</span> is created by applying the above rules simultaneously to every cell in the seed—births and deaths occur simultaneously, and the discrete moment at which this happens is sometimes <span style="background-color: yellow">called a tick (in other words, each generation is a pure function of the preceding one)</span>.

From this part of the algorithm it is clear that GOL is a function of the form

> tick(tick(tick(... (tick(seed))))

Specifically we require a function called Tick which transforms a generation represented as `Grid` to a new generation also represented as `Grid`.  In Java 8 this function has the following form.

~~~ java
class Tick implements Function<Grid, Grid> {
  @Override
  public Grid apply(Grid generation) {
    ...
  }
}
~~~

> At <span style="background-color: yellow">each step in time</span>, the following transitions occur:
>
> - <span style="background-color: yellow">Any live cell</span> ...
> - <span style="background-color: yellow">Any live cell</span> ...
> - <span style="background-color: yellow">Any live cell</span> ...
> - <span style="background-color: yellow">Any dead cell</span> ...
> {: .default-ul }

The structure of the algorithm describes a transition at a cell level and, specifically, it describes transformation rules in terms of every single cell within a generation.  From this `Tick` can now be enriched into the following.

~~~ java
class Tick implements Function<Grid, Grid> {
  @Override
  public Grid apply(Grid generation) {
    return generation.forEach(c -> tickCell(generation, c));
  }

  private CellState tickCell(Grid generation, Coordinate coordinate) {
    switch (generation.at(coordinate)) {
      ...
    }
  }
}
~~~

In order to implement this change `Grid` needs to be enhanced with the method `forEach`.

~~~ java
public interface Grid {
  Grid forEach(Function<Coordinate, CellState> cellMap);
  CellState at(Coordinate coordinate);
}
~~~

There is of course a small technical concern with implementing `forEach` in that `Grid` represents an *infinite* grid.  I'll return to this concern a little later - for now thought let's focus on transcribing GOL and, specifically, the last piece of the algorithm which is contained within `tickCell`.

Returning to the heart of the algorithm there are four scenarios which can be treated separately.

> - Any live cell with fewer than two live neighbours dies, as if caused by under-population.
> {: .default-ul }

Folding this into `Tick` we then get

~~~ java
class Tick implements Function<Grid, Grid> {
  @Override
  public Grid apply(Grid generation) {
    return generation.forEach(c -> tickCell(generation, c));
  }

  private CellState tickCell(Grid generation, Coordinate coordinate) {
    final int numberOfAliveNeighbours = generation.numberOfAliveNeighbours(coordinate);
    switch (generation.at(coordinate)) {
      case ALIVE:
        if (numerOfNeighbours < 2) {
          return DEAD;
        }
        ...
    }
  }
}
~~~

> - Any live cell with two or three live neighbours lives on to the next generation.
> {: .default-ul }

~~~ java
class Tick implements Function<Grid, Grid> {
  @Override
  public Grid apply(Grid generation) {
    return generation.forEach(c -> tickCell(generation, c));
  }

  private CellState tickCell(Grid generation, Coordinate coordinate) {
    final int numberOfAliveNeighbours = generation.numberOfAliveNeighbours(coordinate);
    switch (generation.at(coordinate)) {
      case ALIVE:
        if (numberOfAliveNeighbours < 2) {
          return DEAD;
        } else if (numberOfAliveNeighbours == 2 || numberOfAliveNeighbours == 3) {
          return ALIVE;
        }
        ...
    }
  }
}
~~~

> - Any live cell with more than three live neighbours dies, as if by over-population.
> {: .default-ul }

~~~ java
class Tick implements Function<Grid, Grid> {
  @Override
  public Grid apply(Grid generation) {
    return generation.forEach(c -> tickCell(generation, c));
  }

  private CellState tickCell(Grid generation, Coordinate coordinate) {
    final int numberOfAliveNeighbours = generation.numberOfAliveNeighbours(coordinate);
    switch (generation.at(coordinate)) {
      case ALIVE:
        if (numberOfAliveNeighbours < 2) {
          return DEAD;
        } else if (numberOfAliveNeighbours == 2 || numberOfAliveNeighbours == 3) {
          return ALIVE;
        } else {
          return DEAD;
        }
        break;
        ...
    }
  }
}
~~~

> - Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.
> {: .default-ul }

~~~ java
class Tick implements Function<Grid, Grid> {
  @Override
  public Grid apply(Grid generation) {
    return generation.forEach(c -> tickCell(generation, c));
  }

  private CellState tickCell(Grid generation, Coordinate coordinate) {
    final int numberOfAliveNeighbours = generation.numberOfAliveNeighbours(coordinate);
    switch (generation.at(coordinate)) {
      case ALIVE:
        if (numberOfAliveNeighbours < 2) {
          return DEAD;
        } else if (numberOfAliveNeighbours == 2 || numberOfAliveNeighbours == 3) {
          return ALIVE;
        } else {
          return DEAD;
        }
      default:
      case DEAD:
        if (numberOfAliveNeighbours === 3) {
          return ALIVE;
        } else {
          return DEAD;
        }
    }
  }
}
~~~

In applying these rules we have added a further method into the interface of `Grid`.

~~~ java
public interface Grid {
  int numberOfAliveNeighbours(Coordinate coordinate);
  Grid forEach(Function<Coordinate, CellState> cellMap);
  CellState at(Coordinate coordinate);
}
~~~

Before finishing off transcribing the algorithm I need to return to `Grid.forEach`.  In a pure sense we are stuck - the algorithm describe a transformation from one `Grid` to a second `Grid`.  However, looking at the details of the algorithm, it is clear that any `DEAD` cell that is surrounded by only `DEAD` cells will remain `DEAD` in the next generation.  Therefore the only cells that participate in this algorithm are those cells that have at least a single `ALIVE` neighbour.  So if the `seed` has a finite number of `ALIVE` cells we can conclude that the participating cells is a finite number.

In order to progress I am going to introduce the method `Grid.forEachRelevantCell` which iterates over all those cells that have at least a single `ALIVE` neighbour.  `Tick` trivially changes to

~~~ java
class Tick implements Function<Grid, Grid> {
  @Override
  public Grid apply(Grid generation) {
    return generation.forEachRelevantCell(c -> tickCell(generation, c));
  }

  private CellState tickCell(Grid generation, Coordinate coordinate) {
    final int numberOfAliveNeighbours = generation.numberOfAliveNeighbours(coordinate);
    switch (generation.at(coordinate)) {
      case ALIVE:
        if (numberOfAliveNeighbours < 2) {
          return DEAD;
        } else if (numberOfAliveNeighbours == 2 || numberOfAliveNeighbours == 3) {
          return ALIVE;
        } else {
          return DEAD;
        }
      default:
      case DEAD:
        if (numberOfAliveNeighbours === 3) {
          return ALIVE;
        } else {
          return DEAD;
        }
    }
  }
}
~~~

whilst `Grid` drops `forEach` with the replacement `forEachRelevantCell`.

~~~ java
public interface Grid {
  int numberOfAliveNeighbours(Coordinate coordinate);
  Grid forEachRelevantCell(Function<Coordinate, CellState> cellMap);
  CellState at(Coordinate coordinate);
}
~~~

## Conclusion

What started out as a TDD problem to implement GOL has turned into a much simpler problem - implement the three methods contained within `Grid`.  The remainder of the GOL solution is nothing more than an exercise in transcribing the algorithm from English into Java 8.

I will not show how to TDD `Grid` as this is a relatively simple exercise.  The code that accompanies this note has two implementations - one a set based implementation and the second a linked list styled implementation.  Interestingly both of these implementations have the following characteristics:

- The instances of `Grid` are value objects, and
- Instances of `Grid` are composed using a builder.
{: .default-ul }

I found this interesting because my instinctive implementation was to make `Grid` implementations mutable.

Finally, having come to the end of this implementation, I found no need to refactor `Grid` away and replace it with a standard Java library.  Factually doing so would have had some severe consequences:

- The standard library would have leaked into `Tick` and materially damaged the readability of that class,
- It would have necessary to commit to an implementation of `Grid` - as it stands the source contains two implementations which can be swopped out with a third depending on the need, and
- The algorithm is the algorithm.  To make this implementation time efficient I have no doubt that much progress can be made by building an optimised version of `Grid` and, specifically, the method `forEachRelevantCell`.  By collapsing `Grid` into a standard library implementation this opportunity would have been lost.
{: .default-ul }
