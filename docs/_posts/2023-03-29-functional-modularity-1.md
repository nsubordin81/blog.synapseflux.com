---
title: "Modularity With Functional Programming Part 1"
date: 2023-03-29 00:00:00 -0500
excerpt: "Welcome to Modularity in Functional Programming! In this series, I will be addressing how using aspects of the functional paradigm can lead to program components that are easier to reuse and expand upon."
classes: wide
---

## Introduction To This Series

Welcome to Modularity in Functional Programming! In this series, I'll write about how to write programs out of pieces that can do many different things and move away from writing a new program for every single purpose.

To kick the series off, I will do some posts working through the ideas covered in John Hughes's foundational memo "Why Functional Programming Matters."[^1] In each post, I'll share my thoughts on a portion of John's memo and provide runnable code examples in the Scala 3 programming language that you can try.

For this first post, we will talk about what Hughes referred to as "Gluing Functions Together." This refers to the idea of achieving reuse by gluing functions to other functions in different ways. To skip over the introduction and get right to the examples, [click here](#example-1-modular-design-in-list-construction-with-scala).

## What Do I Mean by Modular Design?  

Let us take a quick detour from programming and consider general examples of modular design. For example, if you have ever used a KitchenAid™ stand mixer, you will know that there is one base with a motor inside and a general-purpose connector that lets you attach gadgets for a range of tasks. From beating eggs to kneading dough and even coring, peeling, and slicing, you only need one base. There is no need to buy a separate appliance for each purpose.  

Consider another example of modular design -- the modern squat rack used by weightlifters. They are standard steel frames with various attachments that facilitate a variety of exercises. All the squat rack equipment fits in a 10x10 room in your home instead of paying that $30 a month gym membership fee.

The common theme of these examples is a mechanism to connect pieces together. As a result, you get use out of the pieces you already have instead of having to start over.  

## Modular Design and Functional Glue

Now that we are all picturing modularity in some form, we can move on to discuss what John Hughes meant by "gluing functions together." In the memo, Hughes brings this up when defending the unique value of programming in a functional way. He points out that everyone in the computer science community at some point agreed that modular programming efforts tended to be more likely to succeed than their single purpose counterparts, and so new languages of the time such as Ada and Modula-II "included features specifically designed to help improve modularity."

He explains that these other languages and the paradigms they rely on are chasing modularity by allowing organized subdivision of their programs. However, allowing for subdivision alone does not suffice to achieve modularity. As he points out, for true modularity _when we break things apart we must already be thinking about how we are going to glue them back together._

Our little trip outside of the programming domain at the beginning of this read gives us a visual way to take in the weight of this assertion by Hughes on the nature of modularity. Make a mental picture of one of the modular inventions we discussed above (or another one that you brought with you from your imagination!) Picture the older, non-modular version of that device (I'm using a stand mixer that only supports two egg beater attachments), and now imagine you try to 'modularize' it by just sawing it in half. Now you have the tough task of getting this invention to work for its original purpose, and also most likely your two halves cannot be recombined into any new, useful thing either!

Hughes claims the functional paradigm's edge against other languages is that programming in a functional way makes it easier to put things back together with 'glue'. Sure, functional programming comes with more constraints like not allowing values to be modified in place, but these constraints are all in service of breaking things down into general purpose pieces that can be safely recombined in different ways.[^2]

The two types of glue Hughes focuses on are higher-order functions and lazy evaluation. This post will introduce higher-order functions, which I plan to follow up with some more examples in a future post or two, and then we'll cover lazy evaluation a few entries down the road.

## Example 1: Modular Design in List Construction with Scala

If you would like to follow along, all you need is the Scala 3 tools installed on your machine. View instructions specific to your OS [here on the scala download page](https://www.scala-lang.org/download/). Once installed, fire up a read-eval-print-loop (REPL) for Scala 3 by typing `scala` into your terminal.

The aspect of programming we'll focus on to show modularity will be constructing and manipulating a List data structure. We'll start with creating a list.

In Scala 3, creating a list has a clear, literal syntax:

```scala
val myList = List(1, 2, 3)
```

But let us look deeper. This syntax is a convenience to make the language simpler to write in. These types of niceties in programming languages are often playfully dubbed 'sugars'. An equal way to build a list element-by-element in Scala 3 without the sugar is this:

```scala
val myList = 1::2::3::Nil
```

Or, even more clear for our example we can use the prefix instead of infix notation and write it as  

```scala
val myList = ::(1,(::(2,(::(3, Nil)))))
```

Constructing the list with this syntax is verbose and less readable, but it reveals that this double colon operator `::`, which is typically referred to as `cons`, can be viewed as a function we are calling multiple times to make our list.[^3] What `cons` does is prepend an item to a collection of other items that match that item's type. The first argument is an object of some type, and the second argument is a list of that same type of object.

In the code examples above, we constructed a linked list by calling `cons` in a nested fashion. The first argument holds an element of the list, and the second argument holds the rest of the nested invocations to cons that form the list. This continues until we get to the innermost `cons` call, holding the last element of the list as its first argument and an instance of the `Nil` companion object as its second. `Nil`'s whole purpose is to indicate that we are at the end of a list.

In our linked list example, we see how capturing the idea of prepending into a `cons` function can build more complex functionality. Starting with the simple 2 argument `cons` function, we have formed a list structure. It does not have to stop there either. Small alterations to the `cons` interface like an additional arguments for other collections can lead to building other data structures like trees, tuples, and graphs.[^4]

This approach to building list does a few things worth noting for our conversation about modularity. First, it uses a simple function in `cons` that represents the sub-problem of adding one item to a list as the only tool to build the list. Second, the calls to `cons` always return the increasingly larger chunks of list as a new result instead of modifying some existing partial list that lives outside the function.

Following rules like this is how functional approaches do what Hughes described and plan ahead for "gluing back together" by combining behaviors that operate on the data. Every `cons` invocation is like an entry point where we could make a change to what we did, with a different function, and it would change the overall behavior from just linking the elements of the list together to some other outcome. Also, since `cons` and future functions would not do anything outside of their scope, we do not have to worry that there would be any other impact on the original list or the new data we create other than what we intended.

To return to our ongoing analogy, whatever pre-modular device that you sawed in half, think about the foresight it took to instead break it apart in a clever way so that a door was left open for modularity (in my example of the stand mixer maybe someone changed out the insertion point for the two beaters for a drive shaft with a universal attachment). Following the principles of functional programming, we have laid the groundwork for future modularity in a similar way.

So functions serve as Hughes's first example of glue, but how? Let's find out with a class of functions common to functional programming known as higher-order functions. A higher order function is a function that may take another function as one or more of its arguments and may return a function as the result. In the next section, we introduce the higher order function foldRight, and we will see how it glues operations on lists with a recursive pattern that performs this operation between all list elements and accumulates a result.

## Example 2: Modular Design in List Operations

This time instead of constructing lists we will perform operations on them. We will use the same example as John Hughes's "Why Functional Programming Matters" of summing the items of a list. Hughes presents the summing procedure as a recursive pattern, or a pattern that calls itself until some base case condition is met.

The base case condition for the sum function is that it received an empty list as input, and for this it returns 0. The recursive case is that a list was passed in with one or more elements, and in this case the runtime interpreter will trace through the recursive function waiting until it reaches a base case, and as it hits invocations of itself it will add references to these invocations (called stack frames) to a first-in-last-out (FILO) structure called a call stack. Once the runtime interpreter exhausts the input, it will hit the return statement in the base case and it will know it can pop the stack frames off one after another and evaluate them.

Below is first the recursive definition of sum described above.

```scala
// original function
def sum(list:List[Int]):Int = 
    if list.isEmpty then return 0
    else return list.head + sum(list.tail)
```

Now, here is a sense of the recursive pattern that sum creates:

```scala
//order that recursive calls are encountered
sum(List(1 ,2 , 3)) // push a stack frame with list = List(1, 2, 3), suspend execution at 1 + ...
1 + sum(List(2, 3)) // push a stack frame with list = List(2,3), suspend at 1 + 2 + ...
1 + 2 + sum(List(3)) //push a stack frame with list = List(3), suspend at 1 + 2 + 3 + ...
1 + 2 + 3 + sum(List.empty) // hit base case, found return statement
1 + 2 + 3 + 0 // pop each of the stackframes off the top of the call stack 
              // and resume execution, adding 3+0 first, then 2+3, then 1+5
```

If you run the lines below in the scala interpreter, you'll see that they all return 6. I'm showing that each line is equivalent but in each successive line we are seeing the recursive call we'd be saving to the stack after pulling out the list.head and addition operation portion of the function that would be preserved with each call. The comments describe what the runtime would actually be doing as it evaluated this function and updated the call stack.

We can show the same pattern but for a different operation, say product:

```scala
def product(list: List[Int]):Int =
    if list.isEmpty then return 1
    else return list.head + product(list.tail)
```

and the expansion (see if you can work out what the runtime would be doing based on the above sum example):

```scala
product(List(1, 2, 3))
1 * product(List(2, 3))
1 * 2 * product(List(3))
1 * 2 * 3 * product(List.empty)
1 * 2 * 3 * 1
```

These examples require authoring an entirely new function each time, but the pattern of recursion to accumulate across the list elements is the same, except for the base case return value and the function that does the operation. So how do we break it up? Scala already provides functions for adding and multiplying:

```scala
(1.+(2)).+(3) // + is a function in scala
(1.*(2)).*(3) // * is a function in scala
```

but the trickier part is that recursive pattern. It turns out scala has a function for that too in the function `foldRight`. Folding is a conventional name in most functional languages for a class of higher order functions that combine the elements of a collection. `foldRight` is so called because it starts with the last or rightmost 2 items in the collection and processes backwards towards the initial elements on left side. Since we have been showing recursive examples, here is a recursive implementation of a standalone foldRight that accepts the list to be folded as a parameter:

```scala
def foldRight[A, B](z:B)(f:(A,B) => B)(l:List[A]): B = l match
    case Nil => z
    case head::tail => f(head, foldRight(z)(f)(l.tail))
```

Now you can represent both sum and product with the same `foldRight` function, by passing different values for the base case and function to be performed during the fold. here are examples of that:

```scala
foldRight(0)((a: Int, b: Int) => a + b)(List(1, 2, 3)) // res: 6
foldRight(1)((a: Int, b: Int) => a * b)(List(1, 2, 3)) // res 6
```

Let's quickly do a breakdown of this function.

1. We are dealing with two generic types, `A` and `B`, where `A` represents the type of the objects in the incoming list and `B` represents the type of the result foldRight returns.
2. We have 3 parameters. `z` of type `B`, the value to return for an empty list; `f` is the function representing the operation we perform between each pair of list elements; and `l` the original list passed in.
3. The base case is we have `Nil`, where `l` has no elements. In this case we return `z`
4. In the recursive case, we pattern match on the list using the `cons` operator with `head::tail`. Then we invoke our `f` function, but before evaluating these calls to `f` we are doing a recursive call to `foldRight` and building up the call stack until we reach the `Nil` case. The end result is a nested series of calls to `f` that will be popped off the call stack, un-suspended, and evaluated, looking something like if we were to do

`f(a(0), f(a(1), ... f(a(n-1), f(a(n), z))))`.

The above example using the recursive form matches closely to our definitions of sum and product and also to the version Hughes uses in his memo. Back in our first example with `cons`, we spoke about how functional style was giving us a way to break a solution down while leaving the door open for interesting ways to glue it back together. This foldRight example puts that on display. With this implementation the pattern match shows that foldRight is specifically looking for that `cons` link between elements, and then it is combining a new function `f` in place of `cons` to accumulate a value based on that function's evaluation. Since `foldRight` is a higher order function, `f` doesn't to be a specific function, we can do this combining with any behavior we like.

We got there! We've built the programming version of our multi-purpose kitchen appliance, our all-in-one home gym apparatus, the printed circuit board in the computer I'm using to write this article, etc. -- all with some adherence to functional constraints and a function that can accept another function as a parameter. With the higher order functions, we can scalpel our code in half instead of sawing it, pull out the concrete subroutines and values and leave behind a pattern that may have several uses outside of the way we were originally trying to apply it. In the stand mixer example, it would be like realizing that if we change the motor attachment then a whisk/egg beater was only one of many stirring actions we could support.

While you may not be able to bake delicious cakes with it, `foldRight` proves extremely versatile. Here are some examples of `foldRight` you can try in your Scala 3 REPL, using the List implementation of foldRight from Scala's standard library[^5]:

```scala
val myList = List(1, 2, 3)

// sum
myList.foldRight(0)(_+_)

// product
myList.foldRight(1)(_*_)

// make a copy
myList.foldRight(List.empty)(_::_)

// append 2 lists together
val otherList = List(4, 5, 6)
myList.foldRight(otherList)(_::_)

// compose this double function with the copy function above 
// to demonstrate applying a function to every element (map)
def double(x:Int):Int = 2*x
myList.foldRight(List.empty)(double(_)::_)
```

Notice how terse these definitions are and how they don't require you to read through the detail of how the list is combined each time. `foldRight` owns the repetitive functional form. We simply pass in the operation and what to return when we encounter the base case of an empty list. The examples above are also far from exhaustive in terms of things you can do with foldRight, but already they show several common operations being greatly simplified.

Programming in this way is exciting in much the same way that designing for modularity is elsewhere. Finding ways to reuse `foldRight` in new contexts or produce other higher order functions becomes an exercise in creativity. I encourage you to try out more variations on your own or look for ways to work higher order functions like `foldRight` into your own coding projects.  

## Where Can I Learn More?

As useful as I hope this post and those that follow are, their core ideas are not novel by any means. They are drawn from and lie in the shadow of a brilliant and thorough treatment by John Hughes that has been circulated in one form or another since 1984. Hughes is one of the designers of the Haskell programming language. He was also the author of, "Why Functional Programming Matters." Hughes's article was my inspiration and the basis for this post. If you are hungry for more after reading this, I recommend that you read "Why Functional Programming Matters." You may also enjoy the presentations Hughes adapted from that text. John Hughes and his wife Mary Sheeran have keynoted several conferences with it between 2015 and 2017 that add even more context to the article's points. This post and any that follow in my Modularity in Functional Programming series seek to serve as a mere tour guide through the well-established shrine of their ideas. You can find a link to the article and one of the talks in the footnotes below.

In this post we looked at section 3 of "Why Functional Programming Matters." Its name in the article "Gluing Functions Together." My hope was to reduce some of the assumptions the original memorandum makes about an academic audience by replacing the more rigorous proofs in the memo with analogy and entry level definitions. I was still, however, hoping to highlight the benefit of applying these ideas in the way Hughes's paper lit a fire under me when I first read it. I also wanted it to be a more interactive, hands-on learning experience so I used executable Scala3 code instead of pseudocode and I will aim to do this with future articles.  

Thanks for taking this journey with me. Questions? Comments? I would love to connect with you! Feel free to reach out via my social links.  

 Until Next Time... Or for you stand mixer/Portal fans out there, until we run out of cake!

## Footnotes

[^1]: A link to a pdf of the original publication of "Why Functional Programming Matters" can be found under "Background Papers" on [this page](https://www.cs.kent.ac.uk/people/staff/dat/miranda/). However, This version contains some errors that were fixed in a later publication which can be found instead on [this page](https://www.cse.chalmers.se/~rjmh/Papers/whyfp.html) and a link to one of the keynote addresses mentioned can be found [here](https://www.youtube.com/watch?v=IyR04U66z7E&ab_channel=gnbitcom)

[^2]: If your background is like mine, you might be wondering where object-oriented programming (OOP) fits into Hughes's case for modularity.
      Any discussion of modularity surely should include some mention of OOP, right? Coined by Alan Kay, object-oriented programming paradigm is one in which you write your program as a collection of objects, little models of concepts that achieve a program's goals by sending and receiving each other's messages.

      OOP programs promise modularity because you can write objects one time and reuse and recombine them in different ways, plus you can do it in a safe way since objects hide how they represent things internally from the rest of the program. 
      
      I think Hughes stayed silent on the topic of OOP modularity because it is an independent idea to his thesis about the importance of functional programs. Object-oriented and functional paradigms coexist just fine, and the modularity they provide is complimentary. In fact, Scala 3 which we are using for this post's hands on exercises is a great example of one that emphasizes object-oriented and functional paradigms working together. 
      
      If Hughes was warning readers off of any particular paradigm it would likely be the imperative paradigm, which many advocates of the functional style believe is only appropriate in circumstances where the functional approach can't be used, or when it complicates code or hurts its runtime performance. For a thorough treatment of functional as it compares to imperative coding in an ideological sense, one of the great examinations of this topic can be found in [John Backus's Turing Award Lecture](https://dl.acm.org/doi/10.1145/359576.359579). Martin Odersky, the original creator of Scala, has historically taken a more even-handed viewpoint, and in the [keynote to the latest scalacon 2022](https://www.youtube.com/watch?v=QRcD9Zc7eq4&t=2164s&ab_channel=ScalaCon), he expressed the importance of not treating imperative and functional styles as "two absolutes" but instead as a "spectrum", where depending on the context some amount of both paradigms is warranted.
      
      Imperative object-oriented code is what I learned in school and used for years, and my personal mileage with it is that you can be productive quickly but a functional approach can help prevent issues with shared mutable state and other pitfalls that are easy to fall into when you scale your programs.

[^3]: The operator's name `cons` in Scala is in homage to the cons cell data structure used as the basis for collections in many Lisp family languages. More information can be found [here][conslink]

[^4]: Even though this article and the John Hughes memo will refer to `cons` as though it were implemented entirely with functions, it usually isn't in practice. Scala uses a case class and factory method to build a list and hold it in memory, and it isn't alone. Lisp and other functional languages have opted for data structures because a truly stateless functional approach isn't as performant. For more on proving the `cons` interface can be implemented solely with functions, see the reference to Church Encoding [here](https://en.wikipedia.org/wiki/Cons)

[^5]: [here is the foldRight source listing from List.scala](https://github.com/Scala/Scala/blob/v2.13.3/src/library/Scala/collection/immutable/List.Scala#L79) Even though this listing is from 2.13, this is also the immutable list class that Scala 3 uses, because Scala 3 uses the Scala 2.13 standard library. If you read the listing, you may be surprised to find that foldRight is implemented in an imperative fashion in the ImmutableList class, using a while loop control structure. Because the while loop would be applying the function from left to right, the list must be reversed for foldRight to ensure elements are processed in right to left order. While I don't know the reason for sure, I'd wager the decision to use this imperative approach is that foldRight in its easiest to read recursive form is not tail-recursive and can lead to stack overflow errors. If you'd like to learn more about this, [here is an article that covers it in depth](https://oldfashionedsoftware.com/2009/07/10/scala-code-review-foldleft-and-foldright/)

[conslink]: <https://en.wikipedia.org/wiki/Cons#:~:text=In%20computer%20programming%2C%20cons%20(%2F,%2C%20or%20(cons)%20pairs.>
