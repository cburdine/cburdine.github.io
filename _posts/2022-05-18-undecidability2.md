---
title: 'An Introduction to Undecidability (Part 2)'
date: 2022-05-18
permalink: /posts/2022/05/undecidability2/
tags:
  - Mathematics
  - Computer Science
---

In this post, we will pick up from where we left off in [Part 1](https://cburdine.github.io/posts/2022/05/undecidability/). So far, we have introduced Turing Machines (TMs) and Universal Turing Machines (UTMs). Now let's see how they help us in proving the undecidability of the domino problem we encountered earlier.

First, recall that we are trying to show that the following question is "unanswerable", in the sense that no method exists for answering it:

> **Question:** 
> Given an _arbitrary_ set of dominoes $\mathcal{D}$ containing symbols from $\Sigma$, does there exist some method for determining if at least one balanced domino sequence from $\mathcal{D}$ exists? 

In the previous post, we gave the following proof outline: 

<ol type="I">
    <li>First, we will need to come up with some way of formally defining a what a "method" is.</li>
    <li>Next, we will need to organize all such methods into some kind of list and ensure that every method is included.</li>
    <li>Finally, we will show that if a method exists that answers the domino question, it cannot possibly be contained in the list, which is a contradiction.</li>
</ol>

In Part I, we established that every "method" for solving a problem (such as the domino problem) could be realized as a Turing machine. Next, we will figure out how to organize Turing Machines into a list.

## II. Enumerating Turing Machines
In the last post, we showed that TMs are uniquely representable by their state transition tables, and that these tables could be encoded as finite binary sequences. This construction was useful in describing how a TM could be converted into part of the input for a UTM. This binary encoding also gives rise to a natural ordering we can impose over all Turing Machines. If $M$ is a TM, let $w(M)$ denote the binary encoding of $M$. Since we can interpret $w(M)$ as a positive integer encoded in binary (say $N_M$), then we have a 1-1 correspondence between all Turing Machines and a subset of the positive integers. Note that not every integer $N$ corresponds to a TM, since $N$ represented in binary may not be a valid encoding of a TM. Because the integers are well-ordered, there must exist some smallest TM $M_1$ such that $N_{M_1} < N_M$ for every machine $M \ne M_1$. We can then find the next smallest TM $M_1$ with $N_{M_1} > N_{M_0}$ , the next smallest $M_2$ with $N_{M_2} > N_{M_1}$ , and so on. This process results in an enumeration of Turing Machines 

$$M_1, M_2, M_3, M_4, ...$$

If some $M$ is not included in this list, then we have $N_{M_{i-1}} < N_M < N_{M_i}$ for some $i$. But then the choice of $M_{i}$ is not minimal, a contradiction. Thus _every_ TM is included in this enumeration.  

Recall that the _input_ to a TM is the finite binary string initialized on the TM's tape prior to computation. The _output_ of a TM is the finite binary string left on the TM's tape when it enters the Stop state. In the special case of decision problems (where the TM outputs a "yes" or "no" answer), we will make the simplifying assumption that the output of the TM is simply the symbol left on the first cell on the TM's tape after it enters the Stop state, ignoring the rest of the output.

It we let $\lbrace 0, 1 \rbrace^*$ denote the set of finite binary input strings, it might be tempting to simply think of TMs that solve decision problems as functions of the form:

$$M : \lbrace 0, 1 \rbrace^* \rightarrow \lbrace 0, 1 \rbrace$$

that is, for every finite input string $w$ in $\lbrace 0, 1 \rbrace^*$, $M$ has an associated output, either $1$ or $0$. However, it might be possible that for some input $w$ in the set $\{0,1\}^*$, a given TM never enters the Stop state. Instead, it might get stuck in a loop, or endlessly write over more and more of the blank ("\_") cells on its tape. This is why we often refer to TMs as _partial functions_, meaning if we want to treat $M$ like a function on $\lbrace 0, 1 \rbrace^*$, then it is only defined on inputs where $M$ eventually stops. 


### Correspondence with the Real Numbers
So, it seems be the case that there are some functions of the form $f : \lbrace 0, 1 \rbrace^* \rightarrow \lbrace 0, 1 \rbrace$ that are not realizable as TMs. Recall that our goal in this proof is to eventually show that the domino problem is one of these functions. Some questions we might want to think about first are: _How rare are these kinds of functions f?_ and _Can we perhaps construct one of these?_

Let's consider the first question. Interestingly, we can show that not only are there infinitely many such functions $f$, but there are as many $f$s as there are real numbers! The proof of this is as follows: Since each item in $\lbrace 0, 1 \rbrace^*$ can be interpreted as an integer in binary, we can enumerate the members of this set as a sequence $w_1, w_2, w_3, ...$, from which we can construct the infinite binary sequence $f(w_1), f(w_2), f(w_3), ...$ and so on. Each of these binary sequences can be uniquely interpreted as [real binary numbers](https://en.wikipedia.org/wiki/Binary_number#Representing_real_numbers) on the interval $[0,1]$ by prepending "0." to them. For example:

$$100101101011010000...\ \mapsto\ (0.100101101011010000...)_{2} = 0.5886...$$

Thus, there is a 1-1 correspondence between functions $f$, and real numbers on the interval $[0,1]$. Some elementary results on the [sizes of infinite sets](https://en.wikipedia.org/wiki/Cardinality_of_the_continuum) (which we will not cover here) can extend this into a 1-1 correspondence between these functions and the set of all real numbers, $\mathbb{R}$. Previously, we said that the subset of all functions $f$ that can be realized as Turing Machines are enumerable, and thus they are in 1-1 correspondence with the positive integers, $\mathbb{N}$. However, we cannot place the set of Turing Machines in 1-1 correspondence with the real numbers on $[0,1]$ (or $\mathbb{R}$ by extension), since the real numbers on $[0,1]$ cannot be enumerated. This fact can be proven by [Cantor's diagonal argument](https://en.wikipedia.org/wiki/Cantor's_diagonal_argument):

Suppose, to the contrary, that the real numbers on the interval $[0,1]$ can be enumerated, meaning they are in 1-1 correspondence with the positive integers. Then there exists an enumeration $r_1, r_2, r_3, ...$ that includes every real number on $[0,1]$. Consider the expansion of each $r_i$ in binary form (with a leading "0."), and let $d_{i,j}$ be the $j$th binary digit of $r_i$. We proceed by constructing a real number $r_N$ on $[0,1]$, by fixing its $k$th digit to be different from $d_{k,k}$. This construction is more clear if we consider a 2D grid of the digits of each $r_i$:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-diag-reals.svg'>
</p>
</div>

As shown above, we take digits along the circled diagonal of the grid, and assign the opposite bit value to the corresponding digit in $r_N$. Because $r_N$ differs from every $r_i$ by at least one bit, $r_N$ is not contained in the enumeration $r_1, r_2, r_3, ...$. But $r_N$ is a valid real number on $[0,1]$ and must be contained in the enumeration. This is a contradiction, which indicates that the enumeration $r_1, r_2, r_3, ...$ cannot exist. Thus, the real numbers on $[0,1]$ are not in 1-1 correspondence with the positive integers $\mathbb{N}$.

### Constructing an Undecidable Function: The Halting Problem

Now let's consider the question of constructing a function $f: \lbrace 0, 1 \rbrace^* \rightarrow \lbrace 0, 1 \rbrace$ that is not realizable by a TM. We already know that TMs are partial functions, which makes things difficult due to TMs having undefined values whenever they fail to stop. We can account for this issue by introducing a new function with an element "?" added to the range to account for undefined values. In particular, for each TM $M$ we define the function $f_M: \lbrace 0, 1 \rbrace^* \rightarrow \lbrace 0, 1, ? \rbrace$, where:

$$f_M(w) = \begin{cases}\ M(w)\ & \text{if $M$ stops on input $w$ } \\ \ ?\ & \text{if $M$ does not stop on input $w$}\end{cases}$$

Using these $f_M$ functions, we can use a variation of Cantor's diagonal argument to construct a special function $f_H: \lbrace 0, 1 \rbrace^* \rightarrow \lbrace 0, 1 \rbrace$ that cannot be computed by a TM. Since TMs are enumerable, so are the $f_M$ functions ($f_{M_1}, f_{M_2}, f_{M_3}, ...$ and so on). For each $f_{M}$, we can compute the output on each binary input string $w_1, w_2, w_3, ...~$. We define $f_H$ as follows:

$$f_H(w_i) = \begin{cases}\ 0\ & \text{if }f_{M_i} =~?\quad \text{(i.e. $M_i$ never stops on input $w_i$)}\\ \ 1\ & \text{if } f_{M_i} \ne~?\quad \text{(i.e. $M_i$ stops on input $w_i$)} \end{cases}$$

By definition $f_H$ differs from each $f_{M_i}$ when evaluated on $w_i$. Note the similarities between this construction and our previous construction with the diagonal argument:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-diag.svg'>
</p>
</div>

Since $f_H$ is different from each $f_{M_i}$, it is not contained in the enumeration $f_{M_1}, f_{M_2}, f_{M_3}, ...$, and thus the function $f_H$ cannot be computed by any TM. This means $f_H$ represents an _undecidable_ problem. How should we interpret $f_H$? Well, it is easiest to think of $f_H$ as being a special case of a more general undecidable problem, called the _Halting Problem_, which was first proven to be undecidable by Alan Turing in the 1930s:

> **Halting Problem:** \
> Given a Turing Machine $M$ and input $w$, will $M$ eventually reach the Stop state when run on on input $w$?

Let $H(M,w)$ be the function that represents the answer to the Halting Problem, meaning  $H(M,w) = 1$ if $M$ eventually stops on input $w$ and $H(M,w) = 1$ if $M$ does not stop on input $w$. Using the enumerations we produced above, we see that $H(M_i,w_i) = f_H(w_i)$ for every index $i$. Since $f_H$ is undecidable, clearly $H$ must be undecidable as well.

### A Similar Proof of the Halting Problem Undecidability

In Alan Turing's original proof that the Halting problem is undecidable (see [Turing's 1936 Paper](https://cburdine.github.io/files/Turing_Paper_1936.pdf)), he employed a diagonal argument similar to the one we used here. There is also a much simpler proof method that avoids some of the formalities we introduced previously. This is the proof that is taught more often in computer science classrooms. While it is (in my opinion) slightly less rigorous, It is very insightful and certainly worth examining here.

The proof is as follows: Suppose there exists a TM $M_H$ that computes the answer to the Halting Problem. The input for $M_H$ consists of a binary-encoded TM $M$ along with an input string $w$. $M_H$ always eventually stops and outputs the correct value of $H(M_i, w_i)$ on its tape. We construct a variant of a UTM, $U$, that only takes as input a binary-encoded TM $M$. $U$ does the following: First, it simulates $M_H$ on the TM $M$ with the input of $M$'s binary encoding. If $M_H$ outputs $0$, $U$ enters an infinite loop. Otherwise, it simply outputs 0. This is shown in the following pseudo-code:

```
function M_H(M,w):
	if M will eventually stop on input w:
		return 1
	else:
		return 0

function U(M):
	if M_H(M,M) == 1:
		loop forever
	else:
		return 0
```

Now consider what happens if we run $M_H$ on the binary-encoded machine $U$ with input string consisting the same encoding of $U$:
```
M_H(U,U)
```

Suppose ``M_H(U,U)`` evaluates to 0, meaning $U$ eventually stops when run on its own encoding. In the definition of ``U`` it then must be the case that ``M_H(U,U) == 1``, meaning ``M_H(U,U)`` evaluates to 1 (i.e. $U$ never stops when run on its own encoding). Likewise if ``M_H(U,U)`` evaluates to 1, we observe that ``M_H(U,U)`` cannot possibly equal 1. In either case, we have a contradiction. This proves that $M_H$ cannot be realized by a TM.

In Part III of this blog post, we will finish up our proof that the domino problem is undecidable by using some of the arguments that we explored above.



