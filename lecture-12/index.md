---
layout: post-no-feature
title: "Lecture 12"
description: "Reductions, Rice's Theorem, and Gödel's First Incompleteness Theorem"
category: articles
mathjax: true

---

**Definition:** Suppose we have two languages $$L_{1}\subseteq\Sigma_{1}^{*}$$ and $$L_{2}\subseteq\Sigma_{2}^{*}$$. We say that $$L_{1}$$ has a *mapping reduction* to $$L_{2}$$, denoted $$L_{1}\leq_{m}L_{2}$$, if there is a computable function $$f:\Sigma_{1}^{*}\rightarrow\Sigma_{2}^{*}$$ such that for all $$x$$ in $$\Sigma_{1}^{*}$$, $$x\in L_{1}$$ if and only if $$f(x)\in L_{2}$$.

$$f$$ is not required to be either one-to-one or onto.

It is obvious that, given $$L_{1}\leq_{m}L_{2}$$, if $$L_{2}$$ is decidable, then $$L_{1}$$ is also decidable - given a string $$x$$, one can test if $$x$$ is in $$L_{1}$$ by feeding $$f(x)$$ into the decider for $$L_{2}$$.

The main use of this fact is for proofs by contraposition that certain languages are undecidable: if $$L_{1}\leq_{m}L_{2}$$, and $$L_{1}$$ is not decidable, then $$L_{2}$$ is not decidable.

We can use a similar proof to show that given $$L_{1}\leq_{m}L_{2}$$, if $$L_{2}$$ is recognizable, then $$L_{1}$$ is also recognizable - given a string $$x$$, one can build a recognizer for $$L_{1}$$ that works by taking input $$x$$ feeding $$f(x)$$ into the recognizer for $$L_{2}$$ (and accepting if $$L_{2}$$'s recognizer accepts). The contrapositive of this fact is useful for showing that certain languages are unrecognizable by, for instance, finding a reduction from $$H^{C}$$ to a given language.

###A Complementary Example

Let $$COMP$$ be defined as the language of pairs $$(\langle M_{1}\rangle,\langle M_{2}\rangle)$$ such that $$L(M_{1})$$ is a complement of $$L(M_{2})$$. We wish to prove that this is undecidable by showing that $$H\leq_{m}COMP$$.

We wish to define a transformation that works as follows: $$\langle M\rangle,x\rightarrow f(\langle M\rangle,x)=(M_{1},M_{2})$$ where $$M_{1}$$ and $$M_{2}$$ are defined such that if $$M$$ halts on input $$x$$, then $$L(M_{1})=\overline{L(M_{2})}$$ and if $$M$$ does not halt on input $$x$$, then $$L(M_{1})\neq\overline{L(M_{2})}$$.

We can define $$M_{1}$$ and $$M_{2}$$ satisfying this condition as follows: $$M_{1}$$ takes input $$z$$, simulates $$M(x)$$, and then accepts. If $$M$$ halts, then $$L(M_{1})=\Sigma^{*}$$; if $$M$$ never halts, then neither does $$M_{1}$$ so $$L(M_{1})=\emptyset$$. $$M_{2}$$ always rejects so $$L(M_{2})=\emptyset$$.

Therefore, we can formally define $$f$$ as a function that takes inputs $$\langle M\rangle$$ and $$x$$, defines $$M_{1}$$ and $$M_{2}$$ as in the preceding paragraph, and returns $$(\langle M_{1}\rangle,\langle M_{2}\rangle)$$; $$f$$ is a computable function, so $$H\leq_{m}COMP$$ which implies that $$COMP$$ is undecidable.

---

Another useful fact: if $$L_{1}\leq_{m}L_{2}$$, then $$\overline{L_{1}}\leq_{m}\overline{L_{2}}$$, since $$\forall x:x\in L_{1}\iff f(x)\in L_{2}$$ holds if and only if $$\forall x:x\in\overline{L_{1}}\iff f(x)\in\overline{L_{2}}$$ is true.

This implies that $$H^{C}\leq_{m}\overline{COMP}$$, which means that $$\overline{COMP}$$ is unrecognizable.

---

We now wish to show that $$COMP$$ is unrecognizable by showing that $$H^{C}\leq_{m}COMP$$; it suffices to define a function $$g(\langle M\rangle,x)\rightarrow(M_{1},M_{2})$$ such that if $$M(x)$$ does not halt, then $$L(M_{1})=\overline{L(M_{2})}$$, and if $$M(x)$$ halts, then $$L(M_{1})\neq\overline{L(M_{2})}$$.

Define $$M_{1}$$ as follows: given an input $$z$$, simulate $$M(x)$$, and accept. If $$M(x)$$ halts, then $$L(M_{1})=\Sigma^{*}$$, and if it does not, then $$L(M_{1})=\emptyset$$. Have $$M_{2}$$ accept all strings.

Therefore, $$g$$ can be defined as a function that takes $$\langle M\rangle$$ and $$x$$, returns $$M_{1}$$ and $$M_{2}$$, and then returns $$(\langle M_{1}\rangle,\langle M_{2}\rangle)$$; if $$M$$ halts, then $$M_{1}$$ does not accept the complement of $$M_{2}$$; while if $$M$$ halts, then $$M_{1}$$ does accept the complement of $$M_{2}$$. This shows that $$COMP$$ is unrecognizable as well.

This is the strongest complexity we will examine in this course: not only is the language $$COMP$$ unrecognizable, but its complement is also unrecognizable.

###Rice's Theorem

Rice's theorem states that *any* language $$\{\langle M\rangle:L(M)\mbox{ is }...\}$$, where “...” is some property is always undecidable, unless “...” is “recognizable” or “not recognizable”.

Formally, Rice's theorem can be expressed as follows: if $$C$$ is a set of languages (e.g. “all infinite languages”, or “set of regular languages”, or $$\{\emptyset\}$$ if we want to decide if a machine doesn't accept any input, or $$\mathbf{P}$$), then define $$L_{C}$$ as the set of languages $$\langle M\rangle$$ such that $$L(M)\in C$$. For every $$C$$ such that $$L_{C}$$ is nonempty and is not the set of all TMs, then $$L_{C}$$ is undecidable.

**Proof:** Suppose $$C$$ is a set of languages and define $$L_{c}:=\{\langle M\rangle:L(M)\in C\}$$. Suppose that there exists $$\langle M_{1}\rangle\in L_{C}$$ and $$\langle M_{2}\rangle\notin L_{C}$$. We have two cases:

**Case 1:** $$\emptyset\notin C$$. We will do a reduction $$H\leq_{m}L_{c}$$. It suffices to define a computable function $$f(\langle M\rangle,x)=M_{x}$$ such that if $$M(x)$$ halts, $$L(M_{x})\in C$$, and if $$M(x)$$ does not halt, then $$L(M_{x})\notin C$$.

Given $$M$$ and $$x$$, define $$M_{x}$$ as a machine that takes input $$z$$, simulates $$M(x)$$, and then simulate $$M_{1}(z)$$. If $$M(x)$$ halts, then $$L(M_{x})=L(M_{1})\in C$$, and if $$M(x)$$ does not halt, then $$L(M_{x})=\emptyset\notin C$$, as desired.

**Case 2:** $$\emptyset\in C$$. We will do a reduction $$H^{C}\leq_{m}L_{c}$$. It suffices to define a computable function $$g(\langle M\rangle,x)=M_{x}$$ such that if $$M(x)$$ does not halt, then $$L(M_{x})\in C$$, and if $$M(x)$$ halts, then $$L(M_{x})\notin C$$. 

Given $$M$$ and $$x$$, define $$M_{x}$$ as a machine that takes input $$z$$, simulates $$M(x)$$, and then simulate $$M_{2}(z)$$. If $$M(x)$$ halts, then $$L(M_{x})=L(M_{2})\notin C$$, and if $$M(x)$$ does not halt, then $$L(M_{x})=\emptyset\in C$$, as desired. $$\blacksquare$$ 

This proof actually gives us a stronger result: if $$\emptyset\in C$$, then $$L_{C}$$ is not only undecidable but also unrecognizable, and if $$\emptyset\notin C$$, then $$\overline{L_{c}}$$ is unrecognizable.

###Gödel's First Incompleteness Theorem

Suppose we have a formal language to write mathematical statements and mathematical proofs. We will make three assumptions:

- Given a TM $$M$$ and an input $$x$$, there is a statement in our language $$s_{M,x}$$ that means “$$M$$ halts on input $$x$$”, and such statement is computable given $$\langle M\rangle$$ and $$x$$.
- Given $$S$$, $$P$$, it is decidable if $$P$$ is a proof of $$s$$.
- If $$M$$ halts on input $$x$$, then $$s_{M,x}$$ is provable (e.g. by describing the steps that $$M$$ takes on input $$x$$. 
- The set of statements is closed under boolean operations.

Define a formalism as *consistent* if there is no statement $$s$$ such that $$s$$ and $$\neg s$$ are both provable.

We will show that in every consistent formalism, there is a statement $$s$$ such that neither $$s$$ nor $$\neg s$$ are provable (Gödel's first incompleteness theorem). 

**Proof:** Suppose for contradiction we have a consistent formalism such that for every statement $$s$$, exactly one of $$s$$ and $$\neg s$$ has a proof.

We shall derive an algorithm for the halting problem as follows: given an input $$\langle M\rangle$$, $$x$$, construct $$s_{m,x}$$.

Initialize $$k$$ to $$1$$ . Start an infinite loop, incrementing $$k$$ at each step: for each $$P$$ of length $$k$$, if $$P$$ is a proof of $$s_{M,x}$$, accept; otherwise, if $$P$$ is a proof of $$\neg s_{M,x}$$, then reject.

Notice that this decides the halting problem, since we know that $$s_{M,x}$$ must have a proof of some kind. Also, note that this must terminate, since a proof of infinite length makes no sense.

*Thanks to TongKe Xue for pointing out mistake and Anonymous for pointing out typesetting error.*
