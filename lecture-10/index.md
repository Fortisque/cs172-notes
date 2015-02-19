---
layout: post-no-feature
title: "Lecture 10"
description: "Undecidable and Unrecognizable Languages"
category: articles
mathjax: true

---

Halting problem: checking if a Turing machine halts on all inputs is not recognizable.

We will use this notation: if $$M$$ is a turing machine, then $$\langle M\rangle$$ is a string in $$\{0,1\}$$ that represents $$M$$. The *halting problem* is as follows: given a TM $$M$$ and an input $$x$$, we wish to determine whether or not $$M$$ halts on input $$X$$. We can represent this as the language $$H$$ of all pairs $$(\langle M\rangle,x)$$ such that $$M$$ halts on input $$x$$. We will show that $$H$$ is undecidable.

####The short proof

Suppose for contradiction that $$H$$ is decidable. Then a TM $$M_{H}$$ that decides $$H$$ much exist. 

Intuitively, our approach will be to a “time-travel” paradox that works as follows: the program will feed itself to$$ M_{H}$$ see if halts, and then, if it does, infinite loop, therefore creating a contradiction.

Since it is complex to make a program that feeds its own code to another machine, we'll define this program:

{% highlight text %}
function M_D(x):
    compute M_H(x,x)
    if M_H(x,x) accepts:
        infinite loop
    else:
        halt
{% endhighlight %}

Now what if we perform $$M_{D}$$ on $$\langle M\rangle$$?

If $$M$$ halts on input $$\langle M\rangle$$, $$M_{D}$$ loops. If $$M$$ does not halt on input $$\langle M\rangle$$, $$M_{D}$$ halts. But what does $$M_{D}$$ do on input $$\langle M_{D}\rangle$$? This leads to an immediate contradiction and therefore $$H$$ is undecidable. $$\blacksquare$$

###Computable numbers

Here's another perspective:

Turing was trying to get a definition of a **computable number**. In a general sense, a number is computable if there exists an algorithm that will write out the number to arbitrary precision given enough time.

Formally, a number $$0\le x\le1$$ if computable if there exists a two-tape TM with a write-only tape and a read/write tape (much like the enumerator) such for every digit $$i$$ of $$x$$ there is some time $$t$$ such that the $$i$$-th digit is written by time $$t$$. If $$x$$ has finitely many digits, the machine can either loop or keep writing zeroes after writing all the nontrivial digits of $$x$$.

Notice that there are only countably many computable numbers - for every two different countable numbers, there are two distinct TMs, and TMs are countable. It is also well known that there are uncountably many real numbers between $$0$$ and $$1$$. That means there are numbers that cannot be computed.

Let's look at Cantor's diagonalization argument for the uncountably infinite number of numbers: suppose we have a countable list of numbers between 0 and 1. Then we could enumerate them in a table, and then take the diagonal elements of the table and add 1 to all of them. The number comprised by the digits on the diagonal differ from all the numbers in the table in at least one place, which violates our assumption.

Now imagine each row in the table is produced by a TM. Now the $$i$$th digit of the diagonal number $$x$$ is the $$i$$th digit that TM $$M_{i}$$ outputs added to $$1$$ modulo $$10$$. 

But this seems to give us a way to compute an incomputable number! The catch is that we cannot be sure that M_{i}
  doesn't get into an infinite loop before printing out digit $$i$$.

Define the complement of a language as follows: given a language $$L\subseteq\Sigma^{*}$$, its complement is defined as $$L^{C}=\Sigma^{*}\backslash L$$.

Fact: if $$L$$ and $$L^{C}$$ are both recognizable, then $$L$$ is decidable.

Proof: If $$M_{1}$$ recognizes $$L$$ and $$M_{2}$$ recognizes $$L^{C}$$, run $$M_{1}$$ and $$M_{2}$$ in parallel on input $$x$$; if $$M_{1}$$ accepts, then accept, and if $$M_{2}$$ accepts, than reject. $$\blacksquare$$

Fact: There is a TM $$U$$ such that for every TM $$M$$ and input $$x$$:

- if $$M$$ accepts $$x$$, then $$U$$ accepts $$(\langle M\rangle,x)$$,
- if $$M$$ rejects $$x$$, then $$U$$ rejects $$(\langle M\rangle,x)$$, and 
- if $$M$$ does not halt on $$x$$, then $$U$$ does not halt on $$(\langle M\rangle,x)$$.

Essentially, this shows the existence of a TM that can “simulate” any other TM. An immediate corollary of this is that the language of the halting problem, $$H$$, is recognizable (just plug the input into the simulator and see if it halts by routing the former reject state to an accept state).

This implies that $$H^{C}$$ is not recognizable, since if we had a recognizer for $$H^{C}$$, we could combine it with the one for $$H$$ and come up with a decider for $$H$$, which we know is impossible.

Let $$AH$$ be defined as the set of machine representations $$\langle M\rangle$$ such that for every input $$x$$, $$M$$ halts on input $$x$$.

We will first prove that $$AH$$ is not decidable (easy), then prove that it's not recognizable.

**Undecidability:** Suppose for contradiction that $$AH$$ is decidable, and $$M_{AH}$$ decides $$AH$$. We will use this to make an algorithm for the halting problem as follows:

{% highlight text %}
Input: <M>, x
Define M_y as a machine with input y that simulates M on input x
Accept if and only if M_AH accepts <M_y>
{% endhighlight %}

which implies that if $$H$$ is decidable. This contradiction proves that $$AH$$ cannot be decidable.

**Unrecognizability:** Suppose for contradiction $$AH$$ is recognizable. We wish to show that there exists a recognizer for $$H^{C}$$. We can construct one as follows: given inputs $$\langle M\rangle$$ and $$y$$, first define a machine $$M_{y}$$ as this machine:

{% highlight text %}
Input x
Simulate M on input y for t = int(x) steps
If M_y halts within t steps:
    infinite loop
else:
    halt
{% endhighlight %}

Now run $$M_{AH}$$ on input $$\langle M_{y}\rangle$$ and accept if $$M_{AH}$$ accepts.

Now note that we give $$\langle M\rangle$$ and $$y$$ such that $$(\langle M\rangle,y)\in H^{C}$$. Since $$M$$ loops on input $$y$$; therefore, for all $$x$$, that $$M_{y}(x)$$ halts, which implies that $$\langle M_{y}\rangle\in AH$$ so $$M_{AH}$$ accepts $$\langle M_{y}\rangle$$; therefore, we accept.

On the other hand, if we feed it $$\langle M\rangle,y\notin H^{C}$$, notice that $$M$$ halts on input $$x$$ in $$t$$ steps. Let $$b$$ be the binary representation of $$t$$. Then $$M_{y}(b)$$ does not halt, which means that $$\langle M_{y}\rangle\notin AH$$, so $$M_{AH}$$ does not accept $$\langle M_{y}\rangle$$; therefore, we do not accept.

Therefore, this is a valid recognizer for $$H^{C}$$; this contradiction implies that $$AH$$ is unrecognizable.
