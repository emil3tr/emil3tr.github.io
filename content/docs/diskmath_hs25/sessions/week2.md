---
title: "Week2 - Predicate Logic"
---
{{< katex />}}

# Week 2 - Predicate Logic

This week we recap propositional logic and discuss the formalism needed in the exercises. Then I introduce predicate logic and we practice working with quantifiers and predicates.

## Resources

+ Slides: [pdf](/dm25_resources/week2/slides.pdf)
+ Kahoot: [link](https://create.kahoot.it/share/dm-week1and2/63789ddd-617d-4d85-81da-33e8c8c4ff93)
+ Exercises: [pdf](/dm25_resources/week2/exercises.pdf)

## Notes on Last Exercise Sheet

+ You only need to hand in the bonus. If you need feedback for another exercise please tell me directly, per default I will only correct the bonus.
+ Read the task description carefully and make sure you satisfy everything it asks from you. If the tasks says "argue by comparing truth tables" you need to compute a truth table and argue with a sentence.
+ Every proof should end with a sentence like "This is what I showed and therefore I conclude the statement is true". Make it clear what exactly you proved and why you think you proved it.
+ Use correct notation. Don't make up your own abbreviations. (If you do so, you need to explain them.)
+ When you do truth tables, start with 000 at the top and end with 111 at the bottom (convention).
+ In general, your submissions this week looked very good and I could see that most of you understood the topic well [:

## Predicate Logic

Last week we worked a lot with propositional logic and symbols like $\lnot , \lor , \land , \rightarrow ,  A , B$. But those types of formulas are very limited in their expression. **Predicate Logic** adds new symbols which allow us to express more concepts in our formulas:

**Predicates** are functions $U^k \rightarrow \{0,1\}$. They take k values from the universe $U$ and then give a truth value depending on them. Whenever we want to work with predicates we need to specify the **universe** we are working in.

{{% hint info %}}
An easy way to define a predicate is specifying when exactly it yields true. For example
$$
P(x)=1 \Longleftrightarrow x \thinspace \text{is prime}
$$
defines a precidate P that gives true exactly when its input is prime.
{{% /hint %}}

{{% hint warning %}}
The universe is very important! Interpreting the formula $\forall x \exists y (y < x)$ with $U=\mathbb{N}$ or $U=\mathbb{Z}$ makes a big difference.
{{% /hint %}}

**Quantifiers** allow us to reason about the elements in the universe:

+ $\forall x P(x)$ means "$P(x)$ is true *for all* $x$ in $U$"
+ $\exists x P(x)$ means "there exists some (at least one) $x$ in $U$ for which $P(x)$ is true.

{{% hint warning %}}
$\exists x$ does **not** mean "exactly one"
{{% /hint %}}

### Negating Quantors

When we negate a quantor, we pull the $\lnot$ inside and change the quantor. So
$$ \lnot \forall x P(x) \equiv \exists x \lnot P(x) $$
and
$$ \lnot \exists x P(x) \equiv \forall x \lnot P(x) $$

{{% hint info %}}
Make sure to read the chapters in the script about predicate logic. If you need some more practice, look at [the page on discmath.ch](https://discmath.ch/content/ch2/predicate-logic)
{{% /hint %}}

## Exercise Sheet Recommendations

All exercises this week are important! **2.4** is a very fun riddle, but don't worry if you cannot solve it. For **2.3**, make sure that you only apply **one** rule per step and apply them **exactly** as in Lemma 2.1. Specifically:

+ Double negation ($\lnot \lnot A \equiv A$) is a rule.
+ Associativity is a rule. You cannot just reorder brackets.
+ The rules in Lemma 2.1 all have a direction. If the rule says $A \lor (A \land B) \equiv A$ then it says exactly that, and **not** $(A \land B) \lor A \equiv A$.

Also read carefully exactly what requirements your final formula needs to fulfill. The task is very doable, so be sure to be exact so you get your well-deserved bonus points.