---
title: "Modularity With Functional Programming Part 1"
date: 2023-03-29 00:00:00 -0500
excerpt: "Welcome to Modularity in Functional Programming! In this series, I will be addressing how using aspects of the functional paradigm can lead to program components that are easier to reuse and expand upon."
---

## Introduction To This Series

Welcome to Modularity in Functional Programming! In this series, I'll write about how to stop writing programs that only do one thing and instead write code in pieces that can be combined to do many things.

To kick the series off, I will do some posts working through the ideas covered in John Hughes's foundational memo "Why Functional Programming Matters."[^1] In each post, I'll share my thoughts on a portion of John's memo and provide runnable code examples in the Scala 3 programming language for an interactive way to engage with this material.

For this first post, we will talk about what Hughes referred to as "Gluing Programs Together." This refers to the idea of achieving reuse by gluing functions to other functions in different ways.

## What Do I Mean by Modular Design?  

Let us take a quick detour from programming and consider general examples of modular design. For example, if you have ever used a KitchenAid™ stand mixer, you will know that there is one base with a motor inside and a general-purpose connector that lets you attach gadgets for a range of tasks. From beating eggs to kneading dough and even coring, peeling, and slicing, you only need one base. There is no need to buy a separate appliance for each purpose.  

Consider another example of modular design -- the modern squat rack used by weightlifters. They are standard steel frames with various attachments that facilitate a variety of exercises. All the squat rack equipment fits in a 10x10 room in your home instead of paying that $30 a month gym membership fee.

The common theme of these examples is a mechanism to connect pieces together. As a result, you get use out of the pieces you already have instead of having to start over.  

## Modular Design and Functional Glue

Now that we are all picturing modularity in some form, we can move on to discuss what John Hughes meant by "gluing programs together." In the memo, Hughes brings this up when defending the unique value of programming in a functional way. He points out that everyone in the computer science community at some point agreed that modular programming efforts tended to be more likely to succeed than their single purpose counterparts, and so new languages of the time such as Ada and Modula-II "included features specifically designed to help improve modularity."

He explains that these other languages and the paradigms they rely on are chasing modularity by allowing organized subdivision of their programs, but that allowing for subdivision alone does not suffice. As he points out, for true modularity _when we break things apart we must already be thinking about how we are going to glue them back together._

Our little trip outside of the programming domain at the beginning of this read helps us take in the weight of Hughes's statement here. Of the modular inventions we discussed above, take the device which resonated most with you (or another one that you brought with you from your imagination!) Picture the older, non-modular version of that device (I'm using a stand mixer that only supports two egg beater attachments) and now imagine you try to 'modularize' it by just sawing it in half. Now you have the tough task of getting this invention to work for its original purpose, and also most likely your two halves cannot be recombined into any new useful thing either!

This focus on putting things back together with 'glue' is the benefit Hughes claims for the functional paradigm. Sure, functional programming comes with more constraints like not allowing values to be modified in place, but these constraints are all in service of breaking things down into general purpose pieces that can be safely recombined in different ways.[^2]

The two types of glue Hughes focuses on are higher-order functions and lazy evaluation. This post will introduce higher-order functions, which I plan to follow up with some more examples in a future post or two, and then we'll cover lazy evaluation a few entries down the road.

## Example 1: Modular Design in List Construction with Scala

If you would like to follow along, all you need is the Scala 3 tools installed on your machine. View instructions specific to your OS [here on the scala download page](https://www.scala-lang.org/download/). Once installed, fire up a read-eval-print-loop (REPL) for Scala 3 by typing ‘scala’ into your terminal.

The aspect of programming we'll focus on to show modularity will be constructing and manipulating a List data structure. We'll start with creating a list.

In Scala 3, creating a list has a clear, literal syntax:

```scala
    val myList = List(1, 2, 3)
```

But let us look deeper. This syntax is a convenient sugar to make the language simpler to write in. An equal way to build a list element by element in Scala 3 without the sugar is this:

```scala
    val myList = 1::2::3::Nil
```

Or, even more clear for our example we can use the prefix instead of infix notation and write it as  

```scala
    val myList = ::(1,(::(2,(::(3, Nil)))))
```

Constructing the list with this syntax is verbose and less readable, but it reveals that this double colon operator '::', which is typically referred to as 'cons', can be viewed as a function we are calling multiple times to make our list.[^3] What 'cons' does is prepend an item to a collection of items that match that item's type. The first argument is an object of some type, and the second argument is a list of that same type of object.

In the code examples above, we constructed a linked list by calling 'cons' in a nested fashion. The first argument holds an element of the list, and the second argument holds the rest of the nested invocations to cons that form the list. This continues until we get to the innermost 'cons' call, holding the last element of the list as its first argument and the Nil companion object as its second. Nil's whole purpose is to indicate that we are at the end of a list.

In our linked list example, we see how capturing the idea of prepending into a 'cons' function can build more complex functionality. Starting with the simple 2 argument function, we have formed a list structure. It does not have to stop there either. Small alterations to the 'cons' interface lead to other data structures like trees, tuples, and graphs.[^4]

How does building lists in this fashion compare to implementations of LinkedList you have seen in other languages or in data structures courses? A typical one I know of is building a list with 'Node' structures where each 'Node' has an element and a pointer to the address in memory of the next 'Node'.

A strict mechanical comparison between using 'cons' and the 'Node' approach does not appear at first blush to make 'cons' more than a novelty, a kind of distinction without a difference from the 'Node' approach. Seeing the recursive nature of 'cons' may even make it feel more complicated at first. It therefore helps to take a step back and look at the intent driving us to use a functional interface for the links as opposed to structures with pointers to each other's memory addresses. We will see that it ties back to characteristics of functional programming and the conversation about modularity.

By using functions to build the list and by always returning the increasingly larger chunks of list as new results instead of modifying an existing partial list, the 'cons' approach is doing what Hughes described and planning ahead to a future where lists will be deconstructed and then reconstructed into new forms. Every 'cons' invocation is like an entry point where we could make a change to what we did, with a different function, and it would change the overall behavior of what we did from just linking the elements of the list together to some other outcome. Also, since 'cons' and our modifications to it would not do anything other then return a new output, we do not have to worry that there would be any other impact on the list by making these changes.

To return to our ongoing analogy, whatever pre-modular device that you sawed in half, think about the foresight it took to instead break it apart in a clever way so that a door was left open for modularity (in my example of the stand mixer maybe someone changed out the two beaters for one drive shaft). A function provided a subdivided way to build our list, and as we will see next it is also a type of function that will give us the to power to reconstruct the list differently later.

 So functions serve as Hughes's first example of glue, but how? We'll show this with a class of functions common to functional programming known as higher-order functions. A higher order function is a function that may take another function as one or more of its arguments and may return a function as the result. In the next section, we introduce the higher order function foldRight and we will see how it glues together the idea of recursively substituting an operation in for 'cons' in our list to the concrete operations and base case values that allow this idea to generalize to many types of list accumulators.

## Example 2: Modular Design in List Operations

This time instead of constructing lists we will perform operations on them. We will use the same example as John Hughes's “Why Functional Programming Matters” of summing the items of a list. Hughes presents the summing procedure as a recursive pattern, or a pattern that calls itself until some base case condition is met.

The base case condition for the sum function is that it received an empty list as input, and for this it returns 0. The recursive case is that a list was passed in with one or more elements, and in this case it will set up nested stack frames that add an element to a call to itself without that item in the list until it exhausts the list, at which point it has reached the base case and so will add 0.

Below is first the recursive definition of sum described above.

```scala
// original function
def sum(list:List[Int]):Int = 
    if list.isEmpty then return 0
    else return list.head + sum(list.tail)
```

Now, here is a sense of the recursive pattern that sum creates. I started with the sum of a list containing 1, 2, and 3. On each next line, I expand any calls to 'sum' from the line above it with the expression that it would expand to and carry the rest down. All of these lines evaluate to 6 when run, so by looking at it we can see how the recursion process gradually builds up to the summation expression at the bottom

```scala
sum(List(1 ,2 , 3))
1 + sum(List(2, 3)) 
1 + 2 + sum(List(3)) 
1 + 2 + 3 + sum(List.empty) 
1 + 2 + 3 + 0 
```

Notice how this recursion is visiting each element of the list in the order that 'cons' put them together. We can show the same pattern but for a different operation, say product:

```scala
def product(list: List[Int]):Int =
    if list.isEmpty then return 1
    else return list.head + product(list.tail)
```

and the expansion:

```scala
product(List(1, 2, 3))
1 * product(List(2, 3))
1 * 2 * product(List(3))
1 * 2 * 3 * product(List.empty)
1 * 2 * 3 * 1
```

These examples require authoring an entirely new function each time, but the pattern of recursion to accumulate across the list elements is the same, except for the base case return value and the function that does the operation. So how do we break it up? Scala already provides functions for adding and multiplying

```scala
(1.+(2)).+(3) // + is a function in scala
(1.*(2)).*(3) // * is a function in scala
```

but the trickier part is that recursive pattern. It turns out scala has a function for that too in the function 'foldRight'. 'foldRight' is the conventional name in most functional languages for a higher order function that combines the elements of a collection using as input a function parameter representing the operation to combine with and a value parameter representing the value to use for an empty collection. FoldRight is so called because it starts with the last or rightmost 2 items and advancing backwards towards the front. Here is the listing for foldRight in the Scala 2.13 standard library as of this post:[^4]

```scala
    final override def foldRight[B](z: B)(op: (A, B) => B): B = {
        var acc = z
        var these: List[A] = reverse
        while (!these.isEmpty) {
        acc = op(these.head, acc)
        these = these.tail
        }
    acc
    }
```

Let us break this down.

1. FoldRight’s signature takes an argument 'z', which is the value to return in case of an empty list. It also takes a function 'op', that needs to take two arguments and return a result that has the same type as the first argument 'z'.  

2. We then create an accumulator variable 'acc' and a variable 'these'. 'Acc' will store the accumulated result. 'These' is the list we are calling foldRight on.  

3. We reverse 'these' so we process from the last element of the list to the first element.[^5]  

4. We then iterate over the 'these' list until it is empty, doing the following in each iteration:

5. apply the 'op' function with the new head of the list and update the accumulator.  

6. Move the 'these' pointer, effectively dropping the head element we just accumulated and making the list one shorter for next iteration.  

7. At the end, we return 'acc' which will be the result of applying the operation to all items in the list.  

FoldRight is a higher order function because it takes in function 'op' as an argument and uses 'op' in a way that influences its return value. This is the glue we've been searching for! With the higher order functions, we can scalpel our code in half instead of sawing it, pull out the concrete subroutines and values and leave behind a pattern that may have several uses outside of the way we were originally trying to apply it. In my stand mixer example, it would be like realizing that if we change the motor attachment then a whisk/egg beater was only one of many stirring actions we could support.

While you may not be able to bake delicious cakes with it, foldRight proves extremely versatile. Here are some examples of foldRight you can try in your Scala 3 REPL:

```scala
    // starting with the list you already made (1, 2, 3)

    // sum
    myList.foldRight(0)(_+_)
    // product
    myList.foldRight(1)(_*_)
    // make a copy
    myList.foldRight(List.empty)(_::_)
    // append 2 lists together
    val otherList = List(4, 5, 6)
    myList.foldRight(otherList)(_::_)
    //TODO map!
```

Notice how terse these definitions are and how they don’t require you to read through the detail of how the list is combined each time. FoldRight owns the repetitive functional form. We simply pass in the operation and base case result. The example list above is also far from exhaustive as far as things you can do with foldRight, but already they show four common operations being greatly simplified.

In "Why Functional Programming Matters," John Hughes discusses another way to view the relationship between what we have done in example 1 and example 2. We can view what foldRight does as passing through the list construction from example 1 and replacing 'cons' wherever it finds it with 'op' instead. It has effectively updated the list as an expression of the prepend function to the list as an expression of combining elements in terms of 'op' and 'z'. Taking that mindset could lead us to think of more functions that update what we do with the list.

Programming in this way is exciting in much the same way that designing for modularity is elsewhere. Finding ways to reuse foldRight in new contexts or produce other higher order functions becomes an exercise in creativity. I encourage you to try out more variations on your own or look for ways to work higher order functions like foldRight into your own coding projects.  

## Where Can I Learn More?

As useful as I hope this post and those that follow are, their core ideas are not novel by any means. They are drawn from and lie in the shadow of a brilliant and thorough treatment by John Hughes that has been circulated in one form or another since 1984. Hughes is one of the designers of the Haskell programming language. He was also the author of, “Why Functional Programming Matters." Hughes's article was my inspiration and the basis for this post. If you are hungry for more after reading this, I recommend that you read "Why Functional Programming Matters." You may also enjoy the presentations Hughes adapted from that text. John Hughes and his wife Mary Sheeran have keynoted several conferences with it between 2015 and 2017 that add even more context to the article's points. This post and any that follow in my Modularity in Functional Programming series seek to serve as a mere tour guide through the well-established shrine of their ideas. You can find a link to the article and one of the talks in the footnotes below.

In this post we looked at section 3 of "Why Functional Programming Matters.” Its name in the article “Gluing Programs Together.” My hope was to reduce some of the academic rigor of the original memorandum and still highlight the benefit of applying these ideas. I also wanted it to be a more interactive, hands-on learning experience so I used executable Scala3 code instead of pseudocode and I will aim to do this with future articles.  

Thanks for taking this journey with me. Questions? Comments? I would love to connect with you! Feel free to reach out via my social links.  

 Until Next Time... Or for you stand mixer/Portal fans out there, until we run out of cake!

## Footnotes

[^1]: A link to a pdf for “Why Functional Programming Matters” can be found under "Background Papers" on [this page](https://www.cs.kent.ac.uk/people/staff/dat/miranda/), and a link to one of the keynote addresses mentioned can be found [here](https://www.youtube.com/watch?v=IyR04U66z7E&ab_channel=gnbitcom)

[^2]: If your background is like mine, you might be wondering where object-oriented programming (OOP) fits into Hughes's case for modularity.
      Any discussion of modularity surely should include some mention of OOP, right? Coined by Alan Kay, object-oriented programming paradigm is one in which you write your program as a collection of objects, little models of concepts that achieve a program's goals by sending and receiving each other's messages.

      OOP programs promise modularity because you can write objects one time and reuse and recombine them in different ways, plus you can do it in a safe way since objects hide how they represent things internally from the rest of the program. 
      
      I think Hughes stayed silent on the topic of OOP modularity because it is an independent idea to his thesis about the importance of functional programs. Object-oriented and functional paradigms coexist just fine, and the modularity they provide is complimentary. In fact, Scala 3 which we are using for this post's hands on exercises is a great example of one that emphasizes object-oriented and functional paradigms working together. 
      
      If Hughes was warning readers off of any particular paradigm it would likely be the imperative paradigm, which by its nature breaks some of the constraints that make functional code effective. For a thorough treatment of imperative v. functional techniques, [John Backus's Turing Award Lecture](https://dl.acm.org/doi/10.1145/359576.359579) is a great starting place. 
      
      Imperative object-oriented code is what I learned in school and used for years, and my personal mileage with it is that you can be productive quickly but a functional approach can help prevent issues with shared mutable state and other pitfalls that are easy to fall into when you scale your programs.

[^3]: The operator’s name ‘cons’ in Scala is in homage to the cons cell data structure used as the basis for collections in many Lisp family languages. More information can be found [here](https://en.wikipedia.org/wiki/Cons#:~:text=In%20computer%20programming%2C%20cons%20(%2F,%2C%20or%20(cons)%20pairs.)

[^4]: Even though this article and the John Hughes memo will refer to 'cons' as though it were implemented entirely with functions, it usually isn't in practice. Scala uses a case class and factory method to build a list and hold it in memory, and it isn't alone. Lisp and other functional languages have opted for data structures because a truly stateless functional approach isn't as performant. For more on proving the cons interface can be implemented solely with functions, see the reference to Church Encoding [here](https://en.wikipedia.org/wiki/Cons)

I took this definition of foldRight for List directly from the Scala 2.13 standard library. This is also the immutable list class that Scala 3 uses, because Scala 3 uses the Scala 2.13 standard library. [here is the foldRight source listing from List.scala](https://github.com/Scala/Scala/blob/v2.13.3/src/library/Scala/collection/immutable/List.Scala#L79)

[^5]: Reverse is a function defined elsewhere in the same Immutable List class. Note that reverse only needs to be called on the list because this is an iterative style implementation of foldRight. In a recursive version of this method the innermost function would be invoked first, so the elements would be processed right to left already. Functional languages also provide a foldLeft in some cases, and the choice of which to use is often one of algorithmic efficiency, which is out of the scope of this article.  
