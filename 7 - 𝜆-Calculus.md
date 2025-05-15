***Lambda expressions*** are composed of:
- variables $v_1,v_2, \dots$;
- the abstraction symbols $\lambda$ (lambda) and $.$ (dot);
- parentheses $()$.
The set of lambda expressions, $\Lambda$, can be [defined inductively](https://en.wikipedia.org/wiki/Recursive_definition "Recursive definition"):
- Variable:       $x \ \text{is a variable} \implies x\in \Lambda$;
- Abstraction: $\displaystyle x \ \text{is a variable and} \ M\in \Lambda \implies ( \lambda x . M) \in \Lambda$;
- Application:  $\displaystyle M,N \in \Lambda \implies ( M \ N ) \in \Lambda$;
Or in [Backus–Naur form](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form "Backus–Naur form"), written as
```BNF
 <expression>  ::= <abstraction>
                 | <application> 
                 | <variable>
 <abstraction> ::= λ <variable> . <expression>
 <application> ::= ( <expression> <expression> )
 <variable>    ::= v1 | v2 | ...
```

# Conventional Notation
To keep the notation of lambda expressions uncluttered, the following conventions are usually applied:
- Outermost parentheses are dropped: $M\ N$ instead of $(M\ N)$
- Applications are assumed to be <u>left associative</u>: $M\ N \ P= ((M \ N) \ P)$
- When all variables are single-letter, the space in applications may be omitted: $xyz = x \ y \ z$
- A <u>lambda abstraction</u> has a <u>lower precedence</u> than an <u>application</u>: $\lambda x.M \ N = \lambda x. (M \ N)$
- A <u>sequence of abstractions</u> is <u>contracted</u>: $\lambda x. \lambda y. \lambda z. M = \lambda xyz. M$

# Free and bound variables
The abstraction operator $\lambda$ <u>binds its variable</u> wherever it occurs <u>in the body</u> of the abstraction. Variables that fall <u>within the scope</u> of an abstraction are said to be ***bound***. All other variables are called ***free***. A variable is bound by its "nearest" abstraction.
- e.g. in $\lambda y. x x y$, the $y$ is ***bound*** and the $x$ is ***free***
- e.g. in $\lambda x. y (\lambda x. xz)$ the $x$ is ***bound*** to the second lambda

## Set of free variables $\text{FV}(M)$
The ***set of free variables*** of a <u>lambda expression</u> $M$ is denoted as $\text{FV}(M)$ and <u>defined by recursion</u> on the structure of the terms, as follows:
1. $\text{FV}(x) = \{ x \}$
2. $\text{FV}(\lambda x. M) = \text{FV}(M) \setminus \{ x \}$
3. $\text{FV}(M \ N) = \text{FV}(M) \cup \text{FV}(N)$

## Closed lambda expressions, i.e. combinators
An expression that contains <u>no free variables</u> is said to be ***closed***. They are also known as ***combinators*** and are equivalent to terms in [combinatory logic](https://en.wikipedia.org/wiki/Combinatory_logic "Combinatory logic").

# Reduction
The meaning of <u>lambda expressions</u> is defined by how expressions can be ***reduced***.

There are three kinds of ***reduction***:
- [[#$ alpha$-conversion|$\alpha$-conversion]]: changing bound variables (**alpha**);
- [[#$ beta$-reduction|$\beta$-reduction]]: applying functions to their arguments (**beta**);
- [[#$ eta$-reduction|$\eta$-reduction]]: which captures a notion of extensionality (**eta**).

## Equivalences
We also speak of the resulting ***equivalences***: two expressions are ***equivalent***, if they can be ***reduced*** into the same expression
- $\displaystyle M_{1} =_{\alpha} M_{2} \iff \exists M'. \ M_{1} \to_{\alpha}^{*} M' \ \text{and} \ M_{2} \to_{\alpha}^{*} M'$
- $\displaystyle M_{1} =_{\beta} M_{2} \iff \exists M'. \ M_{1} \to_{\beta}^{*} M' \ \text{and} \ M_{2} \to_{\beta}^{*} M'$
- $\displaystyle M_{1} =_{\eta} M_{2} \iff \exists M'. \ M_{1} \to_{\eta}^{*} M' \ \text{and} \ M_{2} \to_{\eta}^{*} M'$
And we can even combine them to achieve more *complete* notions of ***equivalence***
- $\displaystyle M_{1} =_{\beta \eta} M_{2} \iff \exists M'. \ M_{1} \to_{\beta \eta}^{*} M' \ \text{and} \ M_{2} \to_{\beta \eta}^{*} M'$

## $\alpha$-conversion
***Alpha-conversion***, sometimes known as ***alpha-renaming***, allows bound [[#Free and bound variables|variable names]] to be changed. It is subject to these rules
-  only rename variable occurrences bound by the <u>same abstraction</u>
	- e.g. $\displaystyle \lambda x. \lambda x. x =_{\alpha} \lambda y . \lambda x. x$ or $\displaystyle \lambda x. \lambda x. x =_{\alpha} \lambda x. \lambda y. y$ but <u>not</u> $\displaystyle \lambda x. \lambda x. x =_{\alpha} \lambda y . \lambda x. y$ 
- you <u>cannot</u> rename a <u>variable</u> if it will result in a <u>different</u> $\lambda$ capturing it
	- e.g. you <u>cannot</u> do $\lambda x. \lambda y. x =_{\alpha} \lambda y. \lambda y. y$

### Substitution
***Substitution***, written $E[R/x]$, is the process of replacing all [[#Free and bound variables|free occurrences]] of variable $x$ in the expression $E$ with expression $R$. ***Substitution*** on terms of the lambda calculus is defined by recursion on the structure of terms, as follows (note: $x$ and $y$ are *only* [[#Free and bound variables|variables]] while $M$ and $N$ are any $\lambda$-expression)
$$\displaystyle 
\begin{align}
x[\textcolor{red}{M}/y] &\equiv \begin{cases}
\textcolor{red}{M} & x=y \\
x & x \neq y
\end{cases} \\
(\textcolor{red}{M_{1}} \ \textcolor{green}{M_{2}})[\textcolor{blue}{N} / x] &\equiv (\textcolor{red}{M_{1}}[\textcolor{blue}{N} / x]) \ (\textcolor{green}{M_{2}} [\textcolor{blue}{N} / x]) \\
(\lambda x. \textcolor{red}{M})[\textcolor{green}{N} /y] &\equiv \begin{cases}
\lambda x. \textcolor{red}{M} & x=y \\
\lambda x. (\textcolor{red}{M}[\textcolor{green}{N} /y]) & x\neq y \ \text{and} \ x \not\in \text{FV}(N) \\
\lambda z. (\textcolor{red}{M}[z/x][\textcolor{green}{N} /y]) & \begin{aligned}
&x\neq y \ \text{and} \ x \in \text{FV}(N), \\
&\text{for some} \ z \neq x, z \neq y, z \not\in \text{FV}(M), z \not\in \text{FV}(N) 
\end{aligned}
\end{cases} \\
\end{align}
$$
To ***substitute*** into a lambda abstraction, it is sometimes necessary to [[#$ alpha$-conversion|$\alpha$-convert]] the expression.
- e.g. its <u>wrong</u> for $\displaystyle (\lambda x.y)[x / y]$ to result in $(\lambda x.x)$, because $x$ was meant to be [[#Free and bound variables|free not bound]]
- instead the correct substitution is $(\lambda z.x)$ up to [[#$ alpha$-conversion|$\alpha$-equivalence]]

## $\beta$-reduction
***Beta-reduction*** captures the idea of <u>function application</u>. It can be defined in terms of [[#Substitution|substitution]] where the ***$\beta$-reduction*** of $\displaystyle (\lambda x. M) \ N$ is $M[N / x]$.
- e.g. $\displaystyle ((\lambda n. n \times 2) \ 7) \longrightarrow _{\beta} 7 \times 2$
From there, you can derive a couple more rules to make performing ***$\beta$-reduction*** more convenient in different scenarios:
$$\large 
\begin{gathered}
\begin{prooftree} 
\AxiomC{$\textcolor{red}{M} \longrightarrow_{\beta} \textcolor{green}{M'}$}
\UnaryInfC{$\lambda x. \textcolor{red}{M} \longrightarrow_{\beta} \lambda x. \textcolor{green}{M'}$}  
\end{prooftree} &
\begin{prooftree} 
\AxiomC{$\textcolor{red}{M} \longrightarrow_{\beta} \textcolor{green}{M'}$}
\UnaryInfC{$\textcolor{red}{M}N \longrightarrow_{\beta} \textcolor{green}{M'}N$}  
\end{prooftree} &
\begin{prooftree} 
\AxiomC{$\textcolor{red}{N} \longrightarrow_{\beta} \textcolor{green}{N'}$}
\UnaryInfC{$M\textcolor{red}{N} \longrightarrow_{\beta} M\textcolor{green}{N'}$}  
\end{prooftree} &
\begin{prooftree}
\AxiomC{$\textcolor{red}{M} =_{\alpha} \textcolor{red}{M'}$}
\AxiomC{$\textcolor{red}{M'} \longrightarrow_{\beta} \textcolor{green}{N'}$}
\AxiomC{$\textcolor{green}{N'} =_{\alpha} \textcolor{green}{N}$}
\TrinaryInfC{$\textcolor{red}{M} \longrightarrow_{\beta} \textcolor{green}{N}$}  
\end{prooftree}
\end{gathered}
$$
### Multiple $\beta$-reductions: $\displaystyle \to_{\beta}^{*}$
If we can reduce expression $M$ into expression $M$ with ***multiple $\beta$-reductions***, we express that fact with $\displaystyle M \to_{\beta}^{*} M'$, with the $*$ being shorthand for the sequence of ***$\beta$-reductions***.

### Reflexivity of [[#$ alpha$-conversion|$\alpha$-conversion]]
![[Pasted image 20241121132159.png|100]]

### Transitivity of [[#$ beta$-reduction|$\beta$-reduction]]
![[Pasted image 20241121132238.png|250]]

## $\eta$-reduction
***Eta-reduction*** expresses the idea of [extensionality](https://en.wikipedia.org/wiki/Extensionality "Extensionality"), which in this context is that two functions are the **same** IFF they give the <u>same result</u> for <u>all arguments</u>.
$$\large
\begin{prooftree} 
\AxiomC{$\displaystyle x \not\in \text{FV}(\textcolor{red}{M})$}
\UnaryInfC{$\displaystyle  (\lambda x. \textcolor{red}{M} x) \longrightarrow_{\eta} \textcolor{red}{M}$}  
\end{prooftree}
$$

## Redexes and Reducts
The term ***redex***, short for **_reducible expression_**, refers to subterms that can be reduced by one of the <u>reduction rules</u>. For example, $(\lambda x.M)$ is a ***$\beta$-redex*** in expressing the [[#Substitution|substitution]] of $N$ for $x$ in $M$; if $x$ is [[#Free and bound variables|not free]] in $M$, $\lambda x.Mx$ is an ***$\eta$-redex***. The expression to which a <u>redex</u> reduces is called its ***reduct***; using the previous example, the ***reducts*** of these expressions are respectively $M[N/x]$ and $M$.

### Innermost and Outermost $\beta$-redexes
Take the <u>redex</u> $\displaystyle E = (\lambda x. M) \ N$, 
- any <u>redex</u> in $M$ or $N$ is ***inside*** of the <u>redex</u> $E$  
- the <u>redex</u> $E$ is ***outside*** of any <u>redex</u> in $M$ or $N$
Then we can use these notions to say that
- A <u>redex</u> is ***outermost*** if there are no *larger* <u>redexes</u> ***outside*** it which *contain* it
- A <u>redex</u> is ***innermost*** if there are no <u>redexes</u> ***inside*** it which it *contains*
![[Pasted image 20241121142017.png|500]]

### Head & Internal $\beta$-redexes
A <u>redex</u> $r$ is in the ***head position*** in an expression $E$ if it has the following shape:
- $\displaystyle \lambda x_{0}\dots \lambda x_{n} . \textcolor{red}{(\lambda x. N) \ M} \ M_{0} \dots M_{m}$, where $n,m \geq 0$ 
	- Here, $\displaystyle r \equiv \textcolor{red}{(\lambda x. N) \ M}$ is the ***head position*** and is called the ***head $\beta$-redex***
- A ***head $\beta$-reduction*** is a [[#$ beta$-reduction|$\beta$-reduction]] applied to the ***head position***, e.g. here applied to $\displaystyle r \equiv \textcolor{red}{(\lambda x. N) \ M}$
	- e.g. here it would be $\displaystyle \lambda x_{0}\dots \lambda x_{n} . \textcolor{red}{(\lambda x. N) \ M} \ M_{0} \dots M_{m} \ \longrightarrow \ \lambda x_{0}\dots \lambda x_{n} . \textcolor{red}{N[M / x]} \ M_{0} \dots M_{m}$
- Any other [[#$ beta$-reduction|$\beta$-reduction]] is called an ***internal $\beta$-reduction*** and that <u>redex</u> is called an ***internal $\beta$-redex***

# Confluence (Church–Rosser property)
The property of ***confluence*** is a general [rewriting systems](https://en.wikipedia.org/wiki/Rewriting) property. The idea of ***confluence*** is that, if $a$ <u>reduces</u> to both $b$ and $c$ in $\geq 0$ rewrite steps then both <u>reducts</u> must in turn reduce to some common $d$.
![[Pasted image 20241121132641.png|200]]![[Pasted image 20241121133521.png|200]]
When applied to [[#Reduction|lambda calculus reductions]], this property is known as the ***Church–Rosser property***. It has been stated and proven for
- <u>Confluence</u> of [[#$ beta$-reduction|$\beta$-reductions]]: $\displaystyle \forall M,M_{1},M_{2}. \ \ M \to_{\beta}^{*} M_{1} \ \text{and} \ M \to_{\beta}^{*} M_{2} \implies \exists M'. \ M_{1} \to_{\beta}^{*} M' \ \text{and} \ M_{2} \to_{\beta}^{*} M'$
- <u>Confluence</u> of [[#$ eta$-reduction|$\eta$-reductions]]: $\displaystyle \forall M,M_{1},M_{2}. \ \ M \to_{\eta}^{*} M_{1} \ \text{and} \ M \to_{\eta}^{*} M_{2} \implies \exists M'. \ M_{1} \to_{\eta}^{*} M' \ \text{and} \ M_{2} \to_{\eta}^{*} M'$
- <u>Confluence</u> of $\beta \eta$<u>-reductions</u>: $\displaystyle \forall M,M_{1},M_{2}. \ \ M \to_{\beta \eta}^{*} M_{1} \ \text{and} \ M \to_{\beta \eta}^{*} M_{2} \implies \exists M'. \ M_{1} \to_{\beta \eta}^{*} M' \ \text{and} \ M_{2} \to_{\beta \eta}^{*} M'$

# Normalization
A lambda expression that <u>cannot</u> be [[#Reduction|reduced further]] is in ***normal form***. You can still apply [[#$ alpha$-conversion|$\alpha$-conversion]] to expressions in ***normal form***, in fact all ***normal forms*** that can be *converted* into each other by [[#$ alpha$-conversion|$\alpha$-conversion]] are defined to be <u>equal</u>. Here is a summary of ***normal forms***:

| Normal Form Type                                         | Definition                                                                                             |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| [[#$ beta eta$-Normal Form\|$\beta \eta$-Normal Form]]   | No [[#$ beta$-reduction\|$\beta$-reductions]] or [[#$ eta$-reduction\|$\eta$-reductions]] are possible |
| [[#$ beta$-Normal Form\|$\beta$-Normal Form]]            | No [[#$ beta$-reduction\|$\beta$-reductions]] are possible                                             |
| [[#Head Normal Form (HNF)\|Head Normal Form]]            | In the form of a lambda abstraction <u>whose body is not reducible</u>                                 |
| [[#Weak Head Normal Form (WHNF)\|Weak Head Normal Form]] | In the form of a lambda abstraction                                                                    |
^these ***normal forms*** form a "subset" hierarchy: $\beta \eta \text{-NF} \subset \beta \text{-NF} \subset \text{HNF} \subset \text{WHNF}$

Normal forms *(both [[#$ beta eta$-Normal Form|$\beta \eta$-NFs]] and [[#$ beta$-Normal Form|$\beta$-NFs]])* are <u>unique/equivalent</u> up to an [[#$ alpha$-conversion|$\alpha$-conversion]],
- $\forall M,N_{1},N_{2}. \ \ M \to_{\beta \eta}^{*} N_{1} \ \text{and} \ M \to_{\beta \eta}^{*} N_{2} \ \text{and} \ N_{1},N_{2} \ \text{are in} \ \beta \eta \text{-normal form} \implies N_{1} =_{\alpha} N_{2}$
- $\forall M,N_{1},N_{2}. \ \ M \to_{\beta}^{*} N_{1} \ \text{and} \ M \to_{\beta}^{*} N_{2} \ \text{and} \ N_{1},N_{2} \ \text{are in} \ \beta \text{-normal form} \implies N_{1} =_{\alpha} N_{2}$
This is *merely* a corollary to them <u>both</u> having the [[#Confluence (Church–Rosser property)|Church-Rosser property]],
- If expression $M$ has two ***NFs*** $N_{1},N_{2}$ then $M$ <u>must</u> have produced them via <u>reductions</u> *($\beta \eta$ or $\beta$ respectively)*
- Therefore by [[#Confluence (Church–Rosser property)|confluence]], $N_{1}$ and $N_{2}$ must <u>both</u> arrive at some <u>common</u> $M'$ with $\geq 0$  <u>reductions</u> *($\beta \eta$ or $\beta$ respectively)
- But since $N_{1}$, $N_{2}$ are ***NFs*** they contain no [[#Redexes and Reducts|redexes]] *($\beta \eta$ or $\beta$ respectively)* so $N_{1}$, $N_{2}$ arrive at $M'$ with exactly $0$ <u>reductions</u> *($\beta \eta$ or $\beta$ respectively)*
- Meaning they were either <u>already equal</u> *(i.e. $N_{1} \equiv M \equiv N_{2}$)* or they needed to [[#$ alpha$-conversion|$\alpha$-convert]] *(i.e. $N_{1} =_{a} M' =_{a} N_{2}$)* making them <u>equivalent</u> up to an [[#$ alpha$-conversion|$\alpha$-conversion]]
***NOTE:*** not *all* expressions *necessarily* have a ***normal form***, e.g. $(\lambda x. xx)(\lambda x.xx) \longrightarrow_{\beta} (\lambda x. xx)(\lambda x.xx)$ so this expression will *always* have a [[#Redexes and Reducts|$\beta$-redex]]

## $\beta \eta$-Normal Form
A term is in ***$\beta \eta$-Normal Form*** if no [[#$ beta$-reduction|$\beta$-reductions]] or [[#$ eta$-reduction|$\eta$-reductions]] are possible, or alternatively, if it contains no [[#Redexes and Reducts|$\beta$-redexes or $\eta$-redexes]].
- $M \ \text{is in} \ \beta \eta \text{-normal form} \ \ \coloneqq \ \ \neg \exists M'. \ M \to_{\beta} M' \ \text{or} \ M \to_{\eta} M'$
- $M \ \text{has a} \ \beta \eta \text{-normal form} \ \ \coloneqq \ \ \exists M'. \ M \to_{\beta \eta}^{*} M' \ \text{and} \ M' \ \text{is in} \ \beta \eta \text{-normal form}$

## $\beta$-Normal Form
A term is in ***$\beta$-Normal Form*** if no [[#$ beta$-reduction|$\beta$-reductions]] are possible, or alternatively, if it contains no [[#Redexes and Reducts|$\beta$-redexes]].
- $M \ \text{is in} \ \beta \text{-normal form} \ \ \coloneqq \ \ \neg \exists M'. \ M \to_{\beta} M'$
- $M \ \text{has a} \ \beta \text{-normal form} \ \ \coloneqq \ \ \exists M'. \ M \to_{\beta}^{*} M' \ \text{and} \ M' \ \text{is in} \ \beta \text{-normal form}$

## Head Normal Form (HNF)
A term is in ***Head Normal Form (HNF)*** if it contains no [[#Redexes and Reducts#Head & Internal $ beta$-redexes|head $\beta$-reducts]].
- Terms in ***Head Normal Form (HNF)*** therefore have the following shape
	- $\displaystyle \lambda x_{0}\dots \lambda x_{n} . \textcolor{red}{x} \ M_{0} \dots M_{m}$, where $x$ is a [[#Set of free variables $ text{FV}(M)$|variable]] and $n,m \geq 0$ 
- ***Head Normal Form (HNF)*** terms are not *necessarily* also [[#$ beta eta$-Normal Form|$\beta \eta$-NF]] or [[#$ beta$-Normal Form|$\beta$-NF]]

## Weak Head Normal Form (WHNF)
A term is in ***Weak Head Normal Form (WHNF)*** if its either
- in [[#Head Normal Form (HNF)|head normal form]], or
- it is a <u>lambda abstraction</u>
This means a WHNF term can contain [[#Redexes and Reducts|$\beta$-redexes]] in the body of the lambda.

# Reduction Strategies
![[Pasted image 20241121182543.png|500]]
^quick summary of main strategies

## Normal Order
- [[#$ beta$-reduction|$\beta$-reduces]] the [[#Innermost and Outermost $ beta$-redexes|leftmost + outermost redex]] first
	- i.e. whenever possible, arguments are substituted into the body of an abstraction before the arguments are reduced
- <u>Always</u> reduces a term to its [[#$ beta$-Normal Form|$\beta$-normal form]] *(if it exists)*
- Can perform computations in unevaluated function bodies
- Is not used by any programming language

## Applicative Order
- [[#$ beta$-reduction|$\beta$-reduces]] the [[#Innermost and Outermost $ beta$-redexes|leftmost + innermost redex]] first
	- therefore,  a function's arguments are always reduced before they are substituted into the function
- Does not always reduce a term to its [[#$ beta$-Normal Form|$\beta$-normal form]] *(even if exists)*
	- e.g. $\displaystyle ( \ \ \ \lambda x. y \ \ (\lambda z. (zz) \lambda z. (zz) \ \ \ )$  is reduced to itself by ***applicative order***, 
	- while [[#Normal Order|normal order]] reduces it to its [[#$ beta$-Normal Form|$\beta$-normal form]] of $y$

## Full [[#$ beta$-reduction|$\beta$-reductions]]
- Any [[#Redexes and Reducts|$\beta$-redex]] can be [[#$ beta$-reduction|$\beta$-reduced]] at any time
- This means essentially the lack of any *particular* <u>reduction strategy</u>
	- With regard to <u>reducibility</u>, "all bets are off"

## Call by Name
- Like [[#Normal Order|normal order]], but <u>no reductions are performed inside abstractions</u>
	- e.g. $\lambda x. (\lambda y.y) x$ will <u>not</u> be <u>reduced</u> by this strategy
- Does not always reduce a term to its [[#$ beta$-Normal Form|$\beta$-normal form]] *(even if exists)*
- Passes the function parameters unevaluated into the function body 
- Evaluates the passed function parameter on each use 
- Is used, with some variations, by, for example, Algol60, Haskell, R, and LaTeX

## Call by Value
- Like [[#Applicative Order|applicative order]], but <u>no reductions are performed inside abstractions</u>
	- e.g. $\lambda x. (\lambda y.y) x$ will <u>not</u> be <u>reduced</u> by this strategy
- Does not always reduce a term to its [[#$ beta$-Normal Form|$\beta$-normal form]] *(even if exists)*
- Evaluates the function parameters before passing them into the function body
- Terminates less often than [[#Call by Name|call by name]], but evaluates parameters only once
- Is used, with some variations, by, for example, C, Scheme, and OCamlX

## Call by Need
- As [[#Call by Name|call by name]], but function applications that would duplicate terms instead name the argument
	- The argument may be evaluated "when needed"
	- at which point the name binding is updated with the reduced value
- This can save time compared to [[#Normal Order|normal order]] evaluation.

## Optimal Reduction
As [[#Normal Order|normal order]], but <u>computations</u> that have the <u>same label</u> are <u>reduced simultaneously</u>
