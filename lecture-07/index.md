---
layout: post-no-feature
title: "Lecture 7"
description: "Streaming Algorithms"
category: articles
mathjax: true

---

Today, we discuss *streaming algorithms*, which make one pass through input and use very limited memory. We'll treat our input is a sequence of $$n$$ elements of a set $$\Sigma$$, such that $$\vert \Sigma \vert=2^{l}$$.

Consider these two problems:

**Problem 1:** Suppose we are given a sequence of characters in which there exists a 'majority element' that comprises a majority of the characters. We want to find the most frequent element of our input. Formally, suppose we have $$s_{i},...,s_{n}$$, is defined such that some input character (the majority element) appears at least $$\frac{n}{2}+1$$ times. We wish to find it with $$\log n+l$$ bits of memory.

Here's how we do it (solution by Anonymous on Piazza): Consider a "tournament" among the $$n$$ symbols. Keep track of a "champion" with an integer number of health points (start with the first symbol as the champion, with $$1$$ health point). Each symbol streams in and lines up to fight the current champion. If the next symbol is the same as the champion, she gets one more health point. Otherwise, the two symbols fight, and both lose $$1$$ health point (if both the champion and the challenger are killed, then the next symbol becomes the champion with $$1$$ health point). Observe that the majority element will in total have more health points than all the other symbols combined. Even if they all attack her, she will still survive to the end with at least $$1$$ health point, and if they waste their time wearing each other down, all the better. Also, at any point in time, you only need to keep track of the current champion ($$l$$ bits) and their hit points ($$\log n$$ bits).

We will see later today that a harder version of problem 1 - finding a most frequent element *without* the assumption that a majority element exists - requires $$\Omega(nl)$$ memory if $$2^{l}>n^{2}$$. In fact, we will prove any algorithm that checks, given $$n$$ inputs, if $$0$$ is the unique most frequent element (we'll call this **Problem 2**), needs at least $$\Omega (ln)$$ bits of memory.

###A lower bound

Define the language $$L$$ to be the set of all sequences of length $$n'\le n$$ in which $$0$$ is the most frequent element. We make the following two claims:

- If there is an algorithm for problem 2 that uses $$m$$ bits of memory, then there exists a DFA for $$L$$ with $$2^{m+\log n}$$ states.

- $$L$$ has at least $$2^{\Omega(nl)}$$ distinguishable strings.

These two imply that $$2^{m+\log n}>2^{\Omega(nl)}$$, implying that $$m>\Omega(nl)-\log n$$ (or, since we're talking asymptotics, $$m>\Omega(nl)$$).

Let $$m$$ be the amount of memory in bits used by an algorithm for problem 2 on inputs of length at most $$n$$.

Add a counter, bringing total memory use to $$m+\log n$$ bits so we can solve problem 2 on inputs of length at most $$n$$ and reject on inputs of length $$>n$$.

Construct a single DFA state for each possible content of the memory of the algorithm. The start state of the DFA corresponds to the state of the algorithm at the start.

Let's look at strings constructed as follows: start with `000`, and then $$\frac{n-5}{2}$$ pairs of nonzero numbers each (for instance: `000113355` and `000112233`). Notice that any two strings with different choices of the $$\frac{n-5}{2}$$ character pairs are distinguishable; for instance, for the two strings given earlier, appending `22` to both strings will cause the first to reject and the second to accept. That means the number of distinguishable strings is $$\binom{2^{l}}{(n-5)/2}$$. We'll use the inequality: $$\binom{n}{k}\ge(\frac{n}{ek})^{k}$$ to get:

$$\binom{2^{l}}{(n-5)/2}>(\frac{2^{l}}{\frac{e(n-5)}{2}})^{\frac{n-5}{2}}$$

Since we made the assumption that $$2^{l}>n^{2}$$ we know that $$\frac{2^{l}}{O(n)}>\frac{2^{l}}{O(\sqrt{2^{l}})}\ge\frac{2^{l/2}}{O(1)}$$. That means that our inequality reduces to:

$$\binom{2^{l}}{(n-5)/2}>2^{\Omega(ln)}$$

which gives us a lower bound on the number of distinguishable states, and therefore, the amount of memory we need. $$\blacksquare$$ 

Notice that it sometimes is the case where accessing the memory in a different order gives us a lower space bound. For instance, if we are given a string and we want to know if it's a palindrome, we'd need to store $$O(n)$$ memory to do it if we were going from left to right, but we could do it in $$O(\log n)$$ memory (to store our current location in the string) if we are allowed to access it in the order $$1,n,n-1,2,3,n-2...$$.

###Distinguishability revisited

**Problem 3:** Given $$s_{1},...,s_{n'}$$, for $$n'\le n$$ such that $$s_{i}\in\Sigma$$ where $$\vert \Sigma \vert=2^{l}$$, we want to compute the number of distinct elements in the input.

We define two sequences $$x_{1}...x_{n_{1}}$$ and $$y_{1}...y_{n_{2}}$$ to be *distinguishable* for a problem for input length $$n$$ if there exists a string $$z_{1}...z_{n_{3}}$$ such that the correct answer for $$x_{1}...x_{n_{1}}z_{1}...z_{n_{3}}$$ is different from the correct answer to $$y_{1}...y_{n_{2}}z_{1}...z_{n_{3}}$$ and $$n_{1}+n_{3},n_{2}+n_{3}\le n$$. This is basically the same thing as distinguishable strings, except we're talking about more general algorithms (instead of just automata) where the answer might be more interesting than a single bit (e.g. an integer).

**Fact:** if a problem admits at least $$2^{m(n)}$$ distinguishable sequences for length $$n$$, then every streaming algorithm for the problem needs at least $$m(n)$$ bits of memory on inputs of length $$n$$. We can see why this is the case using a counting argument similar to the one we used DFAs: having fewer than $$m(n)$$ bits of memory means we'd have less than $$2^{m(n)}$$ memory states, which means that there would be at least two distinguishable states we could feed in that would map to the same memory state; this would mean that any input fed into our algorithm thereafter would lead to the same result, violating the assumption that the states are distinguishable.

Considering the distinct elements problem, consider two strings of length $$n-1$$ comprised of distinct elements. If the character $$a$$ is in one but not the other, then appending $$a$$ to both will allow us to distinguish them (since one will have $$n-1$$ distinct elements and the other has $$n$$ distinct elements). That means that the number of distinguishable strings is bounded below by $$\binom{2^{l}}{n-1}>(\frac{2^{l}}{e(n-1)})^{n-1}$$, so, using the same assumption $$(2^{l}>n^{2}$$) and the same methodology as before, we get $$m\ge2^{\Omega(ln)}$$.

###Approximation approaches

What if we're willing to put up with errors? In this case, we can redefined distinguishability as follows: each input produces a range of possible answers; now we state that the two strings are distinguishable if appending any other string to them (assuming total string length is still at most $$n$$) will result in two sets of possible answers with a null intersection. The previous fact still holds.

Now suppose I have two strings $$x=x_{1}...x_{n/2}$$ and $$y=y_{1}...y_{n/2}$$, each a sequence of distinct elements, and there are at least $$\frac{n}{10}$$ elements of $$x$$ not in $$y$$, and there are at least $$n/10$$ elements of $$x$$ that are not in $$y$$. Then $$xx$$ has $$n/2$$ distinct elements and $$yx$$ has at least $$0.6n$$ distinct elements. It can be proven (we won't do it here) that this still results in least $$2^{\Omega(nl)}$$ distinguishable sequence, which means that we still need $$\Omega(nl)$$ memory even if we allow for a small amount of error.

What if we also allow randomization in addition to error? Recall the hash-based algorithm from the beginning of class: pick a random hash function $$h:\Sigma\rightarrow[0,1]$$, compute $$\delta=\min\{h(s_{1}),...,h(s_{n})\}$$, and output $$1/\delta$$.

Now we want to prove that this algorithm brings us to within a constant factor of the true value.

Suppose the string $$s_{1}...s_{n}$$ has $$k$$ distinct elements $$a_{1}...a_{k}$$. Let $$\delta=\min\{h(a_{1}),...,h(a_{n})\}$$. Since the hash function is random, we can write this as $$\min\{r_{1},...,r_{k}\}$$ where each $$r_{i}$$ is a uniformly distributed random number between $$0$$ and $$1$$ inclusive. Therefore, the probability of overestimating is:

$$
\begin{eqnarray*}
P[\frac{1}{\delta}>10k] & = & P[\delta<\frac{1}{10k}]\\
 & = & P[\exists i\in\{1..k\}|r_{i}<\frac{1}{10k}\}\\
 & \le & \sum_{i=1}^{k}P[r_{i}<\frac{1}{10k}]\\
 & \le & \frac{k}{10k}\\
 & = & 0.1
\end{eqnarray*}
$$ 

The probability of underestimating is:

$$
\begin{eqnarray*}
P[\frac{1}{\delta}<\frac{k}{10}] & = & P[\delta>\frac{10}{k}]\\
 & = & P[\forall i:r_{i}>\frac{10}{k}]\\
 & = & P[r>\frac{10}{k}]^{k}\\
 & = & (1-\frac{10}{k})^{k}\\
 & \approx & e^{-10}
\end{eqnarray*}
$$

*Thanks to Anonymous on Piazza for contributing the problem 1 solution and various enhancements/fixes.*

*Thanks to Manuel Sabin and Saam Barati for pointing out typos.*
