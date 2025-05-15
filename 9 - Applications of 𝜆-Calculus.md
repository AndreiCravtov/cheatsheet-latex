# $\lambda$-definable [[Partial Function|partial functions]]
![[Pasted image 20241121184009.png|550]]

# The Church-Turing Thesis: equivalence between [[1 - Register Machines#Register-machine-Computable functions|RM]], [[6 - Turing Machines#Turing computable function|TM]] and [[7 - 𝜆-Calculus|$\lambda$-calculus]] notions of computability
![[Pasted image 20241121183728.png|500]]
\*by a human following an algorithm, ignoring resource limitations

# Encoding datatypes in [[7 - 𝜆-Calculus|$\lambda$-calculus]]
## Logic and predicates in  [[7 - 𝜆-Calculus|$\lambda$-calculus]]
### Church Booleans: encoding $\displaystyle \mathbb{B} = \{ \ \mathrm{true}, \ \mathrm{false} \ \}$
The Church Boolean encoding associates [[8 - Combinators - a way to eliminate 𝜆-abstraction#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kestrel]] with $\mathrm{true}$ and [[8 - Combinators - a way to eliminate 𝜆-abstraction#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kite]] with $\mathrm{false}$, so
- $\displaystyle \mathrm{true} \equiv \mathrm{K} \equiv \lambda xy.x$
- $\displaystyle \mathrm{false} \equiv \mathrm{KI} \equiv \lambda xy.y$

### Basic logical operators: $\text{and}$, $\text{or}$, $\mathrm{not}$, $\text{if} \ (p) \ \text{then} \ (x) \ \text{else} \ (y)$
There are many *other encodings*, but the advantage of [[#Church Booleans encoding $ displaystyle mathbb{B} = { mathrm{true}, mathrm{false} }$|Church Booleans]] is that ***if-then-else*** becomes just [[8 - Combinators - a way to eliminate 𝜆-abstraction#Identity combinator $ mathrm{I} equiv lambda x.x$|Identity]], so
- $\text{if} \ (p) \ \text{then} \ (x) \ \text{else} \ (y) \ \equiv \ \mathrm{I} \ p \ x \ y \ \to_{\beta} \ p \ x \ y$
[[8 - Combinators - a way to eliminate 𝜆-abstraction#Cardinal combinator $ mathrm{C} equiv lambda fxy. fyx$|Cardinal]] turns [[8 - Combinators - a way to eliminate 𝜆-abstraction#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kestrel]] *($\mathrm{true}$)* into [[8 - Combinators - a way to eliminate 𝜆-abstraction#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kite]] *($\mathrm{false}$)* and vice-versa, so
- $\mathrm{not} \equiv \mathrm{C} \equiv \lambda xyz.xzy$
[[8 - Combinators - a way to eliminate 𝜆-abstraction#Mockingbird combinator $ displaystyle omega equiv lambda x. xx$|Mockingbird]] happens to function as $\mathrm{or}$, where
- $\displaystyle \mathrm{or} \ p \ q \equiv M \ p \ q \to_{\beta} p \ p \ q$
- you can read it as *'if $p$ is $\mathrm{true}$, pick $p$, else pick $q$'*
We can define $\text{and}$ in a similar manner
- $\text{and} \equiv \lambda pq. pqp$
- you can read it as *'if $p$ is $\mathrm{false}$, pick $p$, else pick $q$'

## Arithmetic in [[7 - 𝜆-Calculus|$\lambda$-calculus]]
### Church numerals: encoding natural numbers $\mathbb{N}$
![[Pasted image 20241121184415.png|500]]
The ***Church numeral*** $\displaystyle n \equiv \lambda fx. \ \ f^{(n)} \ x$ is a function that
- takes a single-argument function $f$, and
- returns the $n$-th composition of $f$,
	- i.e. $f$ composed with itself $n$ times
	- this is denoted with $\displaystyle f^{(n)}$
Such repeated compositions $\displaystyle f^{(n)}$ obey the laws of exponents,
- therefore you can use them for ***arithmetic***
You can use ***Church numeral*** $n$ as an instruction to 'repeat $n$ times'
- e.g. using [[#Pairs and Linked Lists|pair functions]] $\mathrm{pair}$ and $\text{nil}$ we can define a function $\displaystyle \lambda nx. \ n \ (\mathrm{pair} \ x) \ \text{nil}$, which
	- constructs a <u>(linked) list</u> of $n$ elements <u>all equal to </u>$x$, 
		- i.e. its bunch of [[#Pairs and Linked Lists|nested pairs]] in the form form $\langle x, \ \langle x, \ \langle \dots, \ \langle x, \ \text{nil} \rangle \rangle \rangle \rangle$
	- by repeating *'prepend another $x$ element'* $n$ times, starting from $\text{nil}$ element
By varying <u>what</u> is being repeated, and varying <u>what argument</u> that function being repeated <u>is applied to</u>, much can be achieved

### Performing addition on Church numerals: $\text{succ}$ and $(+)$
We can define a ***successor function*** as 
- $\text{succ} \ \equiv \ \lambda n f x . \ f \ (n \ f \ x) \equiv \lambda n f x . \ f^{(n+1)} \ x$
- which takes a <u>Church numeral</u> $\displaystyle n \equiv \lambda f x. \ \ f^{(n)} \ x$, and
- returns $n+1$ by adding another application of $f$ to it
We now apply $\text{succ}$ to $n$, $m$ times, to define the ***addition function***
- $\displaystyle (+) \ \equiv \ \lambda mn. \ m \ \text{succ} \ n \equiv \lambda m n f x. \ f^{(m + n)} \ x$
- so if we treat [[#Church numerals encoding natural numbers $ mathbb{N}$|Church numerals]] as <u>natural numbers</u> then $(+)$ is the addition function for $\mathbb{N}$
	- e.g. $(+) \ 2 \ 3 =_{\beta} 5$ are [[7 - 𝜆-Calculus#Equivalences|$\beta$-equivalent expressions]]
- for <u>notational convenience</u> we can use <u>infix notation</u>,
	- when working informally we can just write $m + n$
	- but we <u>know</u> it stands for $(+) \ m \ n$ <u>in actuality</u>

### Other common arithmetic functions: $(\times)$, $\text{pow}$, $\text{pred}$, $(-)$
Define ***multiplication*** of $m$ by $n$ as
- $\displaystyle (\times) \ \equiv \ \lambda m n. \  m \ (n \ +) \ 0$
- can be read as *'add $n$ to $0$ $m$ times'*
- for <u>notational convenience</u> we can use <u>infix notation</u>, i.e. $(\times) \ m \ n \ \equiv \ m \times n$
Define ***exponentiation*** $\displaystyle b^e$ as [[8 - Combinators - a way to eliminate 𝜆-abstraction#Identity once-removed ($ displaystyle mathrm{I {*}}$) and Thrush ($ mathrm{T}$) combinators|Thrush]], i.e. $\mathrm{pow} \equiv \mathrm{T} \equiv \lambda be.eb$
- $\text{pow} \equiv \lambda be. eb \ \to_{\beta \eta}^{*} \ \lambda b e f x. \ (f^{(b)})^{(e)} \ x$
- can be read as *'compose $f^{(b)}$ with itself $e$ times, and then apply that to $x$'*

Define the ***predecessor*** function $\text{pred}$, where $\text{pred} \ 0 = 0$ and $\text{pred} \ n = n - 1$ for $n \geq 1$
- Define helper <u>"shift-and-increment"</u> $\phi \equiv \lambda t. \langle \text{snd} \ t , \ \mathrm{succ} \circ \mathrm{snd} \ t \rangle$
	- It maps [[#Pairs and Linked Lists|pair]] $\langle m, n \rangle$ to $\langle n,n+1 \rangle$
- $\displaystyle \mathrm{pred} \equiv \lambda n. \ \mathrm{fst} \ (n \ \phi \ \langle 0,0 \rangle)$
	- This applies <u>"shift-and-increment"</u> to $\langle 0,0 \rangle$, a total of $n$ times
	- Then selects the <u>first element</u> which is <u>one-behind</u> the <u>second element</u>
We now apply $\text{pred}$ to $m$, $n$ times, to define the ***subtraction function***
- $\displaystyle (-) \ \equiv \ \lambda mn. \ n \ \text{pred} \ m$
- where $(-) \ m \ n$ yields $m - n$ when $m > n$ and $0$ otherwise
- for <u>notational convenience</u> we can use <u>infix notation</u>, i.e. $(-) \ m \ n \ \equiv \ m - n$

## Pairs and Linked Lists
We can use [[8 - Combinators - a way to eliminate 𝜆-abstraction#Viero ($ displaystyle mathrm{V}$), Robin ($ displaystyle mathrm{R}$) and Finch ($ displaystyle mathrm{R}$) combinators|Viero]] as our ***pair constructor***, i.e. $\mathrm{pair} \equiv \mathrm{V} \equiv \lambda xyz.zxy$, so
- $\displaystyle (\mathrm{V} \ x \ y) \equiv \mathrm{pair} \ x \ y$ represents the pair $\langle x,y \rangle$, such that 
	- applying [[8 - Combinators - a way to eliminate 𝜆-abstraction#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kestrel]] *($\mathrm{true}$)* will select the <u>first element</u>, and 
	- applying [[8 - Combinators - a way to eliminate 𝜆-abstraction#Kestrel ($ displaystyle mathrm{K}$) and Kite ($ displaystyle mathrm{KI}$) combinators $ displaystyle mathrm{K} equiv lambda xy.x$, $ mathrm{KI} = lambda xy.y$|Kite]] *($\mathrm{false}$)* will yield the <u>second element</u>, so
- $\text{fst} \equiv \mathrm{K} \equiv \lambda xy.x$,
- $\mathrm{snd} \equiv \mathrm{KI} \equiv \lambda xy.y$
We can then define,
- the ***empty list*** element $\text{nil} \equiv \lambda x.\mathrm{K}$ and
- a check for it $\mathrm{isNill} \equiv \lambda x.x(\lambda yz.\mathrm{KI})$
With this, we can create ***linked lists*** as nested ***pairs***,
- with the <u>second</u> element being either $\mathrm{nil}$
- or another pair which follows the same rules
So a ***linked list*** of $n$ elements $[x_{1}, \dots, x_{n}]$ would be encoded as:
- $\displaystyle \langle x_{1}, \langle \dots,\langle x_{n}, \mathrm{nil} \rangle \rangle \rangle$
