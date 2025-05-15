***Combinators*** are [[7 - 𝜆-Calculus#Closed lambda expressions, i.e. combinators|closed $\lambda$-abstraction terms]] which can be <u>composed</u> to recreate _other_ [[7 - 𝜆-Calculus|$\lambda$-calculus]] terms.
A set of ***combinators*** which <u>compose</u> to produce <u>every</u> other [[7 - 𝜆-Calculus|$\lambda$-calculus]] term *(upto [[7 - 𝜆-Calculus#Equivalences|$\beta \eta$-equivalence]])* is called a ***complete combinatory basis***
- By making the ***combinators*** in such a set as <u>primitive</u> we can get rid of [[7 - 𝜆-Calculus|$\lambda$-abstraction]] entirely
- All possible terms could be written <u>point-free</u> in terms of the ***basis-combinators***
- This is the entire premise of [Combinatory Logic](https://en.wikipedia.org/wiki/Combinatory_logic)
	- The most popular variant is [$SKI$ combinator calculus](https://en.wikipedia.org/wiki/SKI_combinator_calculus#Self-application_and_recursion) which takes [[#Common combinators|the $S,K,I$ combinators as primitive]]
	- You can even drop the [[#Identity combinator $ mathrm{I} equiv lambda x.x$|$I$ combinator]] and recreate it with only [[#Common combinators|$S$ and $K$]]
	- *In fact*, you can use only a <u>single combinator</u> $\displaystyle X \ \equiv \ \lambda x. x \ S \ K$ as a ***complete combinatory basis*** $\{ X \}$, because
		- $\displaystyle X \ (X \ (X \ X)) =_{\beta} = K$, and
		-  $\displaystyle X \ (X \ (X \ (X \ X))) =_{\beta} = S$
		- There are <u>infinitely</u> many such bases

# Common combinators
## Identity combinator: $\mathrm{I} \equiv \lambda x.x$
The ***Identity combinator*** is the *simplest* <u>combinator</u>, which just returns the argument it was given, i.e. $\displaystyle (\mathrm{I} \ x) =_{\beta} x$.
- Together with [[#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kestrel and Kite]] it can be used to implement [[9 - Applications of 𝜆-Calculus#Basic logical operators $ text{and}$, $ text{or}$, $ mathrm{not}$, $ text{if} (p) text{then} (x) text{else} (y)$|if-then-else]] expression for [[9 - Applications of 𝜆-Calculus#Church Booleans encoding $ displaystyle mathbb{B} = { mathrm{true}, mathrm{false} }$|Church booleans]]
- In [Haskell](https://www.haskell.org/) this is the `id` function

### Identity once-removed ($\displaystyle \mathrm{I^{*}}$) and Thrush ($\mathrm{T}$) combinators
- ***Identity once-removed:*** $\displaystyle \mathrm{I^{*}} \equiv \lambda fx.fx$
	- The <u>application</u> combinator
	- In [Haskell](https://www.haskell.org/) this is the `$` function
- ***Thrush:*** $\displaystyle \mathrm{T} \equiv \lambda xf.fx$
	- The <u>transposed-application</u> combinator
	- Can be used to implement [[9 - Applications of 𝜆-Calculus#Church numerals encoding natural numbers $ mathbb{N}$|exponentiation for Church numerals]]
	- In [Haskell](https://www.haskell.org/) this is the `&` function

## Kestrel ($\displaystyle \mathrm{K}$) and Kite ($\displaystyle \mathrm{KI}$) combinators: $\displaystyle \mathrm{K} \equiv \lambda xy.x$, $\mathrm{KI} = \lambda xy.y$
The ***Kestrel and Kite combinators*** *"select"* either the <u>first</u> or <u>second</u> arguments, respectively.
- $\displaystyle \mathrm{KI}$ is *quite literally* ***Kestrel*** applied to [[#Identity combinator $ mathrm{I} equiv lambda x.x$|Identity]]
- By itself, ***Kestrel*** can be used like a *"constant"* function, i.e. $(\mathrm{K} \ x)$ is the *"constant $x$"* function
- Together they can be used as *opposites*, like in [[9 - Applications of 𝜆-Calculus#Church Booleans encoding $ displaystyle mathbb{B} = { mathrm{true}, mathrm{false} }$|Church booleans]] or the [[9 - Applications of 𝜆-Calculus#Pairs and Linked Lists|Church encoding of pairs]]
	- In [Haskell](https://www.haskell.org/) ***Kestrel*** is the `const` function

## Bluebird combinator: $\displaystyle \mathrm{B} \equiv \lambda fgx.f(gx)$
The ***Bluebird combinator*** is the <u>composition</u> combinator
- $(\mathrm{B} \ f \ g)$ is the <u>composition</u> $f \circ g$
- In [Haskell](https://www.haskell.org/) this is the `.` function

### Dove ($\displaystyle \mathrm{D}$), Dickcissel ($\displaystyle \mathrm{D_{1}}$), Dovekies ($\displaystyle \mathrm{D_{2}}$), Blackbird ($\displaystyle \mathrm{B_{1}}$), Eagle ($\displaystyle \mathrm{E}$), Bunting ($\displaystyle \mathrm{B_{2}}$), Becard ($\displaystyle \mathrm{B_{3}}$) and Bald Eagle ($\displaystyle \mathrm{\hat{E}}$) combinators
These combinators are all variations on <u>many-to-one composition</u>
- ***Dove:*** $\displaystyle \mathrm{D} \equiv \lambda fxgy.fx(gy)$
	- $\displaystyle \mathrm{D} =_{\beta} \mathrm{B}\mathrm{B}$
- ***Dickcissel:*** $\displaystyle \mathrm{D_{1}} \equiv \lambda fxygz.fxy(gz)$
	- $\displaystyle \mathrm{D_{1}} =_{\beta} \mathrm{B}(\mathrm{D}) =_{\beta} \mathrm{B}(\mathrm{B} \mathrm{B})$
- ***Dovekies:*** $\displaystyle \mathrm{D_{2}} \equiv \lambda fgxhy.f(gx)(hy)$
	- $\displaystyle \mathrm{D_{2}} =_{\beta} \mathrm{B}\mathrm{B}(\mathrm{D}) =_{\beta} \mathrm{B}\mathrm{B}(\mathrm{B} \mathrm{B})$
- ***Blackbird:*** $\displaystyle \mathrm{B_{1}} \equiv \lambda fgxy.f(gxy)$
	- A <u>2-to-1 composition</u> combinator
	- $\displaystyle \mathrm{B_{1}} =_{\beta} \mathrm{B}\mathrm{B}\mathrm{B}$
- ***Eagle:*** $\displaystyle \mathrm{E} \equiv \lambda fxgyz.fx(gyz)$
	- $\displaystyle \mathrm{E} =_{\beta} \mathrm{B}(\mathrm{B_{1}}) =_{\beta} \mathrm{B}(\mathrm{B} \mathrm{B} \mathrm{B})$
- ***Bunting:*** $\displaystyle \mathrm{B_{2}} \equiv \lambda fgxyz.f(gxyz)$
	- A <u>3-to-1 composition</u> combinator
	- $\displaystyle \mathrm{B_{1}} =_{\beta} \mathrm{E} \mathrm{B} =_{\beta} \mathrm{B} (\mathrm{B}\mathrm{B}\mathrm{B}) \mathrm{B}$
- ***Bunting:*** $\displaystyle \mathrm{B_{3}} \equiv \lambda fghx.f(g(hx))$
	- $(\mathrm{B_{3}} \ f \ g \ h)$ is the <u>composition</u> $f \circ g \circ h$
	- $\displaystyle \mathrm{B_{3}} =_{\beta} \mathrm{D_{1}} \mathrm{B} =_{\beta} \mathrm{B} (\mathrm{B}\mathrm{B}) \mathrm{B}$
- ***Bald Eagle:*** $\displaystyle \mathrm{\hat{E}} \equiv \lambda fgxyhzw.f(gxy)(hzw)$
	- $\displaystyle \mathrm{\hat{E}} =_{\beta} \mathrm{E} (\mathrm{E}) =_{\beta} \mathrm{B}(\mathrm{B} \mathrm{B} \mathrm{B}) (\mathrm{B}(\mathrm{B} \mathrm{B} \mathrm{B}))$

### Queer ($\displaystyle \mathrm{Q}$), Quixotic ($\displaystyle \mathrm{Q_{1}}$), Quizzical ($\displaystyle \mathrm{Q_{2}}$), Quirky ($\displaystyle \mathrm{Q_{3}}$) and Quacky ($\displaystyle \mathrm{Q_{4}}$) combinators
These combinators are all variations on <u>out-of-order composition</u>
- ***Queer:*** $\displaystyle \mathrm{Q} \equiv \lambda fgx.g(fx)$
	- $(\mathrm{Q} \ f \ g)$ is the <u>composition</u> $g \circ f$
	- In [Haskell](https://www.haskell.org/) this is the `>>>` function [specialised on the category of Hask functions](https://hackage.haskell.org/package/base-4.20.0.1/docs/Control-Arrow.html)
- ***Quixotic:*** $\displaystyle \mathrm{Q_{1}} \equiv \lambda fxg.f(gx)$
- ***Quizzical:*** $\displaystyle \mathrm{Q_{2}} \equiv \lambda xfg.f(gx)$
- ***Quirky:*** $\displaystyle \mathrm{Q_{3}} \equiv \lambda fxg.g(fx)$
- ***Quacky:*** $\displaystyle \mathrm{Q_{4}} \equiv \lambda xfg.g(fx)$

## Mockingbird combinator: $\displaystyle \omega  \equiv \lambda x. xx$
The ***Mockingbird combinator*** takes a function and applies it to itself.
- It is a fundamental building block for many [[#Recursion fixpoint combinators|implementations of recursion]]
- Together with [[#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kestrel and Kite]] it can be used to implement [[9 - Applications of 𝜆-Calculus#Basic logical operators $ text{and}$, $ text{or}$, $ mathrm{not}$, $ text{if} (p) text{then} (x) text{else} (y)$|logical "or" operator]] for [[9 - Applications of 𝜆-Calculus#Church Booleans encoding $ displaystyle mathbb{B} = { mathrm{true}, mathrm{false} }$|Church booleans]]

### Omega combinator: $\displaystyle \Omega \equiv \omega \omega$
The ***Omega combinator*** is a <u>non-terminating</u> combinator which has no [[7 - 𝜆-Calculus#Head Normal Form (HNF)|head normal form]] *(let alone [[7 - 𝜆-Calculus#$ beta$-Normal Form|$\beta$-normal form]])* i.e. its <u>unsolvable</u>.
- $\displaystyle \Omega \equiv \omega \omega \to_{\alpha} (\lambda x.xx) \ \omega \to_{\beta} \omega \omega$
- The ***Omega combinator*** is usually <u>identified with all unsolvable combinators</u>
- Interpreted as <u>undefined</u> when used for for logic and arithmetic

### Warbler ($\displaystyle \mathrm{W}$), Lark ($\displaystyle \mathrm{L}$) and Owl ($\displaystyle \mathrm{O}$) combinators
These ***combinators*** all have the property that $\displaystyle \omega =_{\beta} \mathrm{WI} =_{\beta} \mathrm{LI} =_{\beta} \mathrm{OI}$
- ***Warbler:*** $\displaystyle \mathrm{W} \equiv \lambda fx. fxx$
	- The <u>argument duplicator</u> combinator
		-  If $f$ expects $2$ arguments, $(W \ f)$ expects $1$ which it <u>repeats twice</u>
		- e.g. $W \ (+)$ is a function which <u>doubles</u> its argument, i.e. $W \ (+) \ x \ \ \equiv \ \ x+x \ \ \equiv \ \  2x$
	- It is <u>Mockingbird</u> once-removed, i.e. $\displaystyle \omega^* = \mathrm{W}$
- ***Lark:*** $\displaystyle \mathrm{L} \equiv \lambda fg. f(gg)$
- ***Owl:*** $\displaystyle \mathrm{O} \equiv \lambda fg. g(fg)$

## Cardinal combinator: $\mathrm{C} \equiv \lambda fxy. fyx$
The ***Cardinal combinator*** will take a two-argument function, and flip those arguments.
- It can turn [[#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kestrel]] into [[#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kite]] and vice-versa, *i.e. $(\displaystyle \mathrm{C} \ \mathrm{K}) =_{\beta} \mathrm{Ki}$ and $\displaystyle (\mathrm{C} \ \mathrm{Ki}) =_{\beta} \mathrm{K}$,* so it can define the [[9 - Applications of 𝜆-Calculus#Basic logical operators $ text{and}$, $ text{or}$, $ mathrm{not}$, $ text{if} (p) text{then} (x) text{else} (y)$|logical "not" operator]] for [[9 - Applications of 𝜆-Calculus#Church Booleans encoding $ displaystyle mathbb{B} = { mathrm{true}, mathrm{false} }$|Church booleans]]
- It is [[#Identity once-removed ($ displaystyle mathrm{I {*}}$) and Thrush ($ mathrm{T}$) combinators|Trush]] once-removed, i.e. $\displaystyle \mathrm{T^*} = \mathrm{C}$
- In [Haskell](https://www.haskell.org/) this is the `flip` function

### Viero ($\displaystyle \mathrm{V}$), Robin ($\displaystyle \mathrm{R}$) and Finch ($\displaystyle \mathrm{R}$) combinators
Just like the <u>Cardinal combinator</u>, these are all <u>derived</u> from the [[#Identity once-removed ($ displaystyle mathrm{I {*}}$) and Thrush ($ mathrm{T}$) combinators|Trush combinator]]
- ***Viero:*** $\displaystyle \mathrm{V} \equiv \lambda xyz. zxy$
	- Together with [[#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kestrel and Kite]] it can be used to implement the [[9 - Applications of 𝜆-Calculus#Pairs and Linked Lists|Church encoding of pairs]] where $\mathrm{K}$ is [[9 - Applications of 𝜆-Calculus#Pairs and Linked Lists|$\mathrm{fst}$]] and $\mathrm{KI}$ is [[9 - Applications of 𝜆-Calculus#Pairs and Linked Lists|$\mathrm{snd}$]]
- ***Robin:*** $\displaystyle \mathrm{R} \equiv \lambda xyz. yzx$
	- Derived from [[#Identity once-removed ($ displaystyle mathrm{I {*}}$) and Thrush ($ mathrm{T}$) combinators|Trush]] as $\mathrm{BBT}$
- ***Finch:*** $\displaystyle \mathrm{F} \equiv \lambda xyz. zyx$
	- Is ***Viero*** with its arguments flipped, i.e. $\mathrm{CV}$
	- Can be used as an "inverse" <u>pair constructor</u>

## Starling combinator: $\displaystyle \mathrm{S} \equiv \lambda fgx. (fx)(gx)$
The ***Starling combinator*** first *"provides the context of $x$"* to $f$ and $g$, and *then* applies them to each-other
- It is [[#Warbler ($ displaystyle mathrm{W}$), Lark ($ displaystyle mathrm{L}$) and Owl ($ displaystyle mathrm{O}$) combinators|Owl]] once-removed, i.e. $\displaystyle \mathrm{O}^* = \mathrm{C}$
- In [Haskell](https://www.haskell.org/) this is the `<*>` function for [reader monad instance](https://hackage.haskell.org/package/mtl-2.3.1/docs/Control-Monad-Reader.html) of `(->) r` type *(i.e. partially applied function type)*

## Psi combinator: $\displaystyle \Psi \equiv \lambda fgxy. f(gx)(gy)$
The ***Psi combinator*** processes two arguments before combining them
- e.g. $\displaystyle (\Psi \ (+) \ \text{length})$ is a function that will add the lengths of two lists
	- e.g. $\displaystyle (\Psi \ (+) \ \text{length}) \ [1, 2] \ [3] \to_{\beta} (\text{length} \ [1,2]) + (\text{length} \ [3]) \to_{\beta} 2+3 \to_{\beta} 5$
- In [Haskell](https://www.haskell.org/) this is the `on` function

# Recursion: fixpoint combinators
A [fixed-point](https://en.wikipedia.org/wiki/Fixed_point_(mathematics)) of a function is where it returns the input unchanged
- i.e. for any $x$, if $f \ x = x$ then $x$ is a ***fixed-point*** of $f$
	- e.g. $\mathrm{double} \ 0 = 0$
Then the defining property of any ***fixed-point combinator*** $\mathrm{fix}$ is that: if $f$ has *any* ***fixed-points***, then $\mathrm{fix} \ f$ is *one* of those ***fixed-points***
- i.e. $f \ (\mathrm{fix} \ f) =_{\beta} \mathrm{fix} \ f$
These fixed point operators can be used to implement ***recursion*** in [[7 - 𝜆-Calculus|$\lambda$-calculus]].

For example the <u>factorial</u> function $\mathrm{fact}$,
- $\displaystyle \mathrm{fact} \equiv \lambda n. \mathrm{if} \ (\mathrm{isZero} \ n) \ \mathrm{then} \ 1 \ \mathrm{else} \ n \times (\mathrm{fact} \ (n-1) )$
- Now we can simply *extract* $\displaystyle \mathrm{fact} \equiv [\lambda f n. \mathrm{if} \ (\mathrm{isZero} \ n) \ \mathrm{then} \ 1 \ \mathrm{else} \ n \times (f \ (n-1) )] \ \mathrm{fact}$
	- The inner part we can call $F$, i.e. $\displaystyle F \equiv \lambda f n. \mathrm{if} \ (\mathrm{isZero} \ n) \ \mathrm{then} \ 1 \ \mathrm{else} \ n \times (f \ (n-1) )$
	- So now its $\displaystyle \mathrm{fact} \equiv F \ \mathrm{fact}$ 
- Now we use $\mathrm{fix}$ to re-write it as $\mathrm{fact} \equiv \mathrm{fix} \ F$
	- Since $\displaystyle \mathrm{fix} \ F =_{\beta} F \ (\mathrm{fix} \ F)$ and $\mathrm{fix} \ F \equiv \mathrm{fact}$, we get $\mathrm{fact} =_{\beta} F \ \mathrm{fact}$, i.e. our <u>original</u> definition

## $\mathrm{Y}$-combinator: $\mathrm{Y} \equiv \lambda f. (\lambda x.f (xx))(\lambda x.f (xx))$
The ***Y-combinator***, discovered by [Haskell B. Curry](https://en.wikipedia.org/wiki/Haskell_Curry "Haskell Curry"), is a *particular implementation* of $\mathrm{fix}$, with the properties that
- $\displaystyle \mathrm{Y} \ f =_{\beta} f \ (\mathrm{Y} \ f)$ in the sense that they both [[7 - 𝜆-Calculus#$ beta$-reduction|$\beta$-reduce]] to the same common term *(by [[7 - 𝜆-Calculus#Confluence (Church–Rosser property)|confluence]])*
- However $f  \ (\mathrm{Y} \ f)$ may not *in general* [[7 - 𝜆-Calculus#$ beta$-reduction|$\beta$-reduce]] to $\mathrm{Y} \ f$
- Written with [[#Bluebird combinator $ displaystyle mathrm{B} equiv lambda fgx.f(gx)$|$\mathrm{B}$]], [[#Cardinal combinator $ mathrm{C} equiv lambda fxy. fyx$|$\mathrm{C}$]] and [[#Mockingbird combinator $ displaystyle omega equiv lambda x. xx$|$\omega$]] as $\displaystyle \mathrm{Y} \equiv \mathrm{B} \ \omega \ (\mathrm{C} \ \mathrm{B} \ \omega)$
	- $\displaystyle \mathrm{B} \ \omega \ (\mathrm{C} \ \mathrm{B} \ \omega) \ \to_{\beta} \ \lambda f. \ \omega \ (\mathrm{C} \ \mathrm{B} \ \omega \ f) \ \to_{\beta} \ \lambda f. \ \omega \ (\mathrm{B} \ f \ \omega) \ \to_{\beta} \ \lambda f. \ \omega \ (\lambda x. \ f \ (\omega \ x))$
	- $\to_{\beta} \ \lambda f. \ \omega \ (\lambda x. f (xx)) \to_{\beta} \ \lambda f. (\lambda x. f (xx))(\lambda x. f (xx)) \equiv Y$

## Turing ($\mathrm{U}$) and Theta ($\mathrm{\Theta}$) combinators: $\displaystyle \mathrm{U} \equiv \lambda xy.y(xxy)$, $\displaystyle \Theta \equiv \mathrm{U}\mathrm{U}$
The ***Turing and Theta combinators*** mirror the definitions of [[#Mockingbird combinator $ displaystyle omega equiv lambda x. xx$|Mockingbird]] and [[#Omega combinator $ displaystyle Omega equiv omega omega$|Omega]] combinators, where the ***Theta combinator*** is another *particular implementation* of $\mathrm{fix}$ discovered by [Alan Turing](https://en.wikipedia.org/wiki/Alan_Turing)
- Its advantage over $\mathrm{Y}$ is that $\Theta \ f$ [[7 - 𝜆-Calculus#$ beta$-reduction|$\beta$-reduces]] to $f \ (\Theta \ f)$ rather than *only* them both [[7 - 𝜆-Calculus#$ beta$-reduction|$\beta$-reducing]] to the same common term

## $\mathrm{Z}$-combinator: $\mathrm{Z} \equiv \lambda f. (\lambda x.f (\lambda y. xxy))(\lambda x.f (\lambda y.xxy))$
In a <u>strict programming language</u> the [[#$ mathrm{Y}$-combinator $ mathrm{Y} equiv lambda f. ( lambda x.f (xx))( lambda x.f (xx))$|$\mathrm{Y}$-combinator]] will expand until <u>stack-overflow</u>, or never halt in case of [tail call optimization](https://en.wikipedia.org/wiki/Tail_call_optimization "Tail call optimization").
- The ***$Z$-combinator*** will work in <u>strict languages</u> because the next argument is defined <u>explicitly</u>, preventing the expansion of $\mathrm{Z} \ f$ in *RHS* of $\mathrm{Z} \ f \ x = f \ (\mathrm{Z} \ f) \ x$.
- in lambda calculus it is an [eta-expansion](https://en.wikipedia.org/wiki/Eta_expansion "Eta expansion") of the [[#$ mathrm{Y}$-combinator $ mathrm{Y} equiv lambda f. ( lambda x.f (xx))( lambda x.f (xx))$|$\mathrm{Y}$-combinator]]
