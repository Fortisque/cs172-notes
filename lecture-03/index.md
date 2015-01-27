---
layout: post-no-feature
title: "Lecture 3"
description: "Nondeterministic Finite Automata"
category: articles
mathjax: true

---

**INCOMPLETE**: I have figures, but I won't be able to scan them in for another hour and a half, so there are no images at the moment.

**Administrivia:** We have two TAs now! Instructor office hours have been set on Tuesdays at 1730. Discussion sections start tomorrow.

An NFA is an automaton like a DFA, except that the labels on the transitions are arbitrary.

Like a DFA, an NFA starts a computation in its initial state and consumes input one at a time from the initial string.

Upon encountering a character, we take *all* transitions from it corresponding to the character.

![](figure-1.jpg)

Alternatively, we can keep a list of active states at each stage instead of tracking the entire computation - at each stage, the new list of active states is the union of the transitions coming from the current list of active states.

At the end of the string, if any terminating state is active, then we return YES; otherwise, we return NO. For example, the NFA above accepts all strings where the second-last chracater is an `a`.

More formally, an input $$s$$ is accepted if and only if there is a path from the start state to a state in $$F$$ such that concatenating the labels of the edges of the path gives $$s$$. This notion holds for both DFAs and NFAs. 

Theorem: Let $$L$$ be a language. There exists a DFA that decides $$L$$ if and only if there exists an NFA that decides $$L$$.

The “if” definition is obvious: any DFA can be thought of as an NFA that happens to have one. 

To prove the other direction: intuitively, we can create a DFA state for each of the (up to $$2^{n}$$) possible sets of active NFA states. For instance, for the NFA above, we want a DFA that looks like this:

![](figure-2.jpg)

###Formalism

Definition: An NFA $$M=(Q,\Sigma,\delta,q_{0},F)$$ is specified by the following:

- A finite set $$\Sigma$$ representing the alphabet.

- The (finite) set of states, $$Q$$

- The transition function $$\delta:Q\times\Sigma\rightarrow\{Q\}$$ which, given $$q\in Q$$ and a character $$a\in\Sigma$$, returns a new *set of states* $$\delta(q,a)\subseteq Q$$. This is the main difference between a DFA and an NFA: the DFA's transition function must return a single state, while the NFA's returns a *set* of states.

- A start state $$q_{0}$$

- The set of terminal states $$F\subseteq Q$$

Let's return to the theorem and formally define the DFA corresponding to an NFA. Suppose $$L\subseteq\Sigma^{*}$$ and $$N=(Q,\Sigma,\delta,q_{0},F)$$ is a NFA that decides $$L$$. We wish to find a DFA $$M=(Q',\Sigma,\delta',q_{0}',F')$$ that also decides $$L$$. Define $$M$$ as follows:

- Let $$Q'$$ be the set of all subsets of $$Q$$.

- $$\delta'(s,a)
$$, where S is the set of states $$\{s_{1}...s_{k}\}
$$, is defined to be $$\delta(s_{1},a)\cup\delta(s_{2},a)\cup...\cup\delta(s_{k},a)
$$, or, alternatively, $$\{q\in Q:\exists s\in S:q\in\delta(s,a)\}
$$

- $$q_{0}' $$ is the single-element set $$q_{0}$$.

- $$F'$$ is defined as the set of all sets that include at least one element in $$F$$; that is, $$F'=\{s:|s\cap F|\ge1\}$$

We can now proceed by induction to complete the proof. $$\blacksquare $

We also allow for $$\epsilon$$ transitions, where $$\epsilon$$ represents the empty character. When we encounter one, we just activate to the states returned by the $$\epsilon$$-transition without consuming a chracter.

We can trivially change an NFA with $$\epsilon$$-transitions to one without by replacing every path comprised of any number of $$\epsilon$$-transitions and one nontrivial transition with a direct edge containing the nontrivial transition. If there are $$\epsilon$$-transitions, we can bypass the epsilon transitions by connecting the start state to nodes the original target of the $$\epsilon$$-transition node. We must also make all states that have an epsilon transition to a terminating states terminating as well.

![](figure-3.jpg)

###Concatenation, revisited

Recall from the previous lecture that the *concatenation* of two languages $$L_{1}$$ and $$L_{2}$$ is defined as the language $$L_{1}\circ L_{2}=\{s\circ t:s\in L_{1},t\in L_{2}\}$$. If we are given DFAs $$M_{1}$$ deciding $$L_{1}$$ and $$M_{2}$$ deciding $$L_{2}$$, we want to find an automaton that accepts that $$L_{1}\circ L_{2}$$ by testing every single possible “split” of the input stream. This is difficult to express directly with a DFA due to backtracking involved, but it can be done cleanly by an NFA by making all terminating states of $$M_{1}$$ nonterminal, and creating epsilon transitions from all of those states to the start state of $$M_{2}$$. This new automaton will accept input if there exists some path from the start state of $$M_{1}$$ to a terminating state of $$M_{2}$$ - but all such paths must necessarily pass first from the start state of $$M_{1}$$ to a (formerly) accepting state of $$M_{1}$$ (so the string starts with something in $$L_{1}$$) and then immediately go to a start state of $$M_{2}$$ before taking a path through $$M_{2}$$ to a terminating state in $$M_{2}$$, which means that the remainder of the string is in $$L_{2}$$.

If we have a language $$L$$, define $$L^{*}=\{s:\mbox{ there exist strings }s_{1},...,s_{k}\mbox{ all in }L\mbox{ and }s=s_{1}\circ s_{2}\circ...\circ s_{k}\}\cup\{\epsilon\}$$ and an automaton that accepts $$L$$, we can create an automaton that accepts $$L^{*}$$ by connecting all terminating states to the original search state (which we must make a terminating state) with $$\epsilon$$-transitions.
