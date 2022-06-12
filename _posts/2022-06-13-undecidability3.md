---
title: 'An Introduction to Undecidability (Part 3)'
date: 2022-06-13
permalink: /posts/2022/06/undecidability3/
tags:
  - Mathematics
  - Computer Science
---

In this post, we will pick up from where we left off in [Part 2](https://cburdine.github.io/posts/2022/05/undecidability2/) of this blog post and finish the proof that the Domino problem (introduced in [Part 1](https://cburdine.github.io/posts/2022/05/undecidability/)) is undecidable:

> **Question:** 
> Given an _arbitrary_ set of dominoes $\mathcal{D}$ containing symbols from $\Sigma$, does there exist some method for determining if at least one balanced domino sequence from $\mathcal{D}$ exists? 

So far, we have claimed that the answer to the question above is _no_, meaning there exists no method for finding balanced sequences given an arbitrary domino set $\mathcal{D}$. So far, our proof has been following this outline:

<ol type="I">
    <li>First, we will need to come up with some way of formally defining a what a "method" is.</li>
    <li>Next, we will need to organize all such methods into some kind of list and ensure that every method is included.</li>
    <li>Finally, we will show that if a method exists that answers the domino question, it cannot possibly be contained in the list, which is a contradiction.</li>
</ol>

Finally, we have arrived at the most interesting step. Let's see if we can apply some of the concepts we have learned in the previous posts.

## III. The Domino Problem is Undecidable

We have already established that all possible methods for solving the domino problem are realizable as Turing Machines (TMs). We have also shown that all TMs $M$ can be placed into 1-1 correspondence with the positive integers $\mathbb{N}$, meaning they can be enumerated in a sequence:

$$M_1, M_2, M_3, M_4, ...$$

We also showed that all decision problems (i.e. problems requiring a yes or no answer) can be realized as functions of the form $f: \lbrace 0, 1 \rbrace^* \rightarrow \lbrace 0, 1 \rbrace$, which can be placed in 1-1 correspondence with the real numbers $\mathbb{R}$. Even though both of these sets are infinite in size, the size of $\mathbb{R}$ is a degree of infinity larger than $\mathbb{N}$, a fact that we proved using Cantor's diagonal argument. This means that if we were given a "random" decision problem, the probability that it is solvable by a TM is effectively $0$. Thus, it seems reasonable to assume any such problem is "guilty until proven innocent", that is, we should assume undecidability unless we can produce a TM that solves it. 
 
In the previous post, we gave an example of an undecidable problem, the _Halting Problem_:

> **Halting Problem:** \
> Given a Turing Machine $M$ and input $w$, will $M$ eventually reach the Stop state when run on on input $w$?

If we want to show that the domino problem is undecidable, perhaps the first thing we should do is attempt to find similarities between the domino problem and the halting problem. One immediate similarity is that both problems can be interpreted as determining whether a particular process stops or continues forever. In the case of the Halting Problem, we need to determine whether or not a TM will eventually stop on a given input. In the case of the Domino problem, we need to determine if the process of enumerating and checking all possible domino sequences will either eventually stop and produce a balanced sequence or continue forever.

We observe that if the Halting problem were decidable (meaning there is a TM $M_H$ that decides it), then the Domino problem must be also. Consider a TM $M$ that enumerates and checks all sequences of dominoes from a set $\mathcal{D}$ encoded as an input string $w$. We program $M$ such that it enters the Stop state when a balanced sequence is found. If a TM $M_H$ exists that decides the Halting problem, we can decide the domino problem by querying $M_H$ if $M$ halts on the input $w$ that encodes an arbitrary domino set $\mathcal{D}$.

This suggests asking if the converse holds for decidability: _What if the domino problem were decidable? Would that imply the halting problem is decidable?_ We claim that the answer to this question is in fact _yes,_ but proving it is a non-trivial task. If we _can_ prove it, we have effectively shown that the domino problem is undecidable, since if any TM $M_N$ in the enumeration $M_1, M_2, M_3, ...$ decides the domino problem, then there must exist some TM $M_H$ we can construct that decides the halting problem. This is a contradiction, so $M_N$ is in fact not contained in the enumeration, meaning the problem is undecidable. This  proof method showing that the decidability of one problem implies the decidability of another is called _reduction_. If the decidability of problem $A$ implies the decidability of problem $B$, we say that $B$ _is reducible to_ $A$.

Our reduction argument will proceed as follows. First, we will show that for any given TM $M$ and input $w$, we will construct a set of dominoes $\mathcal{D}$ that has a balanced sequence if and only if the TM $M$ eventually halts on input $w$. We can accomplish this by encoding the state transition rules of the TM into $\mathcal{D}$ such that if a balanced sequence exist, the sequence of symbols read across the top and bottom of a balanced sequence can be interpreted a sequence of steps the TM will make until it halts. If $M$ eventually halts, this sequence of dominoes will terminate in a balanced sequence; otherwise no balanced sequence will exist. Previously, we defined the set of valid symbols as $\Sigma = \lbrace \clubsuit, \spadesuit, \diamondsuit, \heartsuit \rbrace$. For convenience in annotating our dominoes, we shall relabel these symbols using the larger alphabet $\Sigma' = \lbrace 0, 1, \mid, {\$}, \ast , \square\rbrace$. We are free to introduce any larger alphabet $\Sigma'$, since we can map each symbol in $\Sigma'$ to a sequence of one or more symbols in $\Sigma$ of length at most $\log_4(\Sigma')+1$. For example, we could use the mapping:

$$\begin{aligned}
0 &\mapsto \clubsuit\clubsuit \\
1 &\mapsto \clubsuit\spadesuit \\
| &\mapsto \clubsuit\diamondsuit \\
\$ &\mapsto \clubsuit\heartsuit \\
\ast &\mapsto \spadesuit\clubsuit \\
\square &\mapsto \spadesuit\diamondsuit
\end{aligned}$$

If we are given a TM $M$ and an input $w$, we assume canonically that the TM begins with its head positioned at the first cell of its tape (the tape is initialized with the $w$ string). $M$ performs its computation by printing 0 or 1 symbols and moving left or right. At any given timestep, we can encode a complete snapshot of the TM's internal state, head position, and tape into a sequence of symbols from $\Sigma'$. First, we use the "${\$}$" symbol to signify the beginning of the tape and proceed to add all of the 1 and 0 symbols to the left of the head. We then encode the position of the head and the internal state as a binary number enclosed by the "$\mid$" symbol. For example, to encode a head in the 5th state, $S_5$, we would use "${\mid}101{\mid}$". If there are $N$ states the TM can be in, we can encode the state's number in binary using at most $\log_2(N) + 1$ symbols. We then add the rest of the tape beneath and to the right of the TM's head up until the first blank ("_") symbol occurs on the tape. In the figure below, we give some examples of this encoding of a TM at various steps:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-tm-history.svg'>
</p>
</div>

Using these encodings, we will attempt to construct a domino set $\mathcal{D}$ such that any balanced sequence will correspond with a valid sequence of these TM shapshots delimited by the "${\$}$" character. Note that we have not yet used the "$\ast$" or "$\square$" symbols in $\Sigma'$- we will introduce these symbols later. 

To denote the transitions in state within a TM, we will use the following shorthand notation. For a transition from state $S_n$ to $S_{n'}$, we write:

$$S \xrightarrow[(s_{\text{w}},D)]{(s_{\text{r}})} S'$$

where $s_r$ is the symbol read by the TM head, $s_w$ is the written symbol, and $D$ is the direction of the head's movement (either $R$ or $L$).

As an example, consider a simple TM that begins with an empty input string $w$, prints three "1"s, and finally moves the head left one cell prior to stopping. This TM will transition between incremental states to keep track of how many 1s have been printed. The state transition sequence of this TM is:

$$S_{\text{start}} \equiv S_0 \xrightarrow[(1,R)]{(\_)} S_1 \xrightarrow[(1,R)]{(\_)} S_2 \xrightarrow[(1,L)]{(\_)} S_3 \equiv S_{\text{stop}}$$

The sequence of snapshots for these state transitions is:

$$ \$|0|\$1|1|\$11|10|\$11|11|\$1|100|11\$ $$

This string is a rough idea of what we would like to read across the top and bottom of a balanced sequence. Through a clever construction of a domino set, we claim that we can do this for an any TM run on any input. For a particular TM $M$ run on an input string $w$, we construct the set of dominoes $\mathcal{D}_{M,w}$ according to the following scheme:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-tm-D.svg'>
</p>
</div>

In the figure above, each state $S_n$ is replaced with the binary representation of its respective number $n$ (using the "1" and "0" symbols in $\Sigma'$). The string $w$ is the TM's input string, which is also written in binary. There is a lot to unpack in this construction, so we will summarize what each of the color-coded dominoes in $\mathcal{D}_M$ do:

* The green domino is the starting domino that begins every balanced sequence corresponding to a valid series of TM computational steps. It encodes the head positioned at cell 0 and the input string $w$ initialized on $M$'s tape.

* The yellow dominoes allow for the copying of the non-blank tape content that remains the same from one state snapshot into the next. If a sequence begins with the green starting domino, then these ensure that none of the tape's contents are "erased" from one state snapshot to the next.

* The purple dominoes encode all of $M$'s allowed transitions in state (capturing both the read symbol, the written symbol, and the change in head position)

Upon closer inspection, we see that any sequence of yellow dominoes in $\mathcal{D}_{M,w}$ is balanced, which is not ideal. For now, we will address this problem by requiring every sequence of dominoes in $\mathcal{D}$ to begin with the green domino. (Later we will see that the "${*}$" and "${\square}$" symbols in $\Sigma'$ can be used to "force" it to be used first). Since the green domino contains the initial state of the TM head along with the tape input string, the next domino in any balanced sequence must be the first purple domino corresponding to the $S_{start} \rightarrow S^{(1)}$ transition, where we let $S^{(n)}$ denote the state of $M$ on the $n$th computational step when run on input input $w$. We also see that this purple domino must be followed by a sequence of yellow dominoes to "balance out" the remainder of the content of the input string $w$ on the bottom of the initial green domino. We see that this process continues, where each blue domino (sandwiched between yellow dominoes) represents a complete transition of $M$ from one state to the next, ensuring that the sequence of symbols read across the top is precisely the sequence of TM shapshots up until the final state is reached. The remaining dominoes serve the following purpose:

* The blue dominoes are necessary to avoid the problem of leftover symbols on the TM's tape. Once a final purple domino is added (i.e.  one that represents the TM transitioning into the Stop state), the string on the top of the domino sequence will be balanced with the string across the bottom up until the snapshot of the TM's final state, which is not included in the top. That is, the bottom is longer than the top by one snapshot. We remark that this disparity was first introduced by the green domino. To remedy this, we must include the blue dominoes, which are used to slowly erase the symbols on the tape to the left and right of the head until the top and bottom are balanced out. We achieve this by appending additional TM snapshots, where each snapshot erases one tape symbol to the right or left of the head. The remaining symbols on the tape that are not erased are copied between snapshots using the yellow dominoes. We call this process "tape contraction", and it solves the problem of the bottom string being longer by allowing the top to eventually "catch up" with the bottom.

* Finally, the red domino is needed to balance out the last fully-contracted snapshot. It ends with an empty snapshot (i.e. "${\$}{\$}$").

To give an example, consider the TM that prints three "1"s. Once the TM reaches the Stop state, its last state snapshot is "${\$} 1{\mid}100{\mid}11{\$}$". One possible tape contraction sequence is:

$$ \$1|100|11\$ 1|100|1\$ |100|1\$ |100|\$\$ $$

Therefore, the complete sequence that we want to read across the top and bottom of the dominoes encoding this computation is:

$$ \$|0|\$1|1|\$11|10|\$11|11|\$1|100|11\$ 1|100|1\$ |100|1\$ |100|\$\$ $$

In the general case, a complete balanced sequence looks like the following sketch:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-tm-D-sequence.svg'>
</p>
</div>

We observe that a balanced sequence in $\mathcal{D}_{M,w}$ beginning with the green domino exists if and only if $M$ eventually enters the stop state when run on input $w$. It is worth remarking that the balanced sequence isn't necessarily unique, since the order in which we use the blue dominoes to contract the symbols left on the TM's tape may not be unique.

There is one last wrinkle in our proof that we must iron out. We have assumed so far that every sequence _must_ begin with the green domino. In the original problem statement, we did not impose any such restrictions. We can resolve this by using the symbols "$\ast$" and "$\square$" in $\Sigma'$ to "force" the green domino to be used first. First, we introduce the string operators $\phi_L, \phi_R, \phi_{LR}$, which act on a strings comprised of symbols in $\Sigma'$. These operators interlace the "$\ast$" character between each character. In particular, we define:

$$\begin{aligned}
\phi_L(c_1c_2c_3...c_n) &= {\ast}c_1{\ast}c_2{\ast}c_3{\ast} ... {\ast}c_n \\
\phi_R(c_1c_2c_3...c_n) &= ~~c_1{\ast}c_2{\ast}c_3{\ast} ... {\ast}c_n{\ast} \\
\phi_{LR}(c_1c_2c_3...c_n) &= {\ast}c_1{\ast}c_2{\ast}c_3{\ast} ... {\ast}c_n{\ast} \\
\end{aligned}$$

where each $c_i$ is a symbol in $\Sigma'$. Next, we augment ${\mathcal{D}_{M,w}}$ to produce the domino set ${\tilde{\mathcal{D}}_{M,w}}$:

<div id="html" markdown="0">
<p align="center">
<img src='/images/posts/undecidability-tm-D2.svg'>
</p>
</div>

Since the green domino in ${ {\tilde{\mathcal{D}}}_{M,w} }$ is the only one that begins with "${\ast}$" on the top and bottom, it must come first in a balanced sequence. Furthermore, we see that any balanced sequence of dominoes in $\mathcal{D}_{M,w}$ that begins with the green domino corresponds uniquely to a balanced sequence of the modified dominoes, provided that we place the special orange domino in ${\tilde{\mathcal{D}}_{M,w}}$ at the end of this modified sequence. This orange domino is necessary to balance the extraneous "${\ast}$" symbol that appears at the end of the bottom of the modified sequence.

This proves that $\tilde{\mathcal{D}}_{M,w}$ has a balanced sequence if and only if the Turing Machine $M$ eventually halts on the input $w$, that is, the domino problem is reducible to the halting problem. However, we established previously that the halting problem is undecidable, which implies that the domino problem must be as well.

## Concluding Remarks


The proof presented here for the undecidability of the domino problem closely follows the proof given in Sipser's textbook _An Introduction to the Theory of Computation_ [2]. This textbook is quite standard in most computer science classrooms, and a recommended read for those new to computer science theory. 

In the literature, the domino problem is commonly referred to as the _Post Correspondence Problem_, first introduced by the mathematician Emil Post in 1946 [1]. You can read Post's original paper [here](https://cburdine.github.io/files/Post_Correspondence_Paper_1946.pdf).

If you would like to read more about Turing Machines and the Halting Problem [3], you can read Turing's original paper [here](https://cburdine.github.io/files/Turing_Paper_1936.pdf).



## References
[1] Post, Emil L. “A Variant of a Recursively Unsolvable Problem.” 	  Bulletin of the American Mathematical Society 52, no. 4 (1946): 264–68. https://doi.org/10.1090/S0002-9904-1946-08555-9.

[2] Sipser, Michael. Introduction to the Theory of Computation. 3rd edition. Boston, MA: Cengage Learning, 2012.

[3] Turing, A. “On Computable Numbers, with an Application to the Entscheidungsproblem,” 1937. https://doi.org/10.1112/PLMS/S2-42.1.230.

