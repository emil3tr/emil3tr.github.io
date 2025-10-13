---
title: "Week4 - Sets"
---
{{< katex />}}

# Week4 - Sets

This week we introduce sets and the basic set operations. Then we practive proving properties / equalities of sets and talk about relations a bit.

## Resources

+ Feedback Form: [link](https://docs.google.com/forms/d/e/1FAIpQLScMBTx1By4t7mXx528I7nb5h_opOEFm7qCXl9oZ3G9RFjT87Q/viewform?usp=header)
+ Slides: [pdf](/dm25_resources/week4/slides.pdf)
+ Kahoot: [link](https://create.kahoot.it/details/29670f94-2d08-4235-a7e8-a3d7f869eb05?drawer=)
+ Exercises: [pdf](/dm25_resources/week4/exercises.pdf) 
+ Mega Summary: [pdf](/dm25_resources/week4/mega_summary.pdf)

## Notes on Last Exercise Sheet

+ Overall the submissions were quite good, most problems were not about understanding but presentation.
+ You should always use brackets to make the precedence in your formulas clear.
+ Use the correct arrows (look at week 3 for a guide).
+ **Read the task carefully!** If it says "do it this way" then you have to do it this way.
+ **Explain what you are doing!** It is not enough if the solution makes sense in you head. A proof needs to be exact and cannot leave room for interpretation.
+ Be careful about restrictions in the exercise. **You need to be precise in Diskmath!** If "<" is a predicate does not mean that ">" is also one.
+ You proof should begin with "We prove ... by showing ..." and end with "This proves the statement by ...". If you cannot describe (or at least have a general idea about) 1) what you want to prove 2) what you need to do to prove it 3) how you are going to do that, then you should not start writing the proof yet.

## About Sets

Sets are some of the most important objects in mathematics. A lot of things (like the natural numbers) are derived from set theory.
If you want to read more on how we define sets, you can look at [discmath.ch](https://discmath.ch/content/ch3/sets) or search for "ZFK Axioms" online.

So what is a *set*? For us, the hintrmal definition "a collection of well defined objects" should be enough. We write $a \in A$ if $a$ is an element of $A$.

{{% hint warning %}}
A set can only contain each element **once**. This means an object is either inside the set or not.
{{% /hint %}}

{{% hint %}}
The script contains a very good explanation for the *set operations* like $\subseteq$, $\cup$, ... Make sure that you not only understand them intuitively, but also the formal definitions.
{{% /hint %}}

### The Powerset

One of the more difficult set operations is the powerset of a set. 

{{% hint %}}
We define the powerset of a set $A$ as:
$$
    S \in P(A) \Longleftrightarrow S \subseteq A
$$
In words: "the powerset of A contains all subsets of A". 
{{% /hint %}}

Note that the powerset is a **set** which contains **sets**. It has the following interesting properties:

+ $A \in P(A)$
+ $\varnothing \in P(A)$ for any set $A$
+ $A$ is **not** necessarily a subset of $P(A)$! $P(A)$ contains **sets**, which contain elements of $A$. But it
does **not** (necessarily) contain the elements of $A$.
+ $|A| = k \implies |P(A)| = 2^{k}$

## Proving Set properties

### Showing A is Subset of B

To show that $A$ is a subset of $B$, we need to show that every element in $A$ is also in $B$. The easiest way
of doing this is by taking an arbitrary but fixed element from $A$ and then showing that it is also in $B$.
This would look something like:

{{% hint %}}
Let $x$ in A be arbitrary.
$$
\begin{align*}
    & x \in A \\\\
    \implies & \dots \\\\
    \implies & \dots \\\\
    \implies & x \in B
\end{align*}
$$
{{% /hint %}}

{{% hint warning %}}
When we say "let x be arbitrary but fixed", then we mean that we take any element of the set. This means that
all of your steps afterwards have to work for any element of the set!
{{% /hint %}}

### Showing A = B

There are two ways of showing $A = B$. The first one uses a very useful statement from the script:

$$
A = B \Longleftrightarrow A \subseteq B \land B \subseteq A
$$

This means if we need to show that $A = B$ we can simply show both $A \subseteq B$ and $B \subseteq A$ as above.

The second way is by showing directly that any element in $A$ is also in $B$ and vice versa. A lot of times this also
works but might be a little bit more difficult. We let $x$ be arbitrary and show:

{{% hint %}}
$$
\begin{align*}
     & x \in A \\\\
     \Longleftrightarrow & \dots \\\\
     \Longleftrightarrow & \dots \\\\
     \Longleftrightarrow & x \in B
\end{align*}
$$
{{% /hint %}}

Note that for this type of proof **all** your implications need to be double-sided! So you need to make sure that
all rows imply the row above and below it!

{{% hint %}}
You can find an example for this in the exercises.
{{% /hint %}}


### Using Logic

In a lot of cases we can use propositional logic to prove statements regarding sets. Those proofs will mostly follow the same pattern:

1. Use the definitions to transform a statement or set from set-language into logic-language.
2. Use propositional logic results to refactor the statement or set
3. Use the defintions again to get back to set-language

A simple example:

{{% hint %}}
Say we want to work with the set $B \cup (C \setminus A)$. Now:

$$
\begin{align*}
& x \in B \cup (C \setminus A) \\\\
\implies & x \in B \lor x \in (C \setminus A) \\\\
\implies & x \in B \lor (x \in C \land \lnot(x \in A))
\end{align*}
$$

Now we can use Lemma 2.1 to refactor the statement and then transform it into a set again with the definitions.
{{% /hint %}}

## Relations

Look at [discmath.ch](https://discmath.ch/content/ch3/relations).

## Exercise Sheet Recommendations

Do all the exercises! If you have to leave out some, then skip **4.1** and **4.7**, but otherwise try to at least think about all the exercises, because they will give you really good intuition about sets!