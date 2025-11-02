---
title: "Week6 - Posets, Functions, Countability"
---
{{< katex />}}

# Week6 - Posets, Functions, Countability

This week we practice posets and functions and then learn how to prove countability / uncountability.

## Resources

+ Exercises + Solutions: [pdf](/dm25_resources/week6/exercises.pdf)
+ Kahoot: [link](https://create.kahoot.it/details/0b6165df-bd22-463d-9947-abbf7eabc1cd?drawer=)
+ Slides: [pdf](/dm25_resources/week6/slides.pdf)
+ Exercise Sheet Questions [link](https://docs.google.com/forms/d/e/1FAIpQLSeA_2dVb5aWD30xELkvIQcuPQkRdHNg91RZTCrwqUdsJvk8hQ/viewform?usp=publish-editor)

There is a great Section about Countability on [discmath.ch](https://discmath.ch/content/ch3/countability). You can find more examples there. 
If cannot quite wrap your head around the concept of different infinities, you can look at this [video](https://m.youtube.com/watch?v=OxGsU8oIWjY) on youtube.

## Notes on Last Exercise Sheet

+ Good work overall
+ Don't just start with some a and b. Write "Take any a,b ..." or "This holds for any a,b in ..." or something similar.
+ **Jusify your steps!!**
+ Don't make it more complicated than it needs to be. Make sure you exactly understand what you need to show before you start.

## Proving Countability of a Set

We know that a set $A$ is countable if $A \preccurlyeq \mathbb{N}$, which we can show by finding an injection from
$A$ to $\mathbb{N}$. But we can also find an injection from $A$ to *any* countable set:

{{% hint %}}
If there is an injection $\varphi : A \rightarrow B$ and $B$ is countable, then $A$ is also countable.
{{% /hint %}}

*Proof:* Since $B$ is countable we know $B \preccurlyeq \mathbb{N}$ and since we found an injection from $A$ to $B$ we
also know $A \preccurlyeq B$. Now the transitivity of $\preccurlyeq$ implies $A \preccurlyeq \mathbb{N}$.

This means we can use some of the countable sets we already know as $B$. Some useful ones are: $\mathbb{N}$, $\\{ 0,1 \\}^{*}$ (the set
of finite bitstrings), $\mathbb{N} \times \mathbb{N}$ or any other countable set that you can create using the rules in the script.
So proving countability usually goes like this:

{{% steps %}}
1. ## Find the Injection
    This is the hardest part. Get an intuition about the set and find the injection $\varphi$.

2. ## Prove that it is a Function
    To prove that $\varphi$ is a function we need to show that it is *well-defined* and *totally defined*. Even if
    those proprties might seem obvious to you, you still need to prove them!

3. ## Prove the Injectivity
    Finally, prove that $\varphi$ is actually an injection.

4. ## Conclusion
    Conclude with the argument above that the set is countable.
{{% /steps %}}

## Proving Uncountability

If we want to prove uncountability we first need a lemma:

{{% hint %}}
If $B$ is uncountable and there is an injection $\varphi : B \rightarrow A$ from $B$ to $A$ then $A$ is
also uncountable.
{{% /hint %}}

*Proof:* Since there is an injection from $B$ to $A$ we know that $B \preccurlyeq A$. Now assume $A$ was countable.
Then $A \preccurlyeq \mathbb{N}$ and per injectivity of $\preccurlyeq$ we get $B \preccurlyeq \mathbb{N}$. But this means
that $B$ is countable, which is a contradiction.

{{% hint success %}}
What you just saw is the **magic sentence**. Whenever you prove uncountability you will copy-paste this
sentence at the start / end of the proof. (Either from your brain or cheatsheet.)
{{% /hint %}}

So now we have a method to prove uncountability. Most of the time you will use $\\{ 0,1 \\}^{\infty}$ as your set $B$.

{{% steps %}}
1. ## Find the Injection
    This is the hardest part. Get an intuition about the set and find the injection $\varphi$.

2. ## Prove that it is a Function
    To prove that $\varphi$ is a function we need to show that it is *well-defined* and *totally defined*. Even if
    those proprties might seem obvious to you, you still need to prove them!

3. ## Prove the Injectivity
    Finally, prove that $\varphi$ is actually an injection.

4. ## Conclusion
    Conclude with the **magic sentence** above that the set is countable.
{{% /steps %}}

See that the steps are basically the same as above? Yes! Proving uncountability and countability is not actually *that*
difficult. After you understoof the set you are working with and found the injection, the rest is mostly just mechanical work.

Look at [discmath.ch](https://discmath.ch/content/ch3/countability) for a more in-depth guide and some useful tips!

## Exercise Sheet Recommendations

All the exercises this week are good (and important). Don't skip **6.6**, a lot of (old) exam exercises are similar to it.
