---
title: "Week3 - Proof Patterns"
---
{{< katex />}}

# Week 3 - Proof Patterns

This week we focus a lot on proof patterns since it is a **very** important topic. We also quickly review predicate logic.

## Resources

+ Slides: [pdf](/dm25_resources/week3/slides.pdf)
+ Proof Examples: [pdf](/dm25_resources/week3/proofexamples.pdf)
+ Kahoot: [link](https://create.kahoot.it/details/976378f9-ef78-4bab-b804-4d0534633cd9) made by Tobias Steinbrecher <3

## Notes on Last Exercise Sheet

+ Overall the submissions were very good (:
+ Be careful when you apply rules to larger formulas. You still need to keep the brackets. For example $\lnot (A \land B) \lor C$ becomes $(\lnot A \lor \lnot B) \lor C$ when using de Morgans rule!
+ By now you probably figured out that the grading scheme can be very strict, so don't be too discouraged if you lost a lot of points. 

## Proving Proof Patterns with Propositional Logic

Okay, we we already know a lot of proof patterns. But what if we want to create our own one and show that it is actually correct? Or what if someone gives us a proof pattern they came up with and we want to show them that it does not work?

We can do both of those things using propositional logic! I will explain the idea first and then give examples for both proving and disproving a proof pattern.

When you are asked to evaluate a proof pattern, it will always be of the following form: It states what statement $S$ it wants to prove and then gives some other statements. Then it claims if we prove those other statements, we have actually proven the statement $S$. For example, a proposed proof pattern might look like this:


{{% hint %}}
We want to prove the statement $S$, we first prove another statement $T$. Then we show that $T \implies S$
{{% /hint %}}

This proof pattern consists of two parts (which should prove $S$):

> 1. We prove $T$  
> 2. We prove $T \implies S$

You might also see proof patterns with more steps and parts. Don't get confused! The first step is always to find out what statements the pattern consists of.

Great, but how do we work with the proof pattern now? Here is where the logic part comes in. We can actually rewrite the pattern using propositional logic. To do this, we take each of the statements, convert them to propositional logic and then combine them with $\land$. To avoid confusion, we use the letters $A, B, C, \dots$ in our formulas and let their truth values correspond to the truth of the statements. For our example proof pattern this would look like this:

{{% hint %}}
We choose the symbol $A$ and $B$, which correspond to the statements $S$ and $T$ and their truth values respectively. Now we rewrite the statements in propositional logic:
  
 1. "We prove $T$" becomes $B$
 2. "We prove $T \implies S$" becomes $B \rightarrow A$  
 
Combining those we get: $F = B \land (B \rightarrow A)$
{{% /hint %}}

Okay, we tranformed our statements to a formula $F$. But how do we reason about the correctness of the proof pattern? There is actually a very simple rule:

{{% hint %}}
The proof pattern is correct exactly if:

$$
    F \vDash A
$$

Where $F$ is the formula we get from the proof pattern and $A$ is the logical variable corresponding to the statement we want to prove. This means we can disprove a proof pattern by showing:

$$
F \nvDash A
$$

{{% /hint %}}

Why does this work? We want to show that our statement $S$ is true if we proved all the other statements. Our formula $F$ evaluating to true represents the fact that all the statements are true (since we "linked" each propositional variable to a statement and combined them with $\land$). If $F \vDash A$ then this means that if all our statements are actually true ($F$ is true), then $S$ must also be true ($A$ is true).
If the logic consequence does not hold, then there must exist some truth assignment where $F$ gives true but $A$ is set to false. This means that $S$ can be false even if all our statements are true, which means the pattern is not valid.

{{% hint %}}
If the statement you want to show is not simply one statement $S$, but for example $S \implies T$, then you need to transform that to propositional logic too and show the consequence. This should look like:

$$
    F \vDash (A \rightarrow B)
$$
{{% /hint %}}

So let's finish our small example. We show that $F \vDash A$:

$$
\begin{align*}
    F & = B \land (B \rightarrow A) \\\\
    & \equiv B \land (\lnot B \lor A) \\\\
    & \equiv (B \land \lnot B) \lor (B \land A) \\\\
    & \equiv \bot \lor (B \land A) \\\\
    & \equiv B \land A \\\\
    & \vDash A
\end{align*}
$$

### Step by Step

Let's go over the method step by step again before we do some examples:

{{% steps %}}
1. ## Extract the Statements
   See what statement $S$ should be proved by the pattern. Then find all the statements we use to prove that statement. (It does not need to be named $S$, we just assume that here for simplicity.)

2. ## Transform the Statements to Prop. Logic
   Define propositional variables $A,B, \dots$ for the statements. Transform each statement individually and then combine them with $\land$ to get a formula $F$.

3. ## (Dis)prove the Cogical Consequence
   You can prove $F \vDash A$ either by equivalence transformations or truth tables. You can disprove it by finding a truth assignment where $F$ gives true but $A$ is set to false.   
{{% /steps %}}

### Example 1: Proving Correctness

This is an exercise from 2020. We get the following proof pattern and want to show that it is correct:

{{% hint %}}
To prove $S$: Find statements $U$ and $T$. Prove $T$, assuming $S$ is false. Prove $U$, assuming $S$ is false. Show that not both $T$ and $U$ are true.
{{% /hint %}}

**Step 1:** We have three statements here:

1. not S $\implies$ T
2. not S $\implies$ U
3. not both U and T are true

Those statements should show S.

**Step 2:** We use $A$ for $S$, $B$ for $T$ and $C$ for $U$. The formulas are:

1. $\lnot A \rightarrow B$
2. $\lnot A \rightarrow C$
3. $\lnot (B \land C)$

Combining them we get:

$$
    (\lnot A \rightarrow B) \land (\lnot A \rightarrow C) \land \lnot (B \land C)
$$

**Step 3:** To show that the proof pattern is correct we now need to show:

$$
    (\lnot A \rightarrow B) \land (\lnot A \rightarrow C) \land \lnot (B \land C) \vDash A
$$

You can use a truth table to show that this holds. This shows that the proof pattern is indeed correct.

### Example 2: Disproving Correctness

We want to show that the following proof pattern is incorrect:

{{% hint %}}
To prove $S \implies T$ we find a statement $R$ and prove that R is true. Then we show that if $R$ is true, $S$ must be true. Finally, we show that at least one of $R$ and $T$ must be true.
{{% /hint %}}

**Step 1:** We want to show $S \implies T$ using three statements:

1. $R$ is true
2. $ R\implies S$
3. $R$ or $T$ (or both) must be true

**Step 2:** We use $A$ for $S$, $B$ for $T$ and $C$ for $R$. The formulas are:

1. $C$
2. $C \rightarrow A$
3. $C \lor B$

Combined we get:

$$
C \land (C \rightarrow A) \land (C \lor B)
$$

**Step 3:** We want to show that:

$$
    C \land (C \rightarrow A) \land (C \lor B) \nvDash A \rightarrow B
$$

Note that we transformed the statement the pattern tries to prove into $A \rightarrow B$.

For the truth assignment $A=1, B=0, C=1$ the left hand side evaluates to true, but the right hand side evaluates to false. This shows that the proof pattern is **not** valid.


## Proof Patterns

Read the script and look at [discmath.ch](https://discmath.ch/content/ch2/proof-patterns) for more examples. The proof patterns are very standard so you can also find good explanations on the internet.

## Exercise Sheet Recommendations

Do all the exercises! **3.6 2)** is a three-star, but actually not that difficult (and very similar to the example in the script if you need a hint), so definitely try it!
