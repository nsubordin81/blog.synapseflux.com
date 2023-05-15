---
title: "Modularity With Functional Programming Part 2"
date: 2023-04-20 00:00:00 -0500
excerpt: "Welcome to Modularity in Functional Programming! In this series, I will be addressing how using aspects of the functional paradigm can lead to program components that are easier to reuse and expand upon."
classes: wide
published: false
---

In the first post of this series, we covered what modularity means and how functional programming is uniquely qualified to provide you modularity when writing programs. We covered examples from the "Gluing Functions Together" portion of John Hughes's 1984 memo titled "Why Functional Programming Matters." This second post will build on those ideas and cover two more examples from that section of the memo with accompanying Scala 3 sample code.

While the first post demonstrated modularity with higher order functions, primarily with the higher order function foldRight and a list of single elements, these two examples extend that to show that the same manner of function composition can be done to perform operations on more complex data structures such as lists of lists and trees.

After this post, we will move on to the second idea Hughes presented in his memo, "Gluing Programs Together", which focuses on a feature common to many functional programming languages known as lazy evaluation.

If you missed the first post in this series, you can find it [here](/2023/03/functional-modularity-1/)), I recommend going back and reading that post as it explains the ideas these examples are expanding upon.

## Example 1: Higher Order Functions To Operate On Matrices

If you would like to follow along, all you need is the Scala 3 tools installed on your machine. View instructions specific to your OS [here on the scala download page](https://www.scala-lang.org/download/). While in the first post I recommended using a scala 3 REPL to code these examples, because we start working with language constructs in scala that expect us to organize our code into files, I found it easier to use a [scala 3 worksheet](https://docs.scala-lang.org/scala3/book/tools-worksheets.html) in Visual Studio Code for these examples.

In our first example, we will move on from lists of single elements to a 2 dimensional list, or list of lists. When the members of each list are numeric types, this grid style data structure is often called a matrix. You may know from mathematics or data analysis that matrices can be useful structures to operate on, so it is helpful to know that what we've learned so far about composing functions in a modular way can get you to working with matrices without much additional work. First, let's define a matrix:

```scala
    val myMatrix = List(List(1,2), List(3,4), List(5,6))
```

Now, let's set a goal. We want to add together every item in the matrix. By following our modular approach with functional building blocks and higher order functions as glue, we can confidently break down the problem into sub-problem functions and compose them to arrive at a solution.

A matrix is a list of lists, so the first sub problem may be to sum all the items of each of these inner lists. In the first post of this series we covered how the higher-order function foldRight makes it easy to build a function to sum numbers in a list. Let's reuse that:

```scala
    def sum(list:List[Int]):Int = list.foldRight(0)(_+_)
```

This sums the elements of a list, but we need to sum the elements of each of the lists in our list of lists. So we can use another function that we derived from foldRight, map, to apply the sum function over every element (inner list) of the outer list. Even though we derived map, it is also available for Immutable List in Scala[^1]:

```scala
    myMatrix.map(sum) // res: List[Int] = List(3, 7, 11)
```

 That will give us a list of intermediate sums. We already have a sum function to sum over a list, and we can use that on this list of intermediate sums to get the total for the matrix. All together it looks like

```scala
    sum(myMatrix.map(sum)) // res: 21
```

With this example we took the higher order function 'glue' that we learned to apply to folding a list and operations like sum, and with just one more higher order function (with map) and one more function composition (with sum), we were able to handle an operation over a whole matrix. Whereas the folding of the list was the main feature of the set of examples in the first post, now it is just a component of a larger 'glued together' function.

Our final example on gluing functions together will be even slightly more complex and involve folding trees, but we will still be able to build it up from the ideas we've already covered.

## Example 2: Higher Order Functions To Operate On Trees

For this last example of gluing functions together, we will continue to explore the composition of functions with higher order functions while working on a tree data structure.

First, let's discuss the properties of our tree. every element of the tree is a `Node`, and every `Node` can have 0 or more `children`. You may be most familiar with the binary tree which have a simpler structure with a maximum of two `children`, but in this case the number of `children` per `Node` is not given a limit. Here is our definition of the `Tree` type:

```scala
    sealed trait Tree[A]
    case class Node[A](value: A, children: List[Tree[A]]) extends Tree[A]
```

Some things to note about this definition:

1. We use a sealed trait as our parent type for `Tree`. This will force all subtypes of this trait into the same file so nobody can extend the available subtypes elsewhere. It will also ensure that later, when use the `match` keyword to match on an object of the `Tree` type, the compiler can check for us that we have created case statements for  all types of trees that are defined.
2. In our `Tree` type definition, there is only one subtype called `Node`.
3. `Node` has a `value` member, whose type is generic and given by the type parameter `A`. Using a generic type parameter this way will allow the compiler to help us enforce how types are used in our class without having to declare a concrete type like `Int` up front and lock our tree into being only a tree for `Int` values.
4. The node's second member is `children`, a list of Trees that will also use type `A`.

Below we create a simple tree and visualize what it would look like:

```scala
val myTree =
  Node(
    1,
    List(Node(2, List()), Node(3, List(Node(4, List()), Node(5, List()))), Node(6, List()))
  )

/*
    Here is what myTree would look like:

                     1
                   / | \  
                  2  3  6
                    / \
                   4   5
*/
```

If you aren't familiar with the tree data structure, here is a quick run down. The above visualization shows a way to think about a tree's organization. The structure is hierarchical, and even though we could draw this tree left to right it woulds still have no circular connections and it would have defined parent-child relationships. Notice also that the Nodes with empty lists represent terminal points in the tree. These are commonly referred to as "leaves."

There are many operations you can do on a tree, we are going to continue exploring the "fold. As with folding lists and matrices, the goal will be to accumulate values in some fashion across all the elements and end up with a single result object.

For our tree version of fold, we are adding the requirement that there are two functions, one to accumulate from parent to child `Nodes`, and one to accumulate across sibling `Nodes`. Remember that for lists and matrices we were performing one operation, such as sum, over the entire data structure. You could use the same operation on both, but you have the option to use a different one. For example, with the fold we are aiming for, if we wanted to we could fold so that parents are added to their children but their siblings are subtracted from them.

Building fold will be tougher this time, but we will still use the same ideas of higher order functions and composing function calls to get to our answer. Our first step is to break down the tasks of combining the tree elements into subroutines and think about how we will apply them.

the first case we can consider is accumulating elements as we move down the tree. a simple tree to work with for this would be nodes that only have one child:

```scala
val treeLine = Node(1, List(Node(2, List(Node(3,(List()))))))
```

With the restriction that we can only have one child, we essentially have an overcomplicated representation of a list. While this is not how you would normally use a list or a tree, It is a good sub-problem because we've eliminated the challenge of dealing with a child `Node's` siblings. Working with this simpler tree, we just need a way to handle `Nodes` with one child and leaf `Nodes`. Here's a way to account for those cases:

```scala
def foldTreeLine[A](f: (A, A) => A, a: A, tree: Tree[A]): A =
tree match {
  case Node(value, List())        => f(value, a)
  case Node(value, first :: rest) => f(value, foldTreeLine(f, a, first))
}
```

This function makes use of a technique we haven't covered yet in this series. It is known as `pattern matching.` Pattern matching itself is not the focus of this article, but it is has an interesting history and purpose in functional programming, and I've included an article about it in the footnotes.[^2] For our purposes, I'll just describe what is happening.

In the above example,

1. We use the `sealed trait` and `Node` `case class` we defined above to restrict what can be matched.
2. We match on the tree parameter that is passed in, and inside the match we specify two variations of Node to match on.
3. For the first case we set up a pattern with a Node that has two attributes, `value` and `List()`.
    - This means that the `tree` object we pass in would need to have something in value, we don't specify what but whatever it is now lives in that `value` variable and the compiler can infer that the object referred to by `value` is goinge to have the parameterized type `A`.
    - We also specify that the first case's `Node` its `children` should be an empty list.
    - If the object pointed to by `tree` meets that pattern, we call our passed in function `f` and pass in the `value` member of our matched `Node`.
4. For the second case, our `Node` still has a `value` for its `value` member, but then its children needs to be a non-empty list.
    - This is because for the list we deconstruct using our familiar `::`, or `cons` operator from the first post and we are able to specify the name for the first element of the list, `first`, and the name for the variable that holds the rest of the elements, `rest`.
    - If we match on this pattern, then we call the passed in function `f` with our `value` as the first argument, and then a recursive call to `foldTreeLine` with the same function `f`, `a` our base case return value, and the first element of the `Node`'s list of trees as the tree argument. Note that we are only safe throwing away the `rest` member representing the rest of the elements of the list because we put a constraint on `treeLine` that each Node's tree list can only have the one element. Here is an example of calling this `foldTreeLine` function on our sample tree using the `sum` function, which should correctly sum the values in the `tree's` `Nodes`:

```scala
val treeLineFoldResult: Int = foldTreeLine((a: Int, b: Int) => a + b, 0, treeLine)
```

Now we are getting somewhere, this correctly sums the values in the above constrained tree to 6! Before we break out the champagne though, we know this wouldn't work if we had more than one tree in a given `Node`'s `children` member, because we are explictly ignoring the `rest` portion of that pattern match in each case. We've written a function that can handle a very small, very specific subset of the trees we want to be able to fold over, but it doesn't work for all of these trees. Nor does it meet our special requirement, that if we had more than one element in a `Node`'s `children`, we would want to be able to apply a different function than `f`.

Now we have to try and break down and work on dealing with `rest` as a subroutine. We need a function that can execute a function over a list of trees. I like to start thinking through these problems with a simple example of the data structure, so here is an example of a list of trees:

```scala
val treeList = List(Node(1, List()), Node(2, List()), Node(3, List()))
```

Whereas our first subproblem constrained the breadth of the tree, this list of trees contains only trees that each have no depth. We need an operation that will perform a passed in function on each of these trees and accumulate the result. I'm about to give away my solution, so if you want to think about how you might solve it now is a good time to close this article for a little while and come back to it.

```scala
def foldTreeList[A](g: (A, A) => A, a: A, treeList: List[Tree[A]]): A =
  treeList match {
    case Node(value, children) :: rest =>
      g(value, foldTreeList(g, a, rest))
    case Nil => a
  }
```

Ok, so in this sub-problem, a few things are happening:

1. we are pattern matching again on `treeList` which is an object of type `List[Tree[A]]`, with 2 cases:
    - the first case's pattern matches on a deconstruction of `treeList` into a `first`::`rest` and then `first` into a tree which can be represented as `Node(value, children)`.
    - the second case just matches on `Nil`, which is the empty companion object to scala's `List` class we've seen in the first post.
2. If we match on the first case, we will call a new function `g` that takes two arguments that have the same type as our `A` type and returns a result of that same type. For the first argument we use the `value` of the `Node` at the head of our list, and then for the second value, we do a recursive call with the `rest` of the list. Since `rest` is also a `List[Tree[A]]` we will be able to process the rest of the list until we reach the empty list and have our base case be the second argument to `g`.

Here is an example of how you would call this subroutine:

```scala
val foldTreeListResult: Int = foldTreeList((a: Int, b: Int) => a + b, 0, treeList)
```

Now we have 2 parts of the problem, but each of them can only match on their own type, and each of them can only call their own function. I'm about to show you you can compose these functions in such a way that you can achieve the overall goal. Another opportunity to pause before reading through to this solution.

```scala
object Tree {
  def reduceTree[A](f: (A, A) => A, g: (A, A) => A, a: A, tree: Tree[A]): A =
    tree match {
      case Node(label, children) => f(label, reduceSubtree(f, g, a, children))
    }
  def reduceSubtree[A](
      f: (A, A) => A,
      g: (A, A) => A,
      a: A,
      subtrees: List[Tree[A]]
  ): A = subtrees match {
    case first :: rest =>
      g(reduceTree(f, g, a, first), reduceSubtree(f, g, a, rest))
    case List() => a
  }

}
```

```scala
val sum =
  Tree.reduceTree((a: Int, b: Int) => a + b, (a: Int, b: Int) => a + b, 0, myTree)
```
