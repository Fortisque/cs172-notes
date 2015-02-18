---
layout: post-no-feature
title: "Lecture 9"
description: "Enumerators and NDTMs"
category: articles
mathjax: true

---

**Administrivia:** Today's office hours are moved to tomorrow from 1000-1130. The first midterm will be next Thursday, the 26th of February, in class. This Thursday, instead of a new homework, a practice exam will be released.

Last time, we defined Turing machines, and defined what it meant for a language to be decidable or recognizable. Today, we'll cover enumerators, characterize recognizable languages, and nondeterministic Turing machines, and the famous halting problem.

###Enumerators

An enumerator is a Turing machine that produces a string (with no limits on the size) as output.

An enumerator is a two-tape machine, with one tape write-only. We can model it using a transition function

$$\delta:Q\times\Gamma\rightarrow Q\times\Gamma\times\{\Sigma\cup\{\square,\#\})\times\{L,R\}\times\{S,R\}$$ 

which takes a state and a character read from the read/write tape, and returns a new state, a character in $$\Gamma$$ to write to the read/write tape, a character in $$\{\Sigma\cup\{\square,\#\})$$ (with $$\#\notin\Sigma$$ as a separator character) to write to the write-only tape, an instruction to the R/W tape to move left or right, and an instruction to the write tape to either stay in the same location or move right (we cannot move backwards on the write tape). Initially, the machine is at the first position for both tapes, which are both completely blank.

An enumerator $$E$$ *outputs* a string $$w\in\Sigma^{*}$$ if from some point in time, the string $$\#w\#$$ appears on the writeonly tape to the left of the writing device.

Define $$L(E)$$ to be the set of strings in $$\Sigma^{*}$$ that are outputted by $$E$$ at least once.

**Theorem:** A language $$L\subseteq\Sigma^{*}$$ is recognizable if and only if there is an enumerator $$E$$ such that $$L(E)=L$$.

Proving the if direction is easy: we can simulate the enumerator using a two-tape TM $$M$$ (which we proved to be equivalent in computational power to a normal one-tape TM in the previous lecture) as follows: first, given an input $$x$$ on the Tape 2, $$M$$ first moves to the end of $$x$$, then simulates all the transitions of $$E$$ (with tape 1 simulating the write tape from $$E$$ and the part of tape 2 after $$x$$ simulating the R/W tape from $$E$$).. At each transition that writes the character $$\#$$ on tape $$1$$, $$M$$ checks if the string $$\#...\#$$ equals $$x$$. If it's equal, then $$M$$ accepts; if we never see $$x$$, then we just keep going (which is fine since recognizability means that we don't accept if the string is in the language - infinite execution is OK).

Conversely, suppose we have a machine $$M$$ that recognizes $$L\subseteq\Sigma^{*}$$. A naive approach is to make an enumerator that works as follows: for each $$k\ge0$$, for each $$s\in\Sigma^{k}$$, print $$s$$ if $$M$$ accepts $$s$$. There's a problem with this approach - $$M$$ isn't guaranteed to terminate on elements in $$L$$! That means the enumerator might never get to anything (e.g. if $$M$$ infinite-loops on the empty string).

Instead, let's try a similar approach, except we bound the number of steps in each step. That is: for each $$B\ge0$$, for each $$s\in\Sigma^{*}$$ of length at most $$B$$, if $$M$$ accepts $$s$$ in at most $$B$$ steps, then print $$s$$. Since the number of strings of length is at most $$B$$ is finite, and each of those strings is given at most $$B$$ time to be tested, each step runs in finite time; therefore, this TM has no chance of hanging like the naive one did.

We now show that $$L(E)=L$$ by showing that (a) L(E)\subseteq $$L$$ and (b) L\subseteq $$L(E)$$. To prove (a): if $$s$$ is an output of $$E$$, then $$M$$ accepts $$s$$, and $$s\in L$$. To prove (b): Suppose $$M$$ accepts $$s$$. Let $$n=\vert S\vert$$, and let the time for $$M$$ to accept be $$t$$; it is obvious that for all $$B\ge\max\{t,n\}$$, $$E$$ outputs $$s$$ in phase $$B$$. $$\blacksquare$$ 

###Nondeterministic TMs

Recall that the transition function of a normal TM has the form:

$$\delta:Q\times\Gamma\rightarrow Q\times\Gamma\times\{L,R\}$$ 

We define a *nondeterministic TM* using a transition function of the form:

$$\forall q\in Q,a\in\Gamma:\delta(q,a)\subseteq Q\times\Gamma\times\{L,R\}$$ 

That is, our transition function takes returns a *set* of possible actions. The NDTM accepts if there exists any path consisting of elements of the “next step” set that leads to the accept state.

Define a *configuration* of a TM to be: (a) the content of the tape, (b) the current state, and (c) the current position of the tape. There are many ways to represent this; one way is to treat it as an element of $$\Gamma^{*}\times Q\times\Gamma^{*}$$ - if our TM is at position $$i$$ on the tape, we can represent its configuration as $$s_{1}...s_{i-1}qs_{i}...s_{n}$$.

It is easy to define the next step configuration, given the current configuration - using the aforementioned notation, the next-step configuration of $$s_{1}...s_{i-1}qs_{i}...s_{n}$$ is $$s_{1}...s_{i-1}aq's_{i+1}...s_{n}$$ if $$\delta(q,s_{i})=q',a,R$$ and $$s_{1}...s_{i-2}q's_{i-1}as_{x+1}...s_{n}$$ if $$\delta(q,s_{i})=q',a,L$$.

For a turing machine $$M$$ given input $$x$$, denote the first step configuration as $$q_{0}x$$. An $$n$$-step computation on an input $$x$$ is a sequence $$c_{0},...,c_{n}$$ of configurations where$$ c_{0}=q_{0}x$$ where $$c_{i+1}$$ is a next step configuration after $$c_{i}$$.

For a normal (deterministic) TM, we know *the* next step configuration after a give state. In an NDTM, each configuration has a *set* of next step configurations, and the NDTM accepts if there is a computation (set of configurations) $$c_{0},...,c_{n}$$ where $$c_{0}=q_{0}x$$, $$c_{i+1}$$ is a next step configuration of $$c_{i}$$ (sometimes denoted $$c_{i}\vdash c_{i+1}$$, and $$c_{n}$$ is a configuration with an accept state.

We now wish to show that we can build a deterministic TM to recognize any language that an NDTM can recognize.

Suppose we have a language $$L\subseteq\Sigma^{*}$$ and we have an NDTM $$M$$ that recognizes $$L$$. We wish to show that there exists a deterministic recognizer for that language, which we build as follows:

Given an input $$x$$, and a configuration $$\{q_{0}x\}$$, our machine works as follows:

{% highlight text %}
while true:
    N := ∅
    for each c ∈ C: 
        for each c' that is a next step configuration for c 
            define N to be N ∪ {c'}
            if c' is accepting, return TRUE
    C := N
{% endhighlight %}

Note that this mathine will never reject (it will infinite loop if it doesn't accept).
