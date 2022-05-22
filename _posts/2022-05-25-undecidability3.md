---
title: 'An Introduction to Undecidability (Part 3)'
date: 2022-05-25
permalink: /posts/2022/05/undecidability3/
tags:
  - Mathematics
  - Computer Science
---

In this post, we will pick up from where we left off in [Part 2](https://cburdine.github.io/posts/2022/05/undecidability2/) of this blog post and finish the proof that the Domino problem (introduced in [Part 1](https://cburdine.github.io/posts/2022/05/undecidability/)) is undecidable.

Recall that we originally posed the following question about solving the Domino problem:

> **Question:** 
> Given an _arbitrary_ set of dominoes $\mathcal{D}$ containing symbols from $\Sigma$, does there exist some method for determining if at least one balanced domino sequence from $\mathcal{D}$ exists? 

So far, we have claimed that the answer to the question above is _no_, meaning we can prove that there exists no method for finding balanced sequences given an arbitrary domino set $\mathcal{D}$. So far, our proof has been following this outline:

<ol type="I">
    <li>First, we will need to come up with some way of formally defining a what a "method" is.</li>
    <li>Next, we will need to organize all such methods into some kind of list and ensure that every method is included.</li>
    <li>Finally, we will show that if a method exists that answers the domino question, it cannot possibly be contained in the list, which is a contradiction.</li>
</ol>

Finally, we have arrived at the most interesting step. Let's see if we can apply some of the concepts we have explored so far.

## III. The Domino Problem is Undecidable

So far, we have established that all possible methods for solving the domino problem are realizable as Turing Machines (TMs). We have also shown that all TMs $M$ can be placed into 1-1 correspondence with the positive integers $\mathbb{N}$, meaning they can be enumerated in a sequence:

$$M_1, M_2, M_3, ....$$

We also showed that all decision problems (i.e. problems requiring a yes or no answer) can be realized as functions of the form $f: \lbrace 0, 1 \rbrace^* \rightarrow \lbrace 0, 1 \rbrace$, which can be placed in 1-1 correspondence with the real numbers $\mathbb{R}$. Even though both of these sets are infinite in size, the size of $\mathbb{R}$ is a degree of infinity larger than $\mathbb{N}$, a fact that we proved using Cantor's diagonal argument. Thus, if we were given a "random" decision problem, the probability that it is solvable by a TM is effectively $0$. Thus, it seems reasonable to assume any such problem is "guilty until proven innocent", that is, we should assume undecidability unless we can produce a TM that solves it. 

In the previous post, we also gave an example of an undecidable problem, the _Halting Problem_:

> **Halting Problem:** \
> Given a Turing Machine $M$ and input $w$, will $M$ eventually reach the Stop state when run on on input $w$?



