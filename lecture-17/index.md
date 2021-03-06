---
layout: post-no-feature
title: "Lecture 17"
description: "SAT and 3SAT"
category: articles
mathjax: true

---

**Note:** I placed proof of $$\mathbf{NP}$$-completeness of CKT-SAT at the end of
[lecture 16's notes]({{ site.url }}/classes/cs172/notes/lecture-16) so as not to
split the proof across two pages.

It is trivial to show that (since the composition of poly-time computable
functions is itself poly-time computable) if $$A\leq_{m}^{p}B$$ and
$$B\leq_{m}^{p}C$$, then $$A\leq_{m}^{p}C$$.

That means that if we can show that $$A$$ has a poly-time reduction
onto any known NP-complete problem (e.g. CSAT), we know that $$A$$
is $$\mathbf{NP}$$-hard (and $$\mathbf{NP}$$-complete if $$A\in\mathbf{NP}$$).

In this lecture and the next, we'll look at a web of reductions that
shows us that certain problems are $$\mathbf{NP}$$-hard or $$\mathbf{NP}$$-complete.

![](reduction-web.png)

###Satisfiability

Define *variables* as symbols $$x_{1},...,x_{n}$$ that can take
on true or false values. We use the term *literal* to describe
either a variable $$x_{i}$$ or its negation $$\overline{x_{i}}$$, and
*clause* to describe a bunch of variables OR-ed together. We call
the AND of an arbitrary number of clauses a *formula* in *conjunctive
normal form*, or CNF.

For example, $$(x_{1}\vee\overline{x_{2}})\wedge(x_{1}\vee\overline{x_{3}}\vee x_{4})\wedge(\overline{x_{1}}\vee x_{3})$$
is a CNF formula.

We define *satisfiability*, abbreviated SAT, as the following
problem: givena boolean formula in CNF, is there a satisfying assignment
to the variables that causes the entire formula to be true.

We will show that SAT is $$\mathbf{NP}$$-complete by reducing CSAT
onto it.

A naive way to go about this is to, when given a circuit $$C$$ over
inputs $$x_{1},...,x_{n}$$ with $$S$$ gates, construct a CNF formula
$$\phi$$ over variables $$y_{1},y_{2},...$$ such that $$C$$ is satisfiable
if and only if $$\phi$$ is. The problem is that there can be an the
CNF may have exponential size; for instance, boolean parity can be
done in linear number of gates, but you need a $$n2^{n}-1$$ size CNF
formula to do it.

Instead, we will, given $$C$$ with $$n$$ inputs, use $$s$$ gates to construct
a CNF $$\phi$$ with $$n+S$$ variables, one for each input and output
of each gate, with clauses ensuring that the output of a given gate
is consistent with the input to that gate. In particular, here are
the clauses corresponding to each gate:

- An and gate with inputs $$x$$ and $$y$$ with output $$z$$ corresponds
to the CNF expression: $$(\overline{z}\vee x\vee y)\wedge(\overline{z}\vee x\vee\overline{y})\wedge(\overline{z}\vee\overline{x}\vee y)\wedge(z\vee\overline{x}\vee\overline{y})$$.

- An or gate with inputs $$x$$ and $$y$$ with output $$z$$ corresponds
to the CNF expression: $$(\overline{z}\vee x\vee y)\wedge(z\vee\overline{x}\vee y)\wedge(z\vee x\vee\overline{y})\wedge(z\vee\overline{x}\vee\overline{y})$$

- A not gate with input $$x$$ and output $$z$$ corresponds to $$(x\vee z)\wedge(\overline{x}\vee\overline{z})$$

We also have a clause consisting of a single variable corresponding
to the output of the circuit to ensure that the output is TRUE.

If $$C(x)=1$$ for some $$x$$, then $$\phi$$ must be satisfiable (one
can just plug in $$x$$ into the circuit and read off the values at
the output of each gate to find the satisfying assignment to the CNF).
Furthermore, if some $$a_{1}...a_{n+S}$$ satisfies $$\phi$$, then $$a_{1}...a_{n}$$
is a satisfying input to the circuit.

###3-SAT

It is often convenient to specify exactly the number of literals in
a clause when reducing satisfiability results to others.

Notice that the output of CSAT only consists of clauses with at most
three distinct variables (AND and OR gates produce three-variable
clauses, NOT gates produce two-variable clauses, and the single variable
at the end is a single-variable clause). We now show this problem,
which we call E3SAT - the satisfiability problem with *at most*
three distinct variables per clause - reduces onto 3SAT, thesatisfiability
problem with *exactly* three distinct variables per clause.

We simply port clauses with three variables over unchanged: $$(x\vee y\vee z)\rightarrow(x\vee y\vee z)$$.
We introduce one dummy variable in two-variable clauses: $$(x\vee y)\rightarrow(x\vee y\vee a)\wedge(x\vee y\vee\overline{a})$$;
similarly, for one-variable clauses, we introduce two dummy variables:
$$x\rightarrow(x\vee a\vee b)\wedge(x\vee\overline{a}\vee b)\wedge(x\vee a\vee\overline{b})\wedge(x\vee\overline{a}\vee\overline{b})$$.

###Graph reductions

Given an undirected graph $$G=(V,E)$$, we define the following:

- $$K\subseteq V$$ is a clique if and only if for all $$u,v$$ in $$K$$,
$$(u,v)\in E$$ (i.e. the induced subgraph is complete)

- $$I\subseteq V$$ is an independent set if and only if for all $$u,v$$
in $$K$$, $$(u,v)\notin E$$ (i.e. the induced subgraph has no edges)

- $$C\subseteq V$$ is a vertex cover if for all $$(u,v)\in E$$, either
$$u\in C$$ or $$v\in C$$.

The smallest vertex cover is equivalent to largest independent set
(if we remove all the vertices in the smallest vertex cover, then
all of the edges lack at least one endpoint, so we have an independent
set left).

Similarly, the clique of a graph is the independent set of its inverse
graph (remove all edges and add edges that weren't present in the
original graph).

All three of these problems are equivalent!

In the next lecture, we will show that they are $$\mathbf{NP}$$-complete
as well.
