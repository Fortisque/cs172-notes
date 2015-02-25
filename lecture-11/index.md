---
layout: post-no-feature
title: "Lecture 11"
description: "Examples of Undecidable and Unrecognizable Languages"
category: articles
mathjax: true

---

###Is the TM Regular?

Let REG be the language of TM representations $$\langle M\rangle$$ such that the language that $$M$$ accepts is a regular language. We will show that REG is undecidable.

Suppose for contradiction that REG is decidable. We will devise an algorithm for devising $$H$$, given inputs $$\langle M\rangle$$ and $$y$$.

Define $$M_{y}$$ as follows: taking an input $$x$$, simulate $$M$$ on input $$y$$, and then accept $$x$$ if and only if $$x\in MP$$ where $$MP$$ is the language of matched paretheses (any other irregular language will do). Notice that if $$M$$ does not halt on $$y$$, then it recognizes the empty language (which is regular) since it will never halt on any input, and if $$M$$ does halt on $$y$$, then it accepts a nonregular language.

Now we can just call the algorithm to REG on $$M_{y}$$, reject if $$M_{y}$$ is in REG and accept if it is not in REG.

To show that this actually does recognize $$H$$: if $$(\langle M\rangle,y)\in H$$, then $$\{x:M_{y}\mbox{ accepts }x\}=MP$$, which is not regular, and the algorithm accepts. On the other hand, if $$(\langle M\rangle,y)\notin H$$, then $$\{x:M_{y}\mbox{ accepts }x\}=\emptyset$$, which is regular, and the algorith rejects.

We can also show that REG is undecidable. using a similar method. Define $$M_{y}$$ as we did above, and run the recognizer for REG on the input $$\langle M_{y}\rangle$$; accept if the recognizer for REG recognizes on $$M_{y}$$ and reject if the recognizer rejects. There are three cases: $$(\langle M\rangle,y)\in H^{C}$$, so $$M_{y}$$ recognizes $$\emptyset$$, which means that $$\langle M_{y}\rangle\in REG$$, so the recognizer for REG accepts. If $$(\langle M\rangle,y)\notin H^{C}$$, then the recognizer for REG either rejects or infinite loops on $$M_{y}$$; in either case, we either reject or infinite loop which means that we have a recognizer for $$H^{C}$$.

###Equivalency

Now given two DFAs $$M_{1}$$ and $$M_{2}$$, we want to know if they recognize the same language.

Define $$M$$ is a DFA and let $$L(M)=\{x:M\mbox{ accepts }x\}$$. Define $$EQ_{DFA}$$ to be the set of pairs of DFAs $$(\langle M_{1}\rangle,\langle M_{2}\rangle)$$ such that the languages that they accept are the same.

Given $$M_{1}$$ and $$M_{2}$$, consider the language $$D:=L(M_{1})\triangle L(M_{2})$$ where $$A\triangle B=(A\cup B)\backslash(A\cap B)$$ such that $$D\neq\emptyset$$.

Given $$M_{1}$$, $$M_{2}$$, we can efficiently construct $$M$$ such that $$L(M)=D$$: let $$M_{1}=(Q_{1},\Sigma,\delta_{1},q_{01},F_{1})$$ and $$M_{2}=(Q_{2},\Sigma,\delta_{2},q_{02},F_{2})$$. Let $$M=(Q_{1}\times Q_{2},\Sigma,\delta,q_{0},F)$$ where $$\delta((q_{1},q_{2}),a)=(\delta(q_{1},a),\delta(q_{2},a))$$ and $$F=\{(q_{1},q_{2}):(q_{1}\in F\wedge q_{2}\notin F_{2})\vee(q_{1}\notin F_{1}\wedge q_{2}\in F_{2})\}$$; we can trivially check (using a DFS traversal, for instance) if there is some string that leads $$M$$ to a terminating state (implying that $$D=L(M)$$ is nonempty); therefore, $$EQ_{DFA}$$ is decidable in quadratic time.

The same cannot be said for Turing machines. Define $$EQ_{TM}$$ be the set of pairs of TMs $$(\langle M_{1}\rangle,\langle M_{2}\rangle)$$ such that the languages that they accept are the same.

Suppose $$EQ_{TM}$$ is decidable. We want to use decider for $$EQ_{TM}$$ to get the decider for $$H$$. Given an input $$\langle M\rangle$$ and $$y$$, define $$M_{1}$$ as the machine that takes input $$x$$, simulates $$M(y)$$, and, if the simulation halts, accepts, and $$M_{2}$$ to be the machine that takes input $$x$$ and always rejects. Suppose $$M$$ halts on input $$y$$; this causes $$M_{1}$$ to accept on all inputs and $$M_{2}$$ to reject on all inputs; therefore, $$(\langle M_{1}\rangle,\langle M_{2}\rangle)\notin EQ_{TM}$$; alternatively, if $$M$$ does not halt given $$y$$, then $$M_{1}$$ accepts no input and $$M_{2}$$ rejects all input, which means that $$(\langle M_{1}\rangle,\langle M_{2}\rangle)\in EQ_{TM}$$.

This means that $$EQ_{TM}$$ is undecidable.

###Linear-bounded Automatons

Consider the linear-bounded automaton, or LBA: a turing machine that cannot write over blank symbols, and whose tape does not continue after the end of input. This models algorithms that, given an input of length $$n$$, use $$O(n)$$ bits of memory.

Fact: given an LBA $$M$$ and a string $$x$$, it is decidable if $$M$$ halts on input $$x$$. Notice that every configuration of the LBA can be represented by the current state, the position of the head, and the current contents of the input; there are therefore $$N=\vert Q\vert n\vert\Gamma\vert^{n}$$ where $$n=\vert x\vert$$. All we need to do is to simulate $$M$$ on input $$x$$ for $$N$$ steps; if it runs for more steps, then some configuration must have been repeated twice, which means that the machine never stops.

Although we can figure out what happens for an LBA given one string, it is still too hard to figure out what happens for *all* strings. Formally, let $$EQ_{LBA}$$ to be the set of pairs of LBAs $$(\langle M_{1}\rangle,\langle M_{2}\rangle)$$ such that the languages that they accept are the same. We will show that $$EQ_{LBA}$$ is undecidable.

Suppose for contradiction that $$EQ_{LBA}$$ is actually decidable. We will come up with an algorithm for $$H$$: given inputs $$\langle M\rangle$$ and $$y$$, where $$M$$ is a full-fledged TM, we define two LBAs $$M_{1}$$ and $$M_{2}$$ as follows:

Define $$M_{1}$$ to be the machine that takes input $$x$$ and simulates $$M(y)$$ for $$\mbox{length}(x)$$ steps; if the simulation halts, the simulation accepts and otherwise it rejects.

Define $$M_{2}$$ to be an LBA that always rejects.

If $$M$$ does not halt on input $$y$$, then the first machine always rejects so the languages are equivalent; on the other hand, if it does halt sometime, then there is some $$x$$ that causes $$M_{1}$$ to accept which means the two languages are not the same.
