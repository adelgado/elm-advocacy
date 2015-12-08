# Advogando Elm

## What is Elm?

Elm é uma linguagem web de *frontend* que compila para JavaScript e HTML. Seu
*design* foi influenciado por linguagens como Haskell, Rust ou OCaml. Além
disso, existe algo chamado de Arquitetura Elm (*Elm Architecture*), que é um
conjunto de padrões e divisões com relação à oragnização e estrutura do fluxo de
informação numa aplicação específica.

## Por que Elm?

O Elm é de interesse para desenvolvedores de *frontend* pois por suas
características visa resolver vários dos problemas encontrados hoje ao
desenvolvermos aplicações JavaScript complexas, como código que quebra
silenciosamente, refatoramentos problemáticos – levando às vezes inclusive a uma
resistência a refatorar –, exceções não tratadas e exeções no geral,
uma necessidade grande de programação defensiva que distrai do foco principal de
criar valor via funcionalidades ao usuário final, e poderiam ser resolvidas a
nível de linguagem –, entre outras coisas.

Eis algumas das características que o Elm possui, que nos ajudam a lutar contra
esses problemas: Elm é uma linguagem de programação funcional, reativa,
fortemente tipada e imutável.

**Funcional** significa que não há efeitos colaterais. O único estado que você
tem acesso são os parâmetros da sua função e os valores do seu fechamento
(*closure*). A única forma que você pode afetar estado é retornando um valor.
Funções podem chamar umas as outras, e estas regras ainda se aplicam, de tal
forma que o fluxo de informação na árvore de chamadas de função é muito claro.
Já que as funções não terão efeitos colaterais, todas as vezes que você chama
uma função com os mesmos argumentos há uma garantia que fará a mesma coisa e
retornará o mesmo valor. Essas funções puras são um prazer de testar! :-)

**Reativo** basicamente significa que, de uma forma ou de outra, há formas para
se lidar com a passagem do tempo, num nível conceitual fundamental. Elm possui
uma abstração poderosa que se chama um sinal (*Signal*), que representa um valor
que muda ao passar do tempo. Um sinal sempre tem um valor inicial e um valor
atual, e normalmente se utiliza o vocabulário de sequências para visualizá-lo.
Da mesma forma que se pode mapear uma sequência de valores para outra (`map`),
ou agrupar uma sequência (`fold`), essas são as operações que normalmente são
executadas num sinal.

Da mesma forma que há `List.map`, existe a função `Signal.map`; da mesma forma
que existe `List.foldr`, que dobra a lista pela direita, também conhecido como
`reduce`, existe `Signal.foldp`, que siginica um `fold` vindo do passado :-) É
uma abstração muito interessante e é a base da reatividade em Elm.

O fato de que o Elm é *fortemente tipado* pode desagradar pessoas que numa outra
vida tiveram que lidar com compiladores que pareciam desnecessariamente se
colocar entre elas e o objetivo que estavam *realmente* querendo atingir;
pessoas que sentiam que o compilador não era realmente seu amigo, e mais algo a
ser vencido. Linguagens como C++, Delphi, Java...

A realidade é que essas experiências refletem um *approach* à tipagem que é
completamente diferente do *approach* das linguagens da família do Elm. Em uma
linguagem orientada a objetos tradicional, existe normalmente uma necessidade
de declarar o tipo de algo, mas em Elm tudo é implicitamente tipado: o compilador realiza *inferência de tipos*, que é observar o fluxo de informação no programa e derivar disso os tipos que as funções ou valores podem ter.

Isso quer dizer que esse pedaço de código já sabe que o seu tipo de entrada
é uma única variável que é um inteiro, ja'que você está adicionando ele a 2,
e que o seu tipo de retorno tambémé um inteiro.

Se você tentar chamar isso com uma string, `plusTwo "Olá"`, o compilador
te alertará de um mismatch de tipos. Nós ganhamos isso de graça por causa da
inferência de tipos.

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
