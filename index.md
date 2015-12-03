Elm is a language which compiles down to JavaScript and
HTML. Additionally, there is something known as the Elm Architecture, which
is a set of patterns and divisions concerning how to organize and structure
the flow of information in a specific application.

Elm is interesting because, by its characteristics, it aims to solve many of
the problems encountered today when developing complex JavaScript applications,
such as unknown breakage of code, brittleness when refactoring – which then
may lead to an unwillingness to do so –, errors that only occur in runtime,
a strong need for defensive programming and awareness of languages particularities –
which derives one from the main focus of providing value through features to
the end users –, among other things.

Here are some of the characteristics Elm has, which help us fighting these problems:
Elm is a functional, reactive, strongly typed, immutable programming language,
influenced by languages like Haskell or OCaml.

Functional means that there are no side-effects. The only state you have access to
are the parameters of your function and the values of its closure. The only way
you can affect state is by returning a value. Functions can call other functions,
and these rules still apply, in a way that the flow of information in the tree
of function calls is very clear. Because functions have no side effect, every
time you call a function with the same arguments it is guaranteed to do the same
thing and return the same value.

Reactive basically means that, in one way or another, there are ways to deal
with the passing of time, as a fundamental concept. Elm has a powerful abstraction
called a Signal, which represents a value that changes over time. It always has
a starting value and a current value, and you generally use the vocabulary
of sequences to visualize it. In the same way that a sequence of values can be
mapped on, or folded over, these are the operations you generally perform on a signal.

In the same way that there is `List.map`, there is `Signal.map`; in the same way that
`List.foldr` exists, fold from the right, also known as reduce, there is `Signal.folp`,
meaning fold from the past :-) It's a very interesting abstraction and the base of
reactivity in Elm.

The fact that Elm is strongly typed may throw off people who have in a past life
dealt with compilers that seem to unnecessarily get in the way of what they
were *actually* trying to achieve, and felt that the compiler wasn't really their
friend, and more something to be beat. Languages like C++, Delphi, Java...

The reality is that these experiences reflect an approach to typing that is completely
different from the approach functional languages from the family that Elm comes from
traditionally take. For starters, these strongly typed OO languages tend to have the
need to constantly be remembered what's the type of something. I'm attributing a literal
to a variable, `Int i = 3;`, and I have to tell it its type? This makes no sense, the
language had all it needed to figure out thee type by itelf. Typecassts, type INFERENCE

functional reactive is basically a function with implied time

immutable becasue no references to old data, no mutating things that shouldn't be mutated

no runtime errors (unless by bugs)

the type system is awesome becasuse it's parametric and functionally rich

eliminates whole classes of bugs


functional is nice becaus of same reasons why functional views are nice.
if you structure your whole program around functional views because they're easy to compose and
esasy to test, then you're alreay aware of the benefites that the fact that your
HTML is a pure function of its parameters, and does not rely on modifying
external state to be rendered provide you.

extremely good error messages

talk about potential bad sides
