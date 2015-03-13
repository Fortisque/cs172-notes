---
layout: post-no-feature
title: "Lecture 15"
description: "Intro to Complexity Classes"
category: articles
mathjax: true

---

So far in the class, we've seen DFAs and streaming algorithms - models
that are extremly accurate and well-understood, but are only applicable
to limited domains. We've also covered decidability by arbitrary algorithms,
with no limits on their space or memory usage, which is a good (and
well-understood) model for proving that things are impossible. Unfortunately,
it's unable to figure out if a problem is actually feasible is real-life
- there is a huge gap between a program that runs in constant time
and one that won't finish running until after the heat death of the
universe!

In this lecture - the beginning the last and longest part of the course
- we'll cover the complexity, an approach that allows us to address
the question "How efficiently can algorithms be solved?" Although
less well understod than the other two models, it allows us to reason
about how programs behave realistically.

We'll start with a few definitions.

**Definition:** A language $$L$$ is in $$\mathbf{DTIME}(t(\cdot))$$
if there exists a TM $$M$$ that decides $$L$$ and runs in time less
than or equal to $$t(n)$$ on inputs of length $$n$$. For instance, $$\mathbf{DTIME}(O(n^{2}))$$
represents the class of problems solvable in quadratic time. A language
is in $$\mathbf{P}$$ if it is in $$\mathbf{DTIME}(p(n))$$ for some polynomial
$$p$$.

We'll take $$\mathbf{P}$$ to be a rough model of efficiently solvable
problems.

There are many problems that we don't know whether or not they are
in $$\mathbf{P}$$. These include integer factorization, integer linear
programming, non-convex optimization, SAT, finding minimal neural
networks approximately matching labelled examples, TSP, some scheduling
programs, and breaking cryptosystems.

Notice, however, that all of these problems can be checked efficiently
- given a problem and a candidate solution, we can check in reasonable
time if the given candidate is a valid solution. We formalize this
notion with our definition of the class $$\mathbf{NP}$$:

**Definition:** An $$\mathbf{NP}$$ search problem is a relation
$$R\subseteq\Sigma^*\times\Sigma^*$$ computable in polynomial time
such that there is a polynomial $$l$$ such hat if $$(x,y)\in R$$, then
$$\vert y\vert<l(\vert x\vert)$$ - the latter clause requires that
the output required is polynomial in the length of the input, so we
don't spend forever just printing out the output. We say that an algorithm
$$A$$ solves an $$\mathbf{NP}$$ search problem $$R$$ if for every input
$$x$$, $$(x,A(x))\in R$$ given that there is some $$y$$ such that $$(x,y)\in R$$,
and say that a language $$L$$ is in the set $$\mathbf{NP}$$ if there
is an $$\mathbf{NP}$$ search problem $$R$$ such that $$L$$ is the set
of strings $$x$$ such that there is at least one $$y$$ such that $$(x,y)\in R$$.

Examples of languages in $$\mathbf{NP}$$ include SAT (boolean expressions
with an assignment to variables that makes the expression true), 3COL
(graphs that can be colored with three colors such that no two endpoints
of a vertex have the same color?), and CONNECTED (connected graphs).

Most problems that we are interested in can be expressed as $$\mathbf{NP}$$
search programs, or are closely related. For optimization problems,
although the problem itself is not an $$\mathbf{NP}$$ search program
(since checking optimality is hard), we can ask for a solution whose
score is greater than some threshold (which is easily checkable),
and then use binary search to find the optimal solution.

The classic $$\mathbf{P}\stackrel{?}{=}\mathbf{NP}$$ problem asks:
is every $$\mathbf{NP}$$ search problem solvable in polynomial time?
If the answer is yes, then cryptography is impossible, math can be
automated and any optimization problem is efficiently solvable; generally,
anything that can be specified can be constructed. In particular,
we have the following result:

**Theorem:** Every $$\mathbf{NP}$$ search problem is solvable
in polynomial time if and only if $$\mathbf{P}=\textbf{NP}$$.

The "only if" direction is a trivial result of the definition
above.

Conversely, suppose $$\mathbf{P}=\mathbf{NP}$$. Let $$R$$ define an
$$\mathbf{NP}$$ search problem. Define $$R'$$ as follows: $$((x,y),z)\in R'\iff(x,yz)\in R$$
- intuitively, $$R'$$ is the set of problems that asks for whether
$$x$$ is solvable in $$R$$ under some extra constraints $$z$$.

Let $$L'=\{(x,y):\exists z\mbox{ s.t. }(x,yz)\in R\}$$; this is an
$$\mathbf{NP}$$ language decidable in polynomial time; by our assumption
that $$\mathbf{P}=\mathbf{NP}$$, this is decidable in polynomial time
by a machine we'll call $$A'$$.

Define this algorithm for $$R$$: given an input $$x$$, if $$A'(x,\epsilon)$$
(where $$\epsilon$$ is the empty string) rejects, then output "no
solution". Now, initialize $$y=\epsilon$$. 

Now, perform a loop while $$(x,y)\notin R$$: if $$A'(x,y0)$$ accepts
then update $$y$$ to be $$y0$$; else, update $$y$$ to be $$y1$$. Intuitively,
we keep $$y$$ as a prefix of a solution, and we are always adding either
$$0$$ or $$1$$ to to construct the solution bit by bit.

Once the loop is finished, output $$y$$.

Notice that the number of iterations is at most $$l(\vert x\vert)$$,
since the output is at most $$l(\vert x\vert)$$ long (we get one character
closer to the solution at each iteration) and each one iteration takes
polynomial time.

Therfore, we've solved the $$\mathbf{NP}$$ search problem in polynomial
time (assuming that $$\mathbf{P}=\mathbf{NP}$$).

On the other hand, if $$\mathbf{P}\neq\mathbf{NP}$$, we'd be able to
prove that thousands of problems are not actually feasibly solvable
in a reasonable amount of time. 

Define a polynomial-time reduction (sometimes called a polynomial-time
Karp reduction) as follows: if $$A$$ and $$B$$ are language, then $$A\leq_{m}^{p}B$$
if there is a polynomial-time computable function $$f$$ such that for
all $$x\in\Sigma^*$$, $$x\in A\iff f(x)\in B$$.

An obvious result is that if $$A\leq_{m}^{p}B$$ and $$B\in\mathbf{P}$$,
then $$A\in\mathbf{P}$$, and if $$A\leq_{m}^{p}B$$ and $$A\notin\mathbf{P}$$,
then $$B\notin\mathbf{P}$$.

**Definition:** A problem $$A$$ is $$\mathbf{NP}$$-hard if for
every language $$L\in\mathbf{NP}$$, $$L\leq_{m}^{p}A$$. If $$A$$ is also
in $$\mathbf{NP}$$, then we refer to $$A$$ as $$\mathbf{NP}$$-complete.

If $$\mathbf{P}\neq\mathbf{NP}$$ and $$A$$ is $$\mathbf{NP}$$-hard, then
$$A\notin P$$; thousands of problems have been shown to be $$\mathbf{NP}$$-hard.
On the other hand, given *any* $$\mathbf{NP}$$-complete problem
$$A$$, then $$A\in P$$ if and only if $$\mathbf{P}=\mathbf{NP}$$.

###Further reading

Please suggest interesting and related material on Piazza if you have any!

Arora and Barak's treatment of of the material can be found in chapter 2 of [their book on complexity](http://theory.cs.princeton.edu/complexity/book.pdf) and includes an interesting section (2.7.3) on the implications of $$\mathbf{P}=\mathbf{NP}$$.
