---
layout: post-no-feature
title: "Lecture 14"
description: "Kolmogorov Complexity"
category: articles
mathjax: true

---

Today, we will cover Kolmogorov complexity, which we will use to find
undecidable problems unrelated to the halting problem, and to find
another way to prove Gödel's incompleteness theorems and to construct
random statements that, which high probability, are unprovable along
with their complements The motivation for this approach will be to
motivate the theory of data compression, and to attack information
theory without the use of probability.

Suppose $$M$$ is a TM and $$y$$ is a string. Let $$x=M(y)$$. Then we
say that $$\langle M\rangle$$, $$y$$ is a representation of $$x$$; we
can think of it as a self-extracting version of $$x$$.

Define a *self-delimiting* bitstring as one we can tell the ending of,
i.e. if another bitstring is appended to it, we have a way to figure
out precisely where it ends and the appended bitstring begins; a
simple way to encode a bitstring into a self-delimiting one is to
record $$0$$ as $$00$$, $$1$$ as $$11$$, and end with $$01$$; an
$$m$$-length bitstring maps to a $$2m+2$$ bitstring; we can also map
$$s$$ to $$\langle m\rangle s$$, where $$m$$ is the length of
$$\langle s\rangle$$ and $$m$$ is encoded using the preceding
notation; this has length $$2\lceil\log m\rceil+2+m$$.

In fact, we can show that the smallest number of bits that we can
map $$m$$ bits to is higher than $$0.5\log m+m$$ bits; suppose for contradiction
that we can represent them with $$0.5\log m+m$$ bits. Pick some large
number $$N$$; consider all pairs $$s$$, $$t$$ binary strings of total
length $$N$$; there are $$N2^{N}$$ possible values of $$s$$ and $$t$$
($$2^{n}$$ configurations total, and $$N$$ places where we can place
the delimiter). By our assumption, we can represent $$s,t$$ (with $$m$$
and $$N-m$$ bits, respectively) as the pair $$\langle s\rangle,t$$ (with
$$\langle s\rangle$$ denoting the self-delimited representation of
$$s$$) with $$m+0.5\log m$$ and $$N-m$$ bits, respectively. Therefore,
the total number of bits in our representation $$N+0.5\log m\leq N+0.5\log N$$.

Since the number of bitstrings of length less than $$L$$ is $$1+2+4+...+2^{L}=2^{L+1}-1<2^{L+1}$$
, so the number of pairs we can represent in this manner is at most
$$2^{N+0.5\log N+1}=2\sqrt{N}2^{N}$$, which is a contradiction since
we need to tell apart more than that many (in fact, $$N2^{N}$$) different
strings. $$\blacksquare$$

---

**Definition:** Fix a self-delimiting representation of Turing Machines
$$M$$ as bit string $$\langle M\rangle$$ (that implies that there is
no valid representation of $$M$$ that is a prefix of another valid representation).
Then we define the Kolmogorov complexity $$K(x)$$ as the minimum length
of a representation of $$x$$.

Define $$M_{ID}$$ as a Turing machine that, on input $$x$$, outputs
$$x$$. Then $$\langle M_{ID}\rangle,x$$ is a representation of $$x$$,
and if the length of $$x$$ is $$n$$, the length of $$\langle
M_{ID}\rangle,x$$ is $$n+O(1)$$; notice that $$K(x)\leq\vert
x\vert+O(1)$$ (we can write down $$x$$ and an identity Turing machine,
whose length is constant). Furthermore, suppose $$C$$ is a computable
compression algorithm, and that $$D$$ is the corresponding
decompression algorithm. Then $$\langle D\rangle,C(x)$$ is a
representation of $$x$$ because $$D(C(x))=x$$ which means that
$$K(x)\leq\vert C(x)\vert+\vert\langle D\rangle\vert=\vert
C(x)\vert+O(1)$$.

A simple counting argument shows that for all $$n$$, exists a string
$$x$$ of length $$n$$ such that $$K(x)\geq n$$. Proof: suppose for contradiction
that there exists some integer $$n$$ such that all strings of length
$$n$$ have KC at most $$n-1$$. Consider all strings of that length;
for each of them, pick a representation $$\langle M_{x}\rangle,y_{x}$$
of length at most $$n-1$$. The number of representations is $$1+...+2^{n-1}=2^{n-1}$$,
but we have $$2^{n}$$ strings, which means that two strings much share
the same representation, which leads to a contradiction as the representation
is not unique.

Random strings have very high KC; in fact, given a random $$n$$-bit
string, $$P[K(x)<n-c]\leq\frac{1}{2^{c}}$$ for all $$c$$. To show this,
consider the number of strings $$x$$ such that $$K(x)<n-c$$ is at most
the number of bitstrings of length at most $$n-c$$, which is $$1+2+4+...+2^{n-c-1}\leq2^{n-c}$$,
so the probability that $$K(x)<n-c$$ is the number of strings of length
$$n$$ with KC at most $$n-c$$ divided by the number of strings of length
$$n$$ which is $$2^{n-c}/2^{n}$$ or $$2^{-c}$$, as desired.

###Incomputability of KC

Suppose for contradiction that $$K(x)$$ is computable given $$x$$. Define
the following computable function $$f$$, which takes an input $$n$$
and outputs a string $$x$$ with KC greater than $$n$$ by testing all
possible strings of length $$n$$ until we see one which satisfies the
requirement.

For every $$n$$, $$f(n)$$ is an $$n$$-bit string such that $$K(f(n))\geq n$$;
since we assume $$K(x)$$ is computable, this algorithm is well-defined;
therefore, $$\langle f\rangle,n$$ is a representation of $$f(n)$$. But
we know that the KC of this is bounded below by $$n$$ (since, by definition
of the algorithm, the output string must require at least $$n$$ bits
to represent) and bounded above by $$\log n+O(1)$$, since we can simply
specify $$\langle f\rangle,n$$ using a bitstring with constant length
specifying $$\langle f\rangle$$, and a $$\log n$$ length representation
of $$n$$. This implies that $$n\leq\log n+O(1)$$ which is false for
sufficiently large $$n$$.

Therefore, $$K(x)$$ must be incomputable. $$\blacksquare$$

---

We can use this to prove Gödel's incompleteness theorem too!

Fix a formalization of math. For every bitstring $$x$$, suppose the
statement $$s_{x}$$ corresponds to the statement "$$K(x)\geq\vert x\vert$$",
and assume that if $$s_{x}$$ is false, then $$\neg s_{x}$$ is provable.

Suppose for contradiction that the system is consistent and complete. 

Now we define a function that takes an input $$x$$, and, iterates through
all strings $$P$$ (say, in lexicographical order), accepting if $$P$$
is a proof of $$s_{x}$$.

There is a constant $$C$$ that depends only on the formalization used,
such that for all $$x$$ of length greater than $$c$$, $$s_{x}$$ has no
proof. Why? Suppose we have an integer $$n$$. We know that there must
exist some $$x$$ such that $$s_{x}$$ is provable; let $$x$$ be the first
such string (lexicographically). Then we know things: 1) the KC of
$$x$$ is greater than $$\vert x\vert$$, and 2) we can specify $$x$$ fully
with the (constant size) representation of the function and $$n$$ (we
can just, for instance, iterate through all strings of length $$x$$
in lexicographical order until we find something that the function
accepts), which means the KC is bounded by $$\log n+O(1)$$. The resulting
inequality $$n<\log n+O(1)$$ is a contradiction for sufficiently large
$$n$$.

Similarly, for every $$n$$, there are strings $$x$$ of length $$n$$ such
that $$\neg s_{x}$$ has no proof.

This shows the existence of unprovable statements.

*Thanks to Luca Trevisan for pointing out a misleading stray "Proof: " flying around.*
