---
layout: post-no-feature
title: "Lecture 1"
description: "Course Information"
category: articles
mathjax: true

---

The course website is located [here](http://www.cs.berkeley.edu/~luca/cs172/). Piazza is used for Q&A.

**Homework assignments** will be released on Thursdays and due the following Fridays; they are worth 45% of course grade.

The **final exam** is scheduled for 11 May and is worth 25% of the grade.

There are **midterms** scheduled for 26 February and 09 April, each worth 15% of course grade.

The general goal of the course is to mathematically address this question: *What is impossible for algorithms?* To do so, we rely on rigorous models of computation, computational problems, and data. The class can be roughly divided into three parts, corresponding to three computational models:

- **Finite automata** capture algorithms that use a constant number of bits of memory and only make one pass through the input; it's easy to prove lower bounds using this model but only applies to a restricted class of problems. This computational model is isomorphic to regular expressions. The Myhill-Nerode theorem specifies the complexity of this problem. A similar class of algorithms are *streaming algorithms*, which represent a similar class of problems, except without the restriction that the memory usage of the algorithm must be constant. Unfortunately, these algorithms don't exist for many problems; in some cases, however, some unexpectedly efficient solutions may arise. For instance: suppose we're fed a stream of IP addresses; at any time, we want to know how many distinct addresses you've seen; a simple way to do so is to keep a set of IP addresses previously seen and a counter, and to add new addresses to the set and increment the counter when they are encountered. It is possible to mathematically prove that $$k(\#\mbox{bits needed to store IP address})$$ is a lower bound for memory usage of an exact algorithm. On the other hand, if we're willing to put up with some randomness, we can use a hash function as follows: take a hash $$h:\mbox{IP}\rightarrow[0,1]$$ of all your IP addresses. Store the minimum hash we've seen so far; at any given time, the inverse of the minimum is an approximation of the number of distinct elements.

- **Turing machines** encapsulate all algorithms. We will prove the undecidability of certain problems, e.g. the halting problem. We will examine Rice's theorem, which states that all problems where we're given a certain program and an input and we want to know some property of the output are undecideable unless we are given space or computation time limits. We will examine Turing's proof of Godel's theoem and Kolmogorov complexity. At the end, we will show that we can pick a mathematical statement at random (in a certain precise sense) that with high probability, it is both true and unprovable.

- **Models encapsulating all efficient algorithms**, where running time polynomial in input size. We will examine hierarchy theorems, which allows you to prove that there are finite problems that are unsolvable in polynomial time, and that there are problems solvable in time $$O(n^{3})$$ but not in time $$O(N^{3}/\log n)$$ or $$O(n^{2})$$. We will explore the consequences of $$\mathbf{P}=\mathbf{NP}$$ and $$\mathbf{P}\neq\mathbf{NP}$$.

We will devote a few lectures at the end of the class covering application in security such as zero-knowledge proofs.
