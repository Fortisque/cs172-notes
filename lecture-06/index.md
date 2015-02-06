---
layout: post-no-feature
title: "Lecture 6"
description: "Minimal DFAs"
category: articles
mathjax: true

---

We'll continue our discussion from the last lecture of minimizing the number of states of a DFA, and present an algorithm to do so in time polynomial to the number of sates of the automaton.

Our approach will be as follows: given an automaton $$M$$, define an equivalence relation $$\approx_{M}$$ among states, and then merge equivalent states. Our equivalence relation is defined as follows: two states $$q_{1}$$ and $$q_{2}$$ are equivalent if for any string $$t$$, $$M$$ will accept if fed $$t$$ when in state $$q_{2}$$ if and only if $$M$$ will accept if it is fed $$t$$ when it is in state $$q_{2}$$; that is:

$$q_{1}\approx_{M}q_{2}\iff\forall t\in\Sigma^{*}:\delta^{*}(q_{1},t)\in F\iff\delta^{*}(q_{2},t)$$ 

Unfortunately, we can't guarantee that we can compute this in finite time. Therefore, we define the following equivalence relation:

**Definition:** For $$n\in\mathbb{Z}$$, we write $$q_{1}\approx_{M}^{n}q_{2}$$ if for all $$t$$ in $$\Sigma^{*}$$ with length at most $$n$$, $$\delta^{*}(q,t)\in F\iff\delta_{2}(q_{2},t)\in F$$.

It is obvious that $$q_{1}\approx_{M}q_{2}$$ if and only if for all nonnegative $$n$$, $$q_{1}\approx_{M}^{n}q_{2}$$. We will see that we can actually test this using only a finite number of values of $$n$$.

Our starting point, $$\approx_{M}^{0}$$ is a partition of $$Q$$ into $$Q\backslash F$$ and $$F$$.

It is simple to construct $$\approx_{M}^{n+1}$$ given $$\approx_{M}^{n}$$: we know that given two states $$q_{1}$$ and $$q_{2}$$, $$q_{1}\not\approx_{M}^{n+1}q_{2}$$ if and only if one of these two hold:

- there is a string $$t$$ of length $$n+1$$ such of $$\delta^{*}(q_{1},t)$$ and $$\delta^{*}(q_{2},t)$$, one is in $$F$$ and the other is not.

- \\(q\_{1}\not\approx\_{M}^{n}q\_{2}\\)

If we're doing this recursively, we know the second; only the first is of consequence. The first is simple to check: we simply check if $$\delta(q_{1},a)\approx_{M}^{n}\delta(q_{2},a)$$ far all $$a\in\Sigma^{*}$$; if this does not hold for some $$a$$, then the first condition holds implying that $$q_{1}\not\approx_{M}^{n+1}q_{2}$$.

Since the quivalence relation $$\approx_{M}^{n}$$ only depends on $$\approx_{M}^{n-1}$$, if $$\approx_{M}^{n}$$ is the same as $$\approx_{M}^{n-1}$$, then it is also the same as $$\approx_{M}^{n+k}$$ for all $$k\ge0$$. Also, note that $$q_{1}\approx_{M}^{n}q_{2}$$ implies that $$q_{1}\approx_{M}^{n-1}q_{2}$$, which means that the number of equivalence classes with respect to $$\approx_{M}^{n}$$ is monotonically non-decreasing; as a result, our fixed-point iteration is guranteed to converge (to $$\approx_{M}$$).

This proceduser allows us to create the smallest possible DFA equivalent to another DFA we feed in. Note that there is no known equivalent process for generating minimal NFAs; in fact, this is an NP-hard problem.

In general, once we compute $$\approx_{M}$$ for $$M=(Q,\Sigma,\delta,q_{0},F)$$, define $$M'=(Q',\Sigma,\delta',q_{0}',F')$$ as follows:

- $$Q'$$ contains one state for each equivalence class of $$Q$$ according to $$\approx_{M}$$.
- $$\delta'([q],a)=[\delta(q,a)]$$; that is, given an equivalence class of states containing $$q$$ and a character $$a$$, we go to the equivalence class of $$\delta(q,a)$$ .
- \\(q\_{0}'=[q\_{0}]\\) 
- \\(F'=\{[q]:q\in F\}\\)
 
We now wish to argue that $$M'$$ is deterministic, correct, and minimal.

####Deterministic:

Suppose we $$A\rightarrow^{a}B$$ and $$C\rightarrow^{a}D$$, and that $$A\approx_{M}C$$ but $$B\not\approx_{M}D$$. This would create an NFA; we wish to argue that this never happens. Proof: Since $$B\not\approx_{M}D$$, there exists some string $$t$$ such that if the current state is $$B$$ and we give it $$t$$, we will get a different result compared to what we would have gotten if we had given it $$t$$ when it was at $$B$$. But then the string $$at$$ would distinguish $$A$$ and $$C$$, which contradicts $$A\approx_{M}C$$, as desired.

####Correctness:

We can prove, by induction on the length of $$s$$, that for every $$s$$, $$\delta'^{*}([q_{0}],s)=[\delta^{*}(q_{0},s)]$$.

####Minimality:

We wish to show that after removing unreachable states, $$M'$$ will be minimal: let $$c_{0}...c_{k}$$ be the new states, and let $$s_{0}...s_{k}$$ be string such that $$\delta^{*}(c_{0},s_{i})=c_{i}$$. It suffices to show that the $$s_{i}$$ are distinguishable. For $$i\neq j$$, $$c_{i}$$ and $$c_{j}$$ are distinct equivalence classes: there exists some string $$t$$ such that $$M$$, if fed $$t$$ when at $$c_{i}$$, will have a different result than when $$M$$ is fed $$t$$ when at $$c_{j}$$. This means that the $$s_{i}$$ and $$s_{j}$$ must be distinguishable, meaning that the DFA is minimal.

---

Note that any optimal DFA is unique up to the labels of the states.
