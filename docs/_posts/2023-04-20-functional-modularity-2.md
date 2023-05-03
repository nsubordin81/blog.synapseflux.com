---
title: "Modularity With Functional Programming Part 2"
date: 2023-04-20 00:00:00 -0500
excerpt: "Welcome to Modularity in Functional Programming! In this series, I will be addressing how using aspects of the functional paradigm can lead to program components that are easier to reuse and expand upon."
classes: wide
---

In the first post of this series, we covered what modularity means and how functional programming is uniquely qualified to provide you modularity when writing programs. We covered examples from the "Gluing Functions Together" portion of John Hughes's 1984 memo titled "Why Functional Programming Matters." We will make this second post a short one. It will cover two more examples from that section of the memo with accompanying Scala 3 sample code.

While the first post demonstrated modularity with higher order functions, primarily with the higher order function foldRight and a list of single elements, these two examples extend that to show that the same composition can be done to perform operations on lists of lists as well as trees.

After this post, we will move on to the second idea Hughes presented in his memo, "Gluing Programs Together" with lazy evaluation.

If you missed the first post in this series, you can find it [here](/2023/03/functional-modularity-1/)), I recommend going back and reading that post as it explains the ideas these examples are expanding upon.

## Example 1: Higher Order Functions To Operate On Matrices

If you would like to follow along, all you need is the Scala 3 tools installed on your machine. View instructions specific to your OS [here on the scala download page](https://www.scala-lang.org/download/). Once installed, fire up a read-eval-print-loop (REPL) for Scala 3 by typing ‘scala’ into your terminal.

In our first example, we will move on from lists of single elements to a 2 dimensional list, or list of lists. When the members of each list are numeric types, this grid style data structure is often called a matrix. You may know from mathematics or data analysis that matrices can be useful structures to operate on, so it is helpful to know that what we've learned so far about composing functions in a modular way can get you to working with matrices without much additional work. First, let's define a matrix:

```scala
    val myMatrix = List(List(1,2), List(3,4), List(5,6))
```

Now, let's set a goal. We want to add together every item in the matrix. Since we are using this modular approach of composing a solution from known functions, we can be confident that if we break down the problem into sub-problems and define functions for those, we can compose these functions to arrive at a working solution.

We have a list of lists, so the first sub problem may be to sum all the items of each of these inner lists. In the first post of this series we covered how the higher-order function foldRight makes it easy to build a function to sum numbers in a list. Let's reuse that:

```scala
    def sum(list:List[Int]):Int = list.foldRight(0)(_+_)
```

This sums the elements of a list, but we need to sum the elements of each of the lists in our list of lists. So we can use another function that we derived from foldRight, map, to apply the sum function over every element (inner list) of the outer list [^1]. That will give us a list of intermediate sums, which we will need to then sum to get our final result. All together it looks like this:

```scala
    sum(myMatrix.map(sum))
```

## Example 2: Higher Order Functions To Operate On Trees

```scala
    sealed trait Tree[A]
    case class Node[A](value: A, Children: List[Tree[A]]) extends Tree[A]
```

```scala
object Tree {
  def reduceTree[A](f: (A, A) => A, g: (A, A) => A, a: A, tree: Tree[A]): A =
    tree match {
      case Node(label, children) => f(label, reduceTreePrime(f, g, a, children))
    }
  def reduceTreePrime[A](
      f: (A, A) => A,
      g: (A, A) => A,
      a: A,
      subtrees: List[Tree[A]]
  ): A = subtrees match {
    case first :: rest =>
      g(reduceTree(f, g, a, first), reduceTreePrime(f, g, a, rest))
    case List() => a
  }

}
```

```scala
val tree =
  Node(
    1,
    List(Node(2, List()), Node(3, List(Node(4, List()), Node(5, List()))))
  )
```

```scala
val sum =
  Tree.reduceTree((a: Int, b: Int) => a + b, (a: Int, b: Int) => a + b, 0, tree)
```
