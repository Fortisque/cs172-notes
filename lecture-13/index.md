---
layout: post-no-feature
title: "Lecture 13"
description: "Gödel's Incompleteness Theorems"
category: articles
mathjax: true

---
We define a *formalization* of mathematics to consist of the following:

- A set of statements $$S$$

- a set of proofs $$P$$

- a definition of when $$P$$ is a valid proof of $$S$$

We make these assumptions:

1. For every TM $$M$$ and string $$x$$, there exists a statement $$S_{M,x}$$
that represents "$$M$$ halts on $$x$$", and a statement $$\neg S_{M,x}$$
saying "M doesn't halt on $$x$$".

2. If $$M$$ halts on input $$x$$, then $$S_{M,x}$$ is provable.

3. If $$M$$ on input $$x$$ repeats a configuration, then $$\neg S_{M,x}$$
is provable.

We define a a formal system as *consistent* if there exists no
statement $$S$$ such that both $$S$$ and $$\neg S$$ is provable, and
*complete* if there is a proof of either $$S$$ or $$\neg S$$ for
all $$S$$.

###The results

The first incompleteness theorem states that a consistent formal system
that satisfies the assumptions above must be incomplete.

The second incompleteness system states that the consistency of a
consistent formal system that satisfies the assumptions cannot be
proved within the system.

Turing proved that in any formal system satisfying the assumptions
above, the provability of a statement is undecidable.

###Proof of the first theorem

Suppose for contradiction that we have a consistent, complete system.

We will construct an algorithm for the halting problem. Given input
$$M$$, $$x$$, construct $$S_{M,x}$$. Now, for each string $$P$$ in lexicographical
order, accept if $$P$$ proves $$S_{M,x}$$ and reject if $$P$$ proves
$$\neg S_{M,x}$$.

Since we assume *completeness*, we know that this will terminate.
But why is it pathological? 

Let's think back to the proof that the halting problem is undecidable.
Define a machine $$M_{D}$$ that takes input $$M$$ and proceeds as follows:
if $$M_{H}(\langle M\rangle,\langle M\rangle)$$ accepts, then loop
infinitely; otherwise, halt. Running $$M_{D}$$ with $$\langle M_{D}\rangle$$
as input results in pathological behavior.

Now we can make a slight modification to the previous algorithm (call
this machine $$M_{G}$$): Given input $$M$$, construct $$S_{M,\langle M\rangle}$$.
Now, for each string $$P$$ in lexicographical order, loop infinitely
if $$P$$ proves $$S_{M,\langle M\rangle}$$ and halt if $$P$$ proves $$\neg S_{M,\langle M\rangle}$$.

Now what happens if we run $$M_{G}$$ with input $$\langle M_{G}\rangle$$?
We have three cases:

1. We get infinitely many iterations of the for loop because there
is no proof $$P$$ for either $$S_{M_{G},\langle M_{G}\rangle}$$ or $$\neg S_{M_{G},\langle M_{G}\rangle}$$,
which means that the system is incomplete.

2. We find a proof $$P$$ for $$S_{M_{G},\langle M_{G}\rangle}$$ and
we then loop infinitely, staying in the same configuration for eternity.
But this means that the fact that the machine does not halt is provable,
which means that there is a proof for $$\neg S_{M_{G},\langle M_{G}\rangle}$$,
But $$P$$ is a proof for $$S_{M_{G},\langle M_{G}\rangle}$$, which means
the system is inconsistent.

3. We find a proof $$P'$$ for $$\neg S_{M_{G},\langle M_{G}\rangle}$$,
and we halt. But that means that $$S_{M_{G},\langle M_{G}\rangle}$$
is provable, since we halt, which, coupled with $$P'$$, shows that
our system is inconsistent.

###Proof of the second theorem

Notice that $$M_{G}$$ from the above proof is designed in such a way
that the consistency of the system implies that $$S_{M_{G},\langle M_{G}\rangle}$$
will not halt. Notice that this implication ($$\mbox{consistency}\implies\neg S_{M_{G},\langle M_{G}\rangle}$$)
can be stated and proved within our system by assumption (3).

But if the system is consistent, then, as shown above, $$\neg S_{M_{G},\langle M_{G}\rangle}$$
is not provable.

Therefore, if the system is consistent, then the fact that a statement
is consistent cannot be proved in the formal system.

###Proof of Turing's result

Suppose for contradiction that there exists $$M_{P}$$ that, given a
statement $$S$$, decides if $$S$$ or $$\neg S$$ is provable.

Construct a machine $$M_{T}$$ that takes input $$M$$ and proceeds as
follows: first, construct $$S_{M,\langle M\rangle}$$. If $$M_{P}(S_{\langle M\rangle,\langle M\rangle})$$
rejects, then halt. Otherwise, for each string $$P$$, loop infinitely
if $$P$$ proves $$S_{M,\langle M\rangle}$$, and halt if $$P$$ proves
$$\neg S_{M,\langle M\rangle}$$.

Suppose the system is consistent. Then we have three cases: 

1. Neither $$S_{M_{T},\langle M_{T}\rangle}$$ nor $$\neg S_{M_{T},\langle M_{T}\rangle}$$
is provable. Then $$M_{T}(\langle M_{T}\rangle)$$ halts, so $$S_{M_{T},\langle M_{T}\rangle}$$
is provable, which is a contradiction with consistency.

2. $$S_{M_{T},\langle M_{T}\rangle}$$ is provable. Then we reach an
infinite loop, and $$\neg S_{M_{T},\langle M_{T}\rangle}$$ is provable,
which provides a contradiction with consistency.

3. $$\neg S_{M_{T},\langle M_{T}\rangle}$$ is provable. Then $$M_{T}(\langle M_{T}\rangle)$$
halts, which means that $$S_{M_{T},\langle M_{T}\rangle}$$ is provable,
which again contradicts consistency.

####An alternative proof of Turing's result

Suppose $$M_{P}'$$ that decides if $$S$$ is provable. Define a machine
$$M_{T}'$$ with input $$M$$ as follows: construct $$S_{M,\langle M\rangle}$$.
If $$M_{P}'(S_{M,\langle M\rangle})$$ rejects, then halt; otherwise,
loop infinitely.

If $$S_{M_{T},\langle M_{T}\rangle}$$ is provable, then $$M_{P}'(S_{M_{T},\langle M_{T}\rangle})$$
accepts so we loop infinitely; this means that $$\neg S_{M_{T},\langle M_{T}\rangle}$$
is also provable, which contradicts consistency.

On the other hand, if $$S_{M_{T},\langle M_{T}\rangle}$$ is unprovable,
then we halt since $$M_{P}'(S_{M_{T},\langle M_{T}\rangle})$$ rejects,
which means that $$S_{M_{T},\langle M_{T}\rangle}$$ is provable, again
contradicting consistency.

###Aside: the continuum hypothesis

Given two sets $$A$$, $$B$$, we say that $$\vert A\vert=\vert B\vert$$
if and only if there is a bijection from $$A$$ to $$B$$, and we say
that $$\vert A\vert\leq\vert B\vert$$ if and only if there is an injection
from $$A$$ to $$B$$.

Cantor showed that $$\vert\mathbb{N}\vert<\vert\mathbb{R}\vert$$: that
is, $$\vert\mathbb{N}\vert\neq\vert\mathbb{R}\vert$$ and $$\vert\mathbb{N}\vert\leq\vert\mathbb{R}\vert$$.

The question asking whether there was some intermediate set $$\mathbb{S}\subset\mathbb{R}$$
such that $$\vert\mathbb{N}\vert<\vert\mathbb{S}\vert<\vert\mathbb{R}\vert$$
was an long-standing open problem. It turned out to that neither this
statement nor its converse were provable in the standard formalization
of mathematics (ZFC).
