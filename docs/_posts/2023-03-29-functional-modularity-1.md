---
title: "Modularity With Functional Programming Part 1"
date: 2023-03-29 00:00:00 -0500
excerpt: "Welcome to Modularity in Functional Programming! In this series, I will be addressing how using aspects of the functional paradigm can lead to program components that are easier to reuse and expand upon."
---

## Introduction To This Series

Welcome to Modularity in Functional Programming! In this series, I will be addressing how using aspects of the functional paradigm can lead to program components that are easier to reuse and expand upon. I will be working through some of the ideas covered in John Hughes's foundational memo "Why Functional Programming Matters."1 I will provide my own take on how to relate to modularity in functional programming and give you some runnable code examples in the Scala 3 programming language so that you can experiment with these powerful tools yourself. For this first post, we will talk about what Hughes referred to as "Gluing Programs Together." This refers to the idea of achieving reuse by gluing functions to other functions or inputs to achieve more reuse in programs.

## What Do I Mean by Modular Design?  

Forget about software development for a minute and consider general examples of modular design. For example, if you have ever used a KitchenAid stand mixer, you will know that there is one base with a motor inside and a general-purpose connector that lets you attach gadgets for a range of tasks. From beating eggs to kneading dough and even coring, peeling, and slicing, you only need one base. There is no need to buy a separate appliance for each purpose.  

Consider another example of modular design -- the modern squat rack used by weightlifters. They are standard steel frames with various attachments that facilitate a variety of exercises. All the squat rack equipment fits in a 10x10 room in your home instead of paying that $30 a month gym membership fee.

LEGO® blocks are also often used as a tangible modularity example.2 There seems to be a LEGO® set now for every intellectual property under the sun. But you can still attach your Harry Potter Set to a bucket you found in your parents or grandparents' closet from the 1960s. You can mix and match these sets to create original pieces. Their connectors all work the same way. You can connect the blocks with their larger DUPLO® siblings. You can build a house out of a set meant for a spaceship or a boat out of pieces intended for a castle.  

The common theme of these examples is a mechanism to connect pieces together. As a result, you get use out of the pieces you already have instead of having to start over.  

Let us circle back to programming. College and popular texts taught me software modularity through the imperative, object-oriented lens. The popular languages for education were Java, C++, and C#. These languages did not have much support for the functional paradigm at the time, so I simply was not exposed to the functional paradigm and its approaches.  

Imperative, Object Oriented languages have tools to achieve modularity, often based on supplying classes for user defined types. While many of these languages now make it easy to use higher order functions and other features we will cover in this series, they have historically tended to use object-oriented features such as composition, inheritance, and polymorphism to achieve behavior reuse. This series will cover functional techniques for achieving modular design that do not diminish object-oriented features but instead provide a powerful set of tools to combine with them.

Functional modularity is sometimes a better choice for flexibility and extensibility than object-oriented modularity. Functional programming tends to not require classes to achieve reuse and it enforces adherence to constraints like immutability that make units of code more predictable. This post will show how with functional techniques you can construct powerful, abstract building blocks out of composed function behaviors.  

## Example 1: Modular Design in List Construction with Scala

If you would like to follow along, all you need is the Scala3 tools installed on your machine. View instructions specific to your OS here on the scala download page. Once installed, fire up a read-eval-print-loop (REPL) for Scala3 by typing ‘Scala’ into your terminal.

The first example we have is a modular pattern for performing operations on lists.  

The basis for our example will be constructing and manipulating a List data structure. In Scala 3, creating a list has a clear, literal syntax:

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

Constructing the list with this syntax is verbose and less readable, but it reveals that this double colon operator, which is typically referred to as 'cons', can be viewed as a function we are calling multiple times to make our list.3 What 'cons' does is build an ordered pair out of its first and second arguments. The first argument is an object of some type, and the second argument is a list of that same type of object. In the code examples above, we constructed a linked list by calling 'cons' in a nested fashion. The first argument holds an element of the list, and the second argument holds a reference to the rest of the list. This continues until we get to the innermost 'cons' call, holding the last element of the list as its first argument and the Nil companion object as its second to terminate the list.  

In our linked list example, we see how some higher-level functions can build more complex functionality. Starting with a simple 2 argument function, we can build a complicated list structure. It does not have to stop there either. Alternate invocation patterns using 'cons' lead to other data structures like trees, tuples, and graphs.  

Technically, Scala is using a case class instance rather than a function to be the cons operator. Using a case class supports deconstructing of lists when pattern matching. Scala also creates these case class instances using a method on the List class instead of having a standalone function. Having a method on List to build the 'cons' objects supports having that infix notation for building lists you see in the syntax of the second example above. Lisp varieties of functional languages also use data structures rather than just functions in their implementation of the 'cons' interface, because it is more efficient to do so even though it is possible to implement 'cons' with just functions and no data structures or user defined types.4 Despite these implementation details, what is important for our purposes is that 'cons' is a functional interface that, when provided an element to prepend and a list to prepend it to, will always return a new list with that prepended element at its head.

How does building lists in this fashion compare to implementations of LinkedList you have seen in other languages or in data structures courses? A typical one I know of is building a list with 'Node' objects where each 'Node' has an element and a pointer to another 'Node'. A strict mechanical comparison between using 'cons' and the 'Node' approach does not appear to make this approach more than a novelty, a kind of distinction without a difference. Seeing the recursive nature of 'cons' may even make this approach feel more complicated at first. It therefore helps to take a step back and look at the intent driving us to use a functional interface for the linkages.  

What this approach is doing is introducing a new way of thinking about a list. This construction shows the notion of lists as data elements associated together by functional transformations. 'Cons' can be thought of as the basic linking function that group the list elements and order them. These 'cons' transformations in the above example do not affect the element or list used in prior calls to cons. Instead with each invocation they generate a new, larger-by-one list.  

What could this new way of thinking about lists lead to? With the knowledge that we create a list by repeatedly calling a function, we can now reason about how to operate on lists in the same way. If we can think of another function that could have generated the list but does one or more added transformations on top of just connecting the elements, then it could stand in the place of 'cons'. Chaining this new function across the elements that 'cons' ordered for us, we could create a whole other list. We need only design what we want the function to do between any two elements, and then compose it with 'cons' to apply the new transformation to any existing list. Additionally, since these transformations result in new return values each time instead of changing an object in place, the original list we made with ‘cons’ stays unaltered and furthermore could always be reproduced from the original elements by using 'cons' on them again.

The class of functions that we can use to compose these extra transformations are known as higher order functions. A higher order function is a function that may take another function as one or more of its arguments and may return a function as the result. In the next section, we introduce the higher order function foldRight that will run over the elements like 'cons' did, but it will also capture the abstract idea of accumulating list elements so that we can apply that accumulation in diverse ways.  

## Example 2: Modular Design in List Operations

This time instead of constructing lists we will perform operations on them. We will use the same example as John Hughes's “Why Functional Programming Matters” of summing the items of a list. Hughes presents the summing procedure as a recursive pattern. The base case is an empty list which returns zero. The recursive case adds the last element of the list to the result of repeatedly applying the sum function to the rest of the list.  

Here we have our main insight. There is an extractable, reusable pattern here. “Return the result of applying [some operation] to the last element of a list and another call to this function on the rest of the list until you reach the base case of an empty list then return [some operation] applied to the accumulated result and [some value]." In the case of sum, the [some operation] function happens to be addition. The empty case's [some value] also happens to return zero. These two concrete values for [some operation] and [some value] make this accumulator what it is. But we could have used a different operator and base case return value to get a different accumulator. As we will see in one of the examples below, using 1 and the * operator would yield the product of every item in the list instead.  

You can encapsulate this accumulation pattern of combining list elements into a higher order function. When you combine the list starting with the last or rightmost 2 items and advancing backwards towards the front, the conventional name for this function is 'fold right' (or foldRight as we will refer to it as Scala adopts CamelCase for methods). FoldRight has broad applicability. It is often made available in collections in functional language libraries. Here is the listing for foldRight in the Scala 2.13 standard library as of this post:5

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

FoldRight’s signature takes an argument 'z', which is the value to return in case of an empty list. It also takes a function 'op', that needs to take two arguments and return a result that has the same type as the first argument 'z'.  

We then create an accumulator variable 'acc' and a variable 'these'. 'Acc' will store the accumulated result. 'These' is the list we are calling foldRight on.  

We reverse 'these' so we process from the last element of the list to the first element.6  

We then iterate over the 'these' list until it is empty, doing the following in each iteration:

apply the 'op' function with the new head of the list and update the accumulator.  

Move the 'these' pointer, effectively dropping the head element we just accumulated and making the list one shorter for next iteration.  

At the end, we return 'acc' which will be the result of applying the operation to all items in the list.  

FoldRight is a higher order function because it takes in function 'op' as an argument and uses 'op' in a way that influences its return value. This gives us that powerful modularity we have been looking for. Going back to the Kitchen Aid example, foldRight would be like the base unit with motors. Using it for different operations would be like using different sets of attachments. Sum could be a special cheese grater and product a freezable bowl mixer combo for making ice cream. Copy could be like adding a special stirrer for beating cake batter. Mmm, cake.  

While you may not be able to bake delicious cakes with it, foldRight is versatile enough to do a lot of what you might want to by combining list elements. Here are some examples of foldRight you can try in your Scala 3 REPL:

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
```

Notice how terse these definitions are and how they don’t require you to read through the detail of how the list is combined each time. FoldRight owns the repetitive functional form. We simply pass in the operation and base case result. The example list above is also far from exhaustive as far as things you can do with foldRight, but already they show four common operations being greatly simplified.

In "Why Functional Programming Matters," John Hughes discusses another way to view the relationship between what we have done in example 1 and example 2. We can view what foldRight does as passing through the list construction from example 1 and replacing 'cons' wherever it finds it with 'op' instead. It has effectively updated the list as an expression of the prepend function to the list as an expression of combining elements in terms of 'op' and 'z'. Taking that mindset could lead us to think of more functions that update what we do with the list.

Programming in this way is exciting in much the same way that building with Legos is. Finding ways to reuse foldRight in new contexts or produce other higher order functions becomes an exercise in creativity. I encourage you to try out more variations on your own or look for ways to work higher order functions like foldRight into your own coding projects.  

Where Can I Learn More?

As useful as I hope this post and those that follow are, their core ideas are not novel by any means. They are drawn from and lie in the shadow of a brilliant and thorough treatment by John Hughes that has been circulated in one form or another since 1984. Hughes is one of the designers of the Haskell programming language. He was also the author of, “Why Functional Programming Matters." Hughes's article was my inspiration and the basis for this post. If you are hungry for more after reading this, I recommend that you read "Why Functional Programming Matters." You may also enjoy the presentations Hughes adapted from that text. John Hughes and his wife Mary Sheeran have keynoted several conferences with it between 2015 and 2017 that add even more context to the article's points. This post and any that follow in my Modularity in Functional Programming series seek to serve as a mere tour guide through the well-established shrine of their ideas. You can find a link to the article and one of the talks in the footnotes below.

In this post we looked at section 3 of "Why Functional Programming Matters.” Its name in the article “Gluing Programs Together.” My hope was to reduce some of the academic rigor of the original memorandum and still highlight the benefit of applying these ideas. I also wanted it to be a more interactive, hands-on learning experience so I used executable Scala3 code instead of pseudocode and I will aim to do this with future articles.  

Thanks for taking this journey with me. Questions? Comments? I would love to connect with you! Feel free to reach out via my social links.  

 Until Next Time... Or for you stand mixer/Portal fans out there, until we run out of cake!

## Footnotes

1 A link to a pdf for “Why Functional Programming Matters” can be found under "Background Papers" on this page, and a link to one of the keynote addresses mentioned can be found here

2 LEGO® and DUPLO® are trademarks of the LEGO Group of companies which does not sponsor, authorize, or endorse this site

3The operator’s name ‘cons’ in Scala is in homage to the cons cell data structure used as the basis for collections in many Lisp family languages. More information can be found here

4 For more on proving the cons interface can be implemented solely with functions, see the reference to Church Encoding here:

5 I took this definition of foldRight for List directly from the Scala 2.13 standard library. This is also the immutable list class that Scala 3 uses, because Scala 3 uses the Scala 2.13 standard library. here is the foldRight source listing from List.scala

6 Reverse is a function defined elsewhere in the same Immutable List class. Note that reverse only needs to be called on the list because this is an iterative style implementation of foldRight. In a recursive version of this method the innermost function would be invoked first, so the elements would be processed right to left already. Functional languages also provide a foldLeft in some cases, and the choice of which to use is often one of algorithmic efficiency, which is out of the scope of this article.
