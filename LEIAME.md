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
de declarar o tipo de algo, mas em Elm tudo é implicitamente tipado: o
compilador realiza *inferência de tipos*, que é observar o fluxo de informação
no programa e derivar disso os tipos que as funções ou valores podem ter.

Isso quer dizer que esse pedaço de código já sabe que o seu tipo de entrada
é uma única variável que é um inteiro, ja'que você está adicionando ele a 2,
e que o seu tipo de retorno tambémé um inteiro.

Se você tentar chamar isso com uma string, `plusTwo "Olá"`, o compilador
te alertará de um mismatch de tipos. Nós ganhamos isso de graça por causa da
inferência de tipos.

Adicionalmente, quaisquer valores ou funções podem ser dadas anotações de tipo.
Isso quer dizer que por mais que as anotações de tipo – mas não a tipagem  –
sejam plenamente opcionais, você ainda pode documentar os tipos explicitamente
para facilitar a compreensão do código e dar dicas pro compilador dos tipos que
você realmente quer dizer.

A função que nós estávamos discutindo é compreendida pelo compilador como se
tivesse sido definida e anotada da seguinte forma:

```
plusTwo : Int -> Int
plusTwo =
  n + 2
```

Mesmo que a inferência de tipo em si já seja legal, o sistema de tipos que o Elm
provê é fundamentalmente diferente daquele que o JavaScript possui, devido ao
fato de que o do Elm oferece tipos paramétricos. Se você já conhece genêricos do
C#/Java ou templates do C++, você já está familiarizado com a ideia de tipos
paramétricos, mas a execução específica desse conceito no Elm nos trás
muito mais.

O exemplo mais comum é o tipo `Maybe`. Ele leva um outro tipo como argumento
(digamos `String`), e suas instânciass terão a qualquer momento um de dois
possíveis valores: O valor `Nothing`, ou um valor do tipo `Just String`.

```
frob : Maybe String -> Foo
frob x =
  case x of
    Nothing ->
      -- fazer alguma coisa aqui
    Just x' ->
      -- fazer alguma coisa aqui
```

Isso é legal porque a forma tradicional de modelar isso em JS seria utilizando
`null`. Eu teria uma variável que eu sei que poderia guardar ou uma `String` ou
`null`. Para estarmos completamente seguros seria então necessário, sempre que
se usar o valor, checar se ele contém algo relevante ou se contém um `null`.

O problema é que isso requer muita disciplina, e potencialmente qualquer valor
pode ser `null`, então na realidade o que acontece é que essas verificações não
são sempre escritas e ficamos passíveis de receber exceções em tempo de
execução. E ainda não estamos sequer considerando a possibilidade de alguém
atribuir a uma variável um valor completamente inesperado.

Elm visa prover código que roda sem *exceções em tempo de execução* (runtime
exceptions). Há um grande porém para essa frase que é o fato de que nada previne
que o compilador em si tenha bugs, é claro. Contudo, excetuando-se bugs de
compilador, a **semântica do Elm é tal que erros em tempo de execução não são
possíveis**.

Esse rico sistema de tipos nos possibilita facilemente derrotar uma série de
bugs que ocorrem em JS. Em Elm, a linguagem te forçará, a nível sintático, a
efetivamente desestruturar o valor que você tem e a lidar com isso no código.
Então, por exemplo, se você tem um valor `x` tipado como `Maybe String`, e você
quer acessá-lo, será necessário código como

Se você não lidar com todos os casos, isso geraria um erro em *tempo de
compilação.* Isso fica ainda melhor com tipos cujos valores vêm de um grupo
específico. Poderíamos, por exemplo, ter um tipo na forma

```
type Readable = Article | Quizz | Review | GroupList
```

Se em qualquer lugar do código eu precisar avaliar um valor desse tipo, o
compilador me obrigará a lidar com todos os cenários possíveis.


```
case post of
  Article ->
    "this is an article"
  Review ->
    "this is a review"
...
```

Se eu por acaso esquecer uma das possibilidades, meu código não compilará.

## Imutável

Os benefícios que a **imutabilidade** nos traz já podem ser por nós observados
com o surgimento e aumento do uso de uma série de bibliotecas de estruturas de
dados imutáveis em JavaScript. A ideia de que uma referência específica sempre
apontará para o mesmo valor, que nenhuma outra função pode modificá-la de outro
lugar, e que se em algum momento uma variável tinha um valor, ela *sempre* terá
esse valor, nos leva a uma habilidade de raciocinar sobre o código muito mais
livre e efetivamente.

## Arquitetura

Vamos falar agora da **Arquitetura Elm**. É muito interessante falar sobre isso
com pessoas que estão utilizando Redux, pois muitos dos conceitos são os mesmos
ou muito perto. A ideia de fluxo unidirecional de dados, por exemplo, e por que
isso é legal, já é completamente aceita.

Essa arquitetura tem basicamente três aprtes: um modelo `Model`, uma função de
*update*, e uma *view*. Um `Model` é utilizado para manter o estado do programa.
A função `Update` recebe como parâmetro uma ação (`Action`) e uma cópia do
modelo e retorna um novo modelo que será feito o modelo atual. É conceitualmente
equivalente aos `reducers` to Redux.

Uma `View`, finalmente, é uma função que recebe um objeto de modelo e retorna
HTML.

Ocorre atualmente muita alogamia (*cross polination*) entre Redux e Elm. Não é
incomum encontrar, ao se ler os *issues* do Redux, pessoas ilustrando a solução
Elm para o problema análogo.

Já vê semelhanças?

## Mensagens de erro legais

Elm tem mensagens de erro extremamente boas, e esforço foi especificamente
colocado ao implementar o compilador para prover boas mensagens de erro. TODO:
Adicionar link para os blog posts relevantes.

## Coisas para se tomar cuidado

– É uma tecnologia muito nova

- Não é meramente uma extensão sintática do JavaScript, como o CoffeeScript ou
  os estágios mais baixos do Babel: É uma linguagem completamente diferente e
  por mais que o JavaScript produzido ao compilar Elm seja legível e bem
  comentado, considerando o que é, ele ainda assim não se mapeia tão diretamente
  ao código que você escreveu.
