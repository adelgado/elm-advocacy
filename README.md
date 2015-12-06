# Elm advocacy

## What is Elm?

Elm is a web frontend programming language that compiles to JavaScript and HTML.
It has been influenced in its design by languages like Haskell, Rust or OCaml.
Additionally, there is something known as the Elm Architecture, which
is a set of patterns and divisions concerning how to organize and structure
the flow of information in a specific application.

## Why Elm?

Elm is interesting for frontend developers because, by its characteristics,
it aims to solve many of the problems encountered today when developing
complex JavaScript applications, such as unknown breakage of code, brittleness
when refactoring – which sometimes leads to an unwillingness to do so –,
uncaught exceptions and exceptions in general, a strong need for defensive
programming that – which derives one from the main focus of providing value
through features to the end users, and could be alleviated at the language level –,
among other things.

Here are some of the characteristics Elm has, which help us fighting these problems:
Elm is a functional, reactive, strongly typed, immutable programming language.

**Functional** means that there are no side-effects. The only state you have
access to are the parameters of your function and the values of its closure.
The only way you can affect state is by returning a value. Functions can call
other functions, and these rules still apply, in a way that the flow of
information in the tree of function calls is very clear. Because functions have
no side effect, every time you call a function with the same arguments it is
guaranteed to do the same thing and return the same value. These are a breeze
to test! :-)

**Reactive** basically means that, in one way or another, there are ways to deal
with the passing of time, as a fundamental concept. Elm has a powerful abstraction
called a Signal, which represents a value that changes over time. It always has
a starting value and a current value, and you generally use the vocabulary
of sequences to visualize it. In the same way that a sequence of values can be
mapped on, or folded over, these are the operations you generally perform on a signal.

In the same way that there is `List.map`, there is `Signal.map`; in the same way that
`List.foldr` exists, fold from the right, also known as reduce, there is `Signal.folp`,
meaning fold from the past :-) It's a very interesting abstraction and the base of
reactivity in Elm.

The fact that Elm is **strongly typed** may throw off people who have in a past life
dealt with compilers that seem to unnecessarily get in the way of what they
were *actually* trying to achieve, and felt that the compiler wasn't really their
friend, and more like something to be beat. Languages like C++, Delphi, Java...

The reality is that these experiences reflect an approach to typing that is completely
different from the approach functional languages from the family that Elm comes from
traditionally take. Whereas in a typed OO language there is generally a need
to declare what type something is, in Elm everything is implicitely typed: the
compiler performs *type inference*, which is to observe the flow of information
in the program and derive from this the types that functions or values can have.

This means that this snippet of code already knows that its input type is a
single variable that's an integer, since you're adding it to 2, and that its
return type is also an integer.

If you try to call this with a string, `plusTwo "hello"`, the compiler will
warn you of a type mismatch. We're getting this for free because of the type
inference.

Additionally, any value or function can be given type annotations. This means
that while type annotations – but not types – are fully optional, you can still
explicitely document them in order to ease the understanding of the code and
hint the compiler to the types you actually mean.

The function we were discussing is understood by the compiler as if it'd
been defined and annotated as

```
plusTwo : Int -> Int
plusTwo =
  n + 2
```

While type inference in itself is already nice, the type system that Elm provides
is fundamentally difference from the one JavaScript provides, due to the fact
that it offers parametric types. If you know Generics from C#/Java or templates
from C++, you're kinda already familiar with the idea of parametric types, but
Elm's execution of it brings us much more.

The most common example is the `Maybe` type. The `Maybe` type takes another
type as an argument (let's say `String`), and its instances will hold one
of two possible values at any time: Either the value `Nothing`, or a value
of type `Just String`.

```
frob : Maybe String -> Foo
frob x = 
  case x of
    Nothing ->
      -- do something here
    Just x' ->
      -- do something else
```

Now, this is cool is because the tradicional way to model this in JS would
be to use `null`. I'd have a variable, that I'd know would either hold a string,
or `null`. So in order to be fully safe, I'd have to check every time that I'm
using this value, whether it holds something relevant or whether it is `null`.

Except this requires great discipline, and potentially any value can be `null`,
so in reality what happens is that these checks are nto written and you get
exeptions in runtime. And that's not even accounting for the fact that nothing
prevents other code from assigning something completely unexpected to it.

Elm aims to provide code that runs with **no runtime exceptions**. The big
caveat for that statement is that nothing prevents the compiler to be buggy,
of course. However, barring bugs in the compiler, Elm's
**semantics are such that runtime errors are not possible**.

This rich type system enables us to defeat whole classes of bugs that happen
in JS. In Elm, the language will force you, at the syntactica level, to
effectively destructure whatever value you think you're holding and make sure
you handle it in the code. So, for example, if I have a value `x` typed as a
`Maybe String`, and I want to access it, I need code such as


A failure to handle one of the cases would result in an error at *compile time*.
This gets even better with types that hold set of specific values. We could have,
for example, a type such as

```
type Readable = Article | Quizz | Review | GroupList
```

If anywhere in the code I need to know and evaluate the type of a value of this type,
the compiler will force me to handle all possible scenarios.

```
case post of
  Article ->
    "this is an article"
  Review ->
    "this is a review"
...
```

If I happen to forget one of the possibilities, my code will not compile. Yet another
bug that would only be caught in production, that was mitigated by having a smart
compiler and a nice type system.

There are of course many other advantages and very interesting characteristics
regarding Elm's type system, but I have little time now, so I'll send some more
references around at some point afterwards.

## Immutable

The benefits **immutability** bring to the table are already being observed by us
with the surge of immutable data structure libraries that we now use in JS. The
idea that a specific reference will always hold the same value, that no other
function can mutate it from somewhere else, that if at some point some variable
had some value, it'll *always* have this value, brings us an ability to reason
about code much more freely and effectively.

## Architecture

Let's talk about the **Elm Architecture** now. The Elm architecture is very nice
to talk about for people who are using Redux, because many of the concepts
are either the same or very close. The idea of unidirectional data flow, for
example, and why it's nice, is completely accepted by you guys.

The Elm architecture has basically three parts: a `Model`, an `Update` function,
and a `View`. A `Model` is used to hold the data that the program is operating on.
The `Update` function is a function that receives an `Action`, a copy of the model,
and returns a model, which is then made the current model. It is conceptually
equivalent to Redux's `Reducer`s.

A `View`, finally, is a function that takes a model object and returns HTML.

There is currently lots of cross-polination between Redux and Elm. It's not uncommon
when reading Redux's issues to be faced with people illustrating Elm's solution
to a specific problem.

See the similarities?

## Nice error messages

Elm has extremely good error messages, and effort has been specifically
taken when implementing the compiler to provide good error messages. TODO: Add
link to relevant blog posts here.

## Things to be wary of

– It's a very new technology
– It's not simply a syntax extension to JavaScript, like CoffeeScript or the
  lower stages of Babel: It's a completely different language and its compiled
  JavaScript code is readable and commented, for what it is, but doens't map
  as straightforwardly to the code you wrote.
