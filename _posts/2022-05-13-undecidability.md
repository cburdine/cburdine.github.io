---
title: 'An Introduction to Undecidability (Part 1)'
date: 2022-05-13
permalink: /posts/2022/05/undecidability/
tags:
  - Mathematics
  - Computer Science
---

In mathematics, it is often assumed that every well-posed _yes_ or _no_ question about some object or property has a definite answer. Despite this assumption, there still exist many important questions in mathematics that have failed to admit some kind of answer. Among these are some of the most challenging unsolved problems faced by modern mathematicians, such as the [Riemann Hypothesis](https://www.claymath.org/millennium-problems/riemann-hypothesis) and the [P vs. NP Problem](https://www.claymath.org/millennium-problems/p-vs-np-problem). However, there are some questions we can pose in mathematics that we can actually prove are "unprovable", meaning we can show with absolute certainty that no method for determining a yes or no answer exists. In the field of computer science, these are referred to as _undecidable problems_. In this two-part (or possibly three-part) blog post, we will be doing a deep dive into the world of undecidability and introduce some important concepts from mathematics and computer science along the way.

### The Domino Problem

Consider the following problem. Suppose we are given a finite set of dominoes $\mathcal{D} = \lbrace D_1, D_2, D_3, ... D_N \rbrace$, where each domino $D_i$ contains two finite sequences consisting of symbols from the set $~\Sigma = \lbrace \clubsuit, \spadesuit, \diamondsuit, \heartsuit \rbrace$. The first sequence is printed along the top of the domino, while the second is printed along the bottom. Sequences of length 0 are allowed. The task is to find a sequence of dominoes from $\mathcal{D}$ arranged horizontally, such that the sequence of symbols read across the top of the dominoes (from left to right) is the same as the sequence of symbols read across the bottom (from left to right). Repeats of dominoes in $D$ are allowed, but the flipping or rotation of dominoes is forbidden. If such a sequence of dominoes satisfies these conditions, we say that it is _balanced_.

For example, consider the following set of dominoes, $\mathcal{D} = \lbrace D_1, ..., D_5\rbrace$:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-dominoes1.svg'>
</p>
</div>

We can arrange dominoes from $\mathcal{D}$ in the order $D_3, D_5, D_1, D_1, D_2, D_4$. This is a balanced domino sequence, since we can read the sequence of symbols $\heartsuit\diamondsuit\spadesuit\diamondsuit\diamondsuit\clubsuit\spadesuit\clubsuit\heartsuit$ across both the top and bottom:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-dominoes2.svg'>
</p>
</div>

However, if we consider the set of dominoes $\mathcal{D}' = \lbrace D_1, ..., D_4 \rbrace$ where we exclude $D_5$, we can show that it is impossible to produce a balanced sequence:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-dominoes3.svg'>
</p>
</div>

The reasoning for this is as follows: we see that no balanced sequence of dominoes from $\mathcal{D}'$ can contain $D_2$, since no domino in the set ends with the $\clubsuit$ symbol on the bottom. After eliminating $D_2$, we can eliminate $D_4$, since no other domino contains the $\clubsuit$ symbol on the top. By similar logic, we can eliminate $D_3$, and finally $D_1$.

Now, let's consider the following question:

> **Question:** 
> Given an _arbitrary_ set of dominoes $\mathcal{D}$ containing symbols from $\Sigma$, does there exist some method for determining if at least one balanced domino sequence from $\mathcal{D}$ exists? 

If we only consider the domino sets $\mathcal{D}$ that are guaranteed to have at least one balanced sequence, then we can find one of the shortest such sequences through brute force search. First we enumerate every possible domino sequence from $\mathcal{D}$ of length 1, then length 2, then length 3, and so on, checking if each sequence is balanced. If at least one balanced sequence exists, this process is guaranteed to eventually stop. However, if we apply this method to a set of dominoes $\mathcal{D}$ with no balanced sequences, this search process will run forever. If we decide to stop the search at some large sequence length $L$ and answer in the negative, it still might be possible that the shortest balanced sequence is larger than $L$. So it seems, the difficulty of answering this question lies in finding a method for showing that _no_ balanced domino sequences exist.


### Introducing Undecidability
Surprisingly, the answer to our question above is _no, there exists no method for determining if balanced domino sequences exist for any given $\mathcal{D}$_. In other words, it can be proven that this problem is "unsolvable" for at least some set $\mathcal{D}$. This domino puzzle is an example of what computer scientists refer to as an _undecidable problem_, meaning there exists no process by which we can _decide_ whether or not solutions to it exist.

To the reader encountering the concept of undecidability for the first time, it might seem strange that we can actually _prove_ that there are no methods for determining if solutions exist to certain problems. After all, there are infinitely many methods one could propose to check if solutions exist, and showing that all of these methods fail seems to be an insurmountable task. Fortunately, we can avoid all of this exhaustive method checking by attempting a _proof by contradiction_; namely, we will show that the existence of a method that answers these kinds of questions results in some kind of logical paradox (a _contradiction_) and therefore the method cannot exist. This is how we will prove that the pomino problem is undecidable. 

To make things a bit more manageable, we will break up the proof into three steps:

<ol type="I">
    <li>First, we will need to come up with some way of formally defining a what a "method" is.</li>
    <li>Next, we will need to organize all such methods into some kind of list and ensure that every method is included.</li>
    <li>Finally, we will show that if a method exists that answers the domino question, it cannot possibly be contained in the list, which is a contradiction.</li>
</ol>

In the remainder of this post, we will cover Step I. Steps II and III will be covered in the next post (Part 2 of 2).

## I. Introducing Turing Machines
What do we mean when we say that there is a "method" for solving certain kinds of problems? In fact, how should we define what a "method" is? 

One (quite reasonable) definition for a "method" is any process that can converted into computer code and executed on a computer. This code accepts some problem parameters as input and simply returns _yes_ or _no_ as output. However, we note that not all computers or computer programs are created equal. Some computers can perform billions of operations a second, while others (such as my brain) have difficulty multiplying numbers with four or more digits. Also, some computers have interpreters that can execute code written in many different [programming languages](https://en.wikipedia.org/wiki/Programming_language), while others can only run a single type of program written in [assembly language](https://en.wikipedia.org/wiki/Assembly_language). If we think all "methods" can be converted into computer programs, how do we account for differences in the hardware executing these programs and the languages the programs are written in? 

We can resolve this by proposing a very simple computer, called a _Turing Machine_, and argue that any program run on any computer can be emulated by this machine. This is the core idea of the assertion known as the [_Church-Turing Thesis_](https://en.wikipedia.org/wiki/Church%E2%80%93Turing_thesis):

> **Church Turing Thesis:**\
 > Every computational procedure can be represented as a Turing Machine.

According to the thesis, if we want to show that some property holds for all possible computational methods, it suffices to consider all Turing Machines- but what exactly _is_ a Turing Machine? A Turing machine (TM for short) is a simple computer that operates on a magnetic tape containing an array of cells. Each cell can store a single _bit_ (binary digit) of information, either a "1" symbol, a "0" symbol, or a special blank symbol "\_". Computation is performed in discrete steps by moving a read/write head left or right on the tape, one cell at a time. We assume here that the tape begins at the left and extends infinitely to the right:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-tm.svg'>
</p>
</div>

During a computation, the read/write head of the TM is in one of a finite number of defined states. One of these states is designated the _Start_ state, and another the _Stop_ state. The TM begins in the Start state with the head positioned on the first cell.  Some finite sequence of "1"s and "0"s are initialized on the tape as input, with the remaining symbols filled in with the blank symbol ("\_"). At each step, the Turing machine reads the symbol on the tape underneath the read/write head. Then, depending on the state of the read/write head, the head will write a new symbol to the tape ("1", "0", or "\_") and then either move left or right and transition into another state. Once the machine enters the Stop state, the machine stops computing, and whatever is left on the machine's tape (excluding the "\_" symbols) is considered the output. The rules for writing symbols, moving the head, and transitioning between states can be summarized in a table, such as the one below:

| (State , Read Symbol) | Write Symbol  | Move | Next  State |
| ---------------| ------- | ---- | ------------|
| (Start, "0")   | "0"     | R    | A           |
| (Start, "1")   | "1"     | R    | B           |
| (Start, "\_")  | "\_"    | R    | Start       |
| (A, "0")       | "0"     | R    | Start       |
| (A, "1")       | "0"     | R    | Start       |
| (A, "\_")      | "0"     | L    | Stop        |
| (B, "0")       | "1"     | R    | Start       |
| (B, "1")       | "1"     | R    | Start       |
| (B, "\_")      | "1"     | L    | Stop        |


This table describes the program of a TM with four states (Start, A, B, Stop), which copies every other bit of the input one cell to the right. Notice that for each state-symbol pair (excluding the "Stop" state), the read/write head has a defined behavior. For example, running the above TM on the input sequence "00100" results in an output of "001100":

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-tm-example.svg'>
</p>
</div>

Since a TM is uniquely described by its state transition table, one could easily convert this table into a finite binary sequence of "1"s and "0"s and feed it as input into another Turing Machine. This is an important observation, as it gives rise to a special type of TM called a _Universal Turing Machine_ (UTM). A UTM is a Turing Machine that takes as input both a description of another Turing machine (encoded as a finite sequence of "1"s and "0"s) and an input for that Turing machine and then proceeds to compute the result of running the encoded Turing machine on the given input.

It is quite difficult construct the state transition table of a Universal Turing machine, and we will not do that here. However, we can at least argue that it is possible to construct a UTM on the basis of of the Church-Turing Thesis. First, we note that it is possible to write a computer program that simulates a TM in a modern programming language such as C, Python, etc. We also know from the Church-Turing thesis that a TM is capable of performing the same computation as any system running a TM simulator program. Thus, a UTM is capable of simulating any other TM, including itself.

As we will show later, UTMs are quite useful when attempting to produce contradictions, such as in the case of the domino problem. It is relatively straightforward to establish that any method for determining if a given set of dominoes $\mathcal{D}$ has a balanced sequence corresponds to a Turing Machine that takes some binary-encoded form of $\mathcal{D}$ as input, and stops with either a "1" or a "0" on its tape, indicating whether or not a balanced sequence exists.

In Part 2 of this blog post (coming soon), we will introduce some additional concepts and continue the proof that the domino problem is undecidable.


