---
title: "Modularity With Functional Programming Part 2"
date: 2023-04-20 00:00:00 -0500
excerpt: "Welcome to Modularity in Functional Programming! In this series, I will be addressing how using aspects of the functional paradigm can lead to program components that are easier to reuse and expand upon."
classes: wide
---

In the first post of this series, we covered what modularity means and how functional programming is uniquely qualified to provide you modularity.

We covered examples from the "Gluing Functions Together" portion of John Hughes's 1984 memo titled "Why Functional Programming Matters." This second post will build on those ideas and cover two more examples from that section of the memo with accompanying Scala 3 sample code.

The two examples in this post extend the first post's "functions as glue" concept to show that the ideas of modularity through function composition and higher order functions can be used to perform operations on more complex data structures such as lists of lists and trees.

Following this post the series will move on to the second idea Hughes presented in his memo, "Gluing Programs Together", which focuses on a feature common to many functional programming languages known as "lazy evaluation."

If you missed the first post in this series, you can find it [here](/2023/03/functional-modularity-1/), I recommend going back and reading it if the ideas of modularity and higher order functions presented here are feeling unfamiliar.

## Example 1: Higher Order Functions To Operate On Matrices

If you would like to follow along, all you need is the Scala 3 tools installed on your machine. View instructions specific to your OS [here on the scala download page](https://www.scala-lang.org/download/).

While in the first post I recommended using a scala 3 REPL to code these examples, for these I recommend an editor like Visual Studio Code and [scala 3 worksheets](https://docs.scala-lang.org/scala3/book/tools-worksheets.html) instead. We start working with slightly larger programs in these examples, and it will be easier to write longer procedures out and save them for later instead of submitting commands to a CLI interpreter. With worksheets we still get the benefit of not having to perform manual compile and run steps each time we want to see intermediate results.

In our first example, we will move on from lists of single elements to a 2 dimensional list, or list of lists. When the members of each list are numeric types, this grid-style data structure is often called a "matrix." You may know from mathematics, computer graphics or data analysis that matrices can be useful structures to represent systems of equations, which lends a practical flavor to our example. We can add very little code to what we already did in the first post with lists to start working with matrices.

First, let's define a concrete example matrix:

```scala
    val myMatrix = List(List(1,2), List(3,4), List(5,6))

    /* visual
    [ 
      [ 1, 2 ],
      [ 3, 4 ],
      [ 5, 6]
    ]
```

Now, let's set a goal. We want to add together every item in the matrix.

We'll keep to our modular approach, with functional building blocks and higher order functions as glue. This way we can confidently break down the problem into sub-problem functions and compose them to arrive at a solution.

A matrix is a list of lists, so the first sub problem we'll choose is to sum all the items of each of these inner lists.

In the first post of this series we covered how the higher-order function foldRight makes it easy to build a function to sum numbers in a list. Let's reuse that:

```scala
    def sum(list:List[Int]):Int = list.foldRight(0)(_+_)
```

Sum by itself will only sum the items of one list, but we have a list made of lists. We need to apply sum to every sub-list. We can use another function that we derived from foldRight, map, to apply the sum function over every element (inner list) of the outer list.

Even though we derived map from foldRight in the first post, it is also available natively for Immutable List in Scala[^1]. We'll use this library method for convenience:

```scala
    myMatrix.map(sum) // res: List[Int] = List(3, 7, 11)
```

 Mapping over the matrix with the sum function will give us a list of intermediate row-wise sums. These intermediate sums are just a list of numbers again, so we can use our existing sum function on that list to get the total for the matrix. Putting everything we've done so far together, it looks like:

```scala
    def sum(list:List[Int]):Int = list.foldRight(0)(_+_)

    def matrixSum(matrix: List[List[Int]]): Int = sum(matrix.map(sum))

    val result = matrixSum(myMatrix)
```

With this example we took the higher order function 'glue' that we already used for folding a list, and with just one more higher order function (mapping over the inner lists) and one more function composition (taking the sum of the list of intermediate results), we were able to handle the sum operation over a whole matrix.

Whereas the folding of the list was the main feature of the set of examples in the first post, now it is just a component of a larger 'glued together' function that works on a more complex data structure.

Our final example on gluing functions together will be even slightly more complex and involve folding trees, but we will still be able to build it up from the ideas we've already covered.

## Example 2: Higher Order Functions To Operate On Trees

For this last example of gluing functions together, we will continue to explore composition of functions using higher order functions while working on a tree data structure.

First, let's discuss the properties of our tree. every element of the tree is a `Node`, and every `Node` can have 0 or more `children`. You may be most familiar with binary trees, which have a simpler structure to the trees we are talking about here with a maximum of 2 `children`. In this case the number of `children` per `Node` is not given a limit. Here is our definition of the `Tree` type:

```scala
    sealed trait Tree[A]
    case class Node[A](value: A, children: List[Tree[A]]) extends Tree[A]
```

Some things to note about this definition:

1. We use a sealed trait as our parent type for `Tree`. A sealed trait will force all subtypes of this trait into the same file so nobody can extend the available subtypes elsewhere. It will also ensure that later, when use the `match` keyword to pattern match on an object of the `Tree` type, the compiler can check for us that we have created `case` statements for all types of trees that are defined[^2].
2. In our `Tree` type definition, there is only one subtype extending the trait. It is called `Node`.
3. `Node` has a `value` member, whose type is generic and given by the type parameter `A`. Using a generic type parameter this way will allow the compiler to help us enforce how types are used in our class without having to declare a concrete type like `Int` for `value` up front and lock our tree into being only a tree for `Int` objects[^3].
4. The node's second member is `children`, a list of Trees that will also use type `A` for their `value` members.

Below we create a simple tree and visualize what it would look like:

```scala
val myTree: Tree[Int] =
  Node(
    1,
    List( 
      Node(2, List()), 
      Node(3, List(
        Node(4, List()), Node(5, List()))
      ), 
      Node(6, List())
    )
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

If you aren't familiar with the tree data structure, here is a quick run down.

- The above visualization shows a way to think about a tree's organization.
- The structure is hierarchical, and even though we could draw this tree left to right instead of top to bottom it would still have no circular connections and it would have defined parent-child relationships.
- Notice also that the `Node` elements with empty lists represent terminal points in the tree. These are commonly referred to as "leaves."
- This is a Tree[Int], we chose Int as the type of teh values in the nodes and bound this instance of Tree to the Int type[^4].

Let's continue exploring higher order function glue with folding trees. As with folding lists and matrices, the goal for folding trees will be to accumulate values across all the elements with some operation and end up with a single result object.

For our tree version of fold, we are adding the requirement that there are two functions, one to accumulate from parent to child `Nodes`, and one to accumulate across sibling `Nodes`. Remember that for lists and matrices we were performing one operation, such as sum, over the entire data structure. For this tree folding, we should be able to use the same operation for both combining down and across a tree, but have the option to use a different operation for each of these accumulations if we choose. For example, with the fold we are aiming for, if we wanted to we could fold so that parents are summed with their children but siblings are combined by taking the difference.

Building fold will be tougher this time, but we will still use the same ideas of higher order functions and composing function calls to get to our answer. Our first step is to break down the tasks of combining the tree elements into subroutines and think about how we will apply them.

the first case we can consider is accumulating elements as we move down the tree. a simple tree to work with for this would be nodes that only have one child:

```scala
val treeLine = Node(1, List(Node(2, List(Node(3,(List()))))))

/* visualized:
      1
       \
        2
         \
          3
*/
```

With the restriction that we can only have one child, we essentially have an overcomplicated representation of a list. While this is not how you would normally use a tree, It is a good sub-problem because we've eliminated the challenge of dealing with a child `Node's` siblings. Working with this simpler tree, we just need a way to handle `Nodes` with one child and leaf `Nodes`. Here's a way to account for those cases:

```scala
def foldTreeLine[A](f: (A, A) => A, a: A, tree: Tree[A]): A =
tree match {
  case Node(value, List())        => f(value, a)
  case Node(value, first :: rest) => f(value, foldTreeLine(f, a, first))
}
```

This function makes use of a technique we used for `foldRight` in the first post of this series but didn't cover in more depth. It is known as `pattern matching.` Pattern matching itself is not the focus of this post, but it is has an interesting history and purpose in functional programming, and I've included an article about it in the footnotes.[^5] For our purposes, I'll just describe what is happening.

In the above example,

1. We use the `sealed trait` and `Node` `case class` we defined above to restrict what an instance of `Tree[A]` can match to.
2. We match on the tree parameter that is passed in, and inside the match we specify two deconstructed variations of `Node` to match on.
3. For the first case, we set up a pattern with a Node that has two attributes, `value` and `List()`.
    - This means that the `tree` object we pass in would need to have something in value, we don't specify what but whatever it is now lives in that `value` variable and the compiler can infer that the object referred to by `value` is going to have whatever type gets assigned to `A`.
    - We also specify that in the first case's `Node`, its `children` should be an empty list.
    - If the object pointed to by `tree` meets that pattern, we call our passed in function `f` and pass in the `value` member of our matched `Node`.
4. For the second case, our `Node` still has a `value` for its `value` member, but then its `children` member needs to be a non-empty list.
    - This is because for the list we deconstruct using our familiar `::`, or `cons` operator from the first post and we are able to specify the name for the first element of the list, `first`, and the name for the variable that holds the rest of the elements, `rest`.
    - If we match on this pattern, then we call the passed in function `f` with our `value` as the first argument, and then a recursive call to `foldTreeLine` with the same function `f`, `a` our base case return value, and the first element of the `Node`'s list of trees as the tree argument. Note that we are only safe throwing away the `rest` member representing the rest of the elements of the list because we put that constraint on our tree representation (`treeLine` and trees like it) that each Node's `children` list can only have one element.

Here is an example of calling this `foldTreeLine` function on our sample tree using the `sum` function, which should correctly sum the values in the `tree's` `Nodes`:

```scala
val treeLineFoldResult: Int = foldTreeLine((a: Int, b: Int) => a + b, 0, treeLine)
```

Now we are getting somewhere, this correctly sums the values in the above constrained tree to 6! Before we break out the champagne though, we know this wouldn't work if we had more than one tree in a given `Node`'s `children` member, because we are explicitly ignoring the `rest` portion of that pattern match in each case.

We've written a function that can handle a very small, very specific subset of the trees we want to be able to fold over, but it doesn't work for trees where Nodes can have more than one child. Nor does it meet our special requirement, that if we had more than one element in a `Node`'s `children`, we would want to be able to apply a different function than `f` to combine them.

For our second sub-problem, let's break down and work on processing over a `Node's children` member as a subroutine. We need a function that can execute a function over a list of trees. I like to start thinking through these problems with a simple example of the data structure, so here is an example of a list of trees:

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

1. we are pattern matching again on `treeList`, which is an object of type `List[Tree[A]]`, with 2 cases:
    - the first case's pattern match takes a deconstruction of `treeList` into a `first`::`rest` and then further deconstructs `first` into a tree which can be represented as `Node(value, children)`.
    - the second case just matches on `Nil`, which is the empty companion object to scala's `List` class we've seen in the first post.
2. If we match on the first case, we will call a new function `g` that takes two arguments that have the same type as our `A` type and returns a result of that same type. For the first argument we use the `value` of the `Node` at the head of our list, and then for the second value, we do a recursive call with the `rest` of the list.
3. Since `rest` is also a `List[Tree[A]]`, it matches our `List` type for the `treeList` parameter and it will continue to mach for all recursive calls including when we reach the base case of `Nil` and match on the second pattern to return `a` as the second argument to the final call to `g`.

Here is an example of how you would call this subroutine:

```scala
val foldTreeListResult: Int = foldTreeList((a: Int, b: Int) => a + b, 0, treeList)
```

Now we have 2 parts of the problem, but because the first sub-problem is working on a `Tree[A]` and the second is working on a `List[Tree[A]`, we have functions that only match on their expected types and only have parameters that match these types. The tree we want to fold over would have both of these subtypes in it, that of parent-child (`Node`-to-`Node`) and sibling (across `List[Tree[A]]`) relationships. I'm about to show you you can compose these functions in such a way that you can achieve the overall goal. Another opportunity to pause before reading through to this solution and think about how you would do it or give it a try in your worksheet!

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
    case Nil => a
  }

}
```

The above solution uses function composition to tie together the two functions that we separately designed to cover the components of this tree fold operation.[^6] Note I've not actually used `foldTreeLine` or `foldTreeList` directly in the solution, because neither function by itself could solve the whole problem. Instead I've introduced the new names `reduceTree` and `reduceSubTree` for the functions that embody those subproblem solutions but are altered to solve the problem in a way that generalizes to all `Trees`. Here is some detail of what is going on in the code:

1. `foldTreeLine` and its problem of applying some function `f` down the nodes a a tree is represented in `reduceTree`'s single case `match` statement. But remember we had to pick the first element of the list in our recursive calls before to end up with a `Tree` type argument each time.
2. Now we don't, because `reduceTree` delegates this job to `reduceSubtree`, which takes in the entire list of children. `reduceSubTree` has the logic we put in to fold across a list using some function `g` and return when we had exhausted the list and hit `Nil`.
3. This would cover the first time we had to process children, but then what if each of those children has children? To account for this, we are replacing the first argument in the call to `g` with a call  _back_ to the `reduceTree` function, so as we move across the `subtrees` list, we will try to also move down into that `Node's` children. These functions calling each other allow for recursion both down and across the tree.
4. In all of this we managed to also meet the criteria we gave ourselves that we would need to be able to support different functions for folding across nodes on the same level and folding down by having `f` and `g` be separate functions. If we wanted to enforce only one folding function for both, we could have stuck with just one function, like `f` and used it in both contexts. Alternatively, we can just pass the same function in for `f` and `g`, as we do in the example below.

To demonstrate this solution working, below is a call to the `reduceTree` function using sum as the operation to fold with for both parent-child and sibling directions.

```scala
val sum =
  Tree.reduceTree((a: Int, b: Int) => a + b, (a: Int, b: Int) => a + b, 0, myTree)
```

## Summary

These 2 additional examples took what we covered in the first post of the series and expanded it to show that with higher order functions and function composition, functions that we define for simpler problems can generalize to more complicated data representations.

The solutions to folding over a matrix and a tree were more involved and did require more up front thought to break down problems, but what I'm hoping is you also got a sense for how programming in this way encourages the reuse of general purpose functions such as `foldRight` and `map`, and also encourages the typically helpful approach of solving complex problems by first subdividing them into simpler parts.

Another significant benefit we have by programming in this mode is we have avoided the state mutations that arise from using imperative constructs such as variable assignment and looping, so it will just follow that our code is thread-safe and simpler to reason about. We can reuse the functions we make with them, like `matrixSum`, `reduceSubTree`, and `reduceTree` that we've created here, just based on the inputs and return values of their signature and their description without worrying that combining them will have unintended effects.

If you want to further prove to yourself that these functions are easily combined, I suggest trying to combine what we created here with some requirement. For example, create a `Tree[List[List[Int]]]`, or tree of matrices, and then find use some composition of the functions you've defined through this post to find either the sum or another type of accumulation across all of the tree elements.

With "gluing functions together" sufficiently covered, the next post in the series will introduce the concept of 'lazy evaluation' which will help us think a level higher and "glue programs together."

## Where Can I Learn More?

(I'm copying this section to the bottom of every post in the series to make it easy to find, so no need to read it again if you've been following along!)

As useful as I hope this post and those that follow are, their core ideas are not novel by any means. They are drawn from and lie in the shadow of a brilliant and thorough treatment by John Hughes that has been circulated in one form or another since 1984. Hughes is one of the designers of the Haskell programming language. He was also the author of, “Why Functional Programming Matters." Hughes's article was my inspiration and the basis for this post. If you are hungry for more after reading this, I recommend that you read "Why Functional Programming Matters." You may also enjoy the presentations Hughes adapted from that text. John Hughes and his wife Mary Sheeran have keynoted several conferences with it between 2015 and 2017 that add even more context to the article's points. This post and any that follow in my Modularity in Functional Programming series seek to serve as a mere tour guide through the well-established shrine of their ideas. You can find a link to the article and one of the talks in the footnotes below.

In this post we continued following section 3 of "Why Functional Programming Matters.” Its name in the article “Gluing Functions Together.” My hope was to reduce some of the assumptions the original memorandum makes about an academic audience by replacing the more rigorous proofs in the memo with analogy and entry level definitions. I was still, however, hoping to highlight the benefit of applying these ideas in the way Hughes's paper lit a fire under me when I first read it. I also wanted it to be a more interactive, hands-on learning experience so I used executable Scala3 code instead of pseudocode and I will aim to do this with future articles.  

## Footnotes

[^1]: [listing for ImmutableList in the Scala 2.13 standard library docs](https://www.scala-lang.org/api/2.13.3/scala/collection/immutable/List.html#map[B](f:A=%3EB):List[B])
[^2]: A few resources to visit for pattern matching: first, the [docs on pattern matching from Scala's website](https://docs.scala-lang.org/tour/pattern-matching.html) and then [an article interviewing Martin Odersky on why pattern matching is an important feature](https://www.artima.com/articles/the-point-of-pattern-matching-in-scala)
[^3]: For more information about generic classes, check out this [official scala guide](https://docs.scala-lang.org/tour/generic-classes.html)
[^4]: Scala compiler's powerful type inference is able to infer from the integers we pass in for each `value` argument that we are making a `Tree[Int]` type tree without us having to indicate it. The compiler would also throw an exception if we were to mix in objects of other types as `value`s for some of these `Nodes`. I left out the type annotation from myTree which would have looked like `val myTree: Tree[Int]`, but it is good practice to include type annotations that are otherwise inferrable for humans reading your code except where you feel it is hurting readability.
[^5]: The requirement that there be case statements for every type that extends a sealed trait in Scala is termed an "exhaustive matching."
[^6]: if you opted to work through this before looking at the solution and were stumped, congratulations for putting in that extra effort! You can also take heart from this small anecdote: In Hughes's original publication of this memo, he actually had only one foldTree function to handle both of the foldTree sub-problems outlined in this post. That version as written would have required somehow handling the `Tree[A]` and `List[Tree[A]]` types as though they could be matched as the same type. Over time, due to repeated mentions by the community of readers of his memo that the pseudocode didn't translate into a solution in an actual language (Haskell in most cases), Hughes re-published this article so that `redTree` and `redTree'` were the two functions and they called each other in a very similar fashion to the solution I provided here. When writing this post, I also ended up stuck trying to figure out a way to solve this problem given the constraints of his pseudocode and was unable to adapt my code until I became aware of his updated publication. I posted a link to both the original and updated publication in the first footnote of my [first post](/2023/03/functional-modularity-1/#fn:1)
