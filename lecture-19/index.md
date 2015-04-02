---
layout: post-no-feature
title: "Lecture 19"
description: "NP-Complete Problems II: Bin Packing, Scheduling, Knapsack, and Steiner Tree"
category: articles
mathjax: true

---

So far, we have proven the NP-completeness of CKT-SAT, 3SAT, independent
set, clique, vertex cover, subset sum, and partition. Today we will
examine bin packing, scheduling, knapsack, Steiner tree, Hamiltonian
cycle, and TSP.

Our first two reductions today will be from the partition problem
described at the end of the previous lecture: given integers $$a_{1},a_{2},...,a_{n}$$,
is there some set $$S\subseteq\{1,...,n\}$$ such that $$\sum_{i\in S}a_{i}=\sum_{i\notin S}a_{i}=\frac{1}{2}\sum_{i=1}^{n}a_{i}$$?

###Knapsack

Given $$n$$ items, suppose item $$i$$ has cost $$c_{i}$$ and weight $$w_{i}$$.
We are given a weight bound $$B$$, and we wish to find some set $$S\in\{1,...,n\}$$
such that $$\sum_{i\in S}c_{i}$$ is maximized subject to $$\sum_{i\in S}w_{i}\leq B$$.

We can formulate this optimization problem as a decision function:
given the same items and the same weight bound, we take an additional
parameter $$t$$ and ask whether or not there exists $$S\in\{1,...,n\}$$
such that $$\sum_{i\in S}c_{i}\geq t$$.

The reduction from partition is quite simple: suppose we are given
an instance of the partition problem with numbers $$a_{1},...,a_{n}$$.
We let both the weight and the cost of the $$i$$th item be $$a_{i}$$,
and $$t=B=\frac{1}{2}\sum_{i=1}^{n}a_{i}$$; a set $$S\subseteq\{1,...,n\}$$
is a solution to knapsack if and only if $$\sum_{i\in S}a_{i}\geq t=\frac{1}{2}\sum_{i=1}^{n}a_{i}$$
and $$\sum_{i\in S}a_{i}\leq B=\frac{1}{2}\sum_{i=1}^{n}a_{i}$$ which
means $$\sum_{i\in S}a_{i}=\frac{1}{2}\sum_{i=1}^{n}a_{i}$$ which implies
that $$S$$ is a solution to the partition problem as well.

###Bin Packing

Given a collection of items, each with a certain weight $$w_{i}$$ and
a weight bound $$B$$, we find a way to place each item into a bin such
that the a) the total weight in each bin does not exceed $$B$$ and
b) we use as few bins as possible.

Formally speaking, given a set $$\{w_{1},...,w_{n}\}$$, we wish to
minimize $$k$$ such that there is a partition of $$\{1,...,n\}$$ into
$$S_{1},...,S_{k}$$ such that for all $$i$$, $$\sum_{j\in S_{i}}w_{j}\leq B$$.

To reformulate this as a decision function, we make $$k$$ an input
parameter and ask whether or not we can pack our object into $$k$$
bins in such a way that the weight bound is satisfied.

Again, the reduction from partition is trivial: simply let $$k=2$$,
$$B=\frac{1}{2}\sum_{i=1}^{n}a_{i}$$, and $$w_{i}=a_{i}$$ for all $$i$$;
the solution to this is obviously a solution to partition.

---

For both previous problems, in addition to subset sum and partition,
there are known algorithms whose running times are polynomial in the
number of input $$a_{i}$$, the size of the inputted $$a_{i}$$'s/$$w_{i}$$'s,
and, in the case of bin packing, the size of $$k$$; however, there
is no known algorithm whose running time does not have a polynomial
dependence on the size of the inputted $$a_{i}$$'s - notice that our
reduction from vertex cover to subset sum relied on exponentially
large $$a_{i}$$'s; if we could find an algorithm that did not depend
on the size of the $$a_{i}$$'s, we could find an algorithm for NP-complete
problems that run faster than exponential time.

###Scheduling on parallel machines with no interrupt

Suppose we have $$m$$ machines, $$n$$ tasks, and tasks that take time
$$t_{ij}$$ (time to run task $$i$$ on processor $$j$$).

Suppose we wish to assign tasks to machines that minimizes completion
time.

It turns out that this problem is NP-hard even when we include the
restriction that all $$m$$ machines are identical, which we will show
here (since the machines are identical, we will denote the running
time of the $$i$$th task as $$t_{i}$$ since the running time has no
more dependency on machine).

Formulating this as a decision problem: given $$m$$ identical machines,
completion times $$t_{1},...,t_{n}$$, and a time bound $$T$$, is there
some mapping of tasks to machines $$A:\{1,...,n\}\rightarrow\{1,...,m\}$$
such that for all machines $$j\in\{1,...,m\}$$, $$\sum_{i:A(i)=j}t_{i}\leq T$$?

It is obvious from this formulation that this decision problem is
exactly the same as the decision problem form of bit packing, with
$$B$$ replaced by $$T$$, $$k$$ replaced by $$m$$, and $$w_{i}$$ replaced
by $$t_{i}$$.

Since the reduction from bin packing is trivial, there exists an algorithm
for scheduling whose running time is polynomial in both the completion
times and the number of processors.

###Steiner Tree

We will first start with the geometric interpretation of the Steiner
tree problem: given points on the plane, we wish to find a network
that connects all points and has the shortest total length. This problem
is NP-complete.

A similar problem (whose NP-completeness is easier to prove) is as
follows: given a set $$X$$, a subset $$R\subseteq X$$ of required points,
and a nonnegative distance function $$d:X\times X\rightarrow\mathbb{R}$$,
find a tree $$T=(X',E)$$ where $$R\subseteq X'\subseteq X$$ and $$\sum_{(a,b)\in E}d(a,b)$$
is minimized. As a decision problem, we have an additional parameter
$$k$$ and we want to see if we can make $$\sum_{(a,b)\in E}d(a,b)\leq k$$. 

For ease of proof, let us suppose $$d$$ is a metric (that is, $$d(x,x)=0$$,
$$d(x,y)=d(y,x)$$, and $$d(x,z)\leq d(x,y)+d(y,z)$$).

We will reduce vertex cover onto Steiner tree. Vertex cover gives
us a graph $$G=(V,E)$$ and a threshold $$t$$ - we want to figure out
if there is a vertex cover that uses $$t$$ vertices.

Make a set of points, one corresponding to each edge and one corresponding
to each vertex. The required points $$R$$ will be the set of points
corresponding to vertices.

The distance between each edge point and a vertex point corresponding
to one of the edge's endpoints will be $$1$$, and the distance between
distinct vertex points will be $$1$$ as well.

Let the distance between all other points be the distance of the shortest
path comprised entirely of length-1 distances, and let $$k$$, the threshold
for our Steiner tree decision problem, be $$k=\vert E\vert+t-1$$.

We will show that there is a vertex cover of size $$t$$ if and only
if there is a Steiner tree of cost $$k=\vert E\vert+t-1$$. Suppose
we have a vertex cover $$C\subseteq V$$ such that $$\vert C\vert=t$$.
Now consider a subgraph of the one we created, with the vertices being
the points being $$C\cup E$$ and the edges being all distance-1 connections
between them. This graph is connected by $$t+\vert E\vert$$ edges,
so it has a spanning tree with $$t+\vert E\vert-1$$ edges, which is
a Steiner tree of cost $$t+\vert E\vert-1$$.

Conversely, suppose have a Steiner tree of cost $$k=t+\vert E\vert-1$$
for our set of points; we wish to show that we can find a vertex cover
of size $$t\leq k+1-\vert E\vert$$ in the original graph. Our approach
will be to transform the Steiner tree into one that only uses length-1
connections by removing all length-2 connections and replacing each
one with length-1 connections that connect the same edges; by construction
of the weights, we do not increase the total weight of the Steiner
tree at all.

Now simplify by removing parallel edges and breaking cycles. Since
all that remain are distance-1 connections, and the tree is still
a Steiner tree, the selection of vertex points that remain comprise
a vertex cover.

Therefore, we have a reduction from vertex cover onto Steiner tree,
so Steiner tree is NP-complete.
