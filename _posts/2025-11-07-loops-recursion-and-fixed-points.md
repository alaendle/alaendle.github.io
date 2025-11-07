---
layout: post
title: "Loops, Recursion, and Fixed Points"
subtitle: "How abstractions change the way we think about iteration"
tags: [Haskell, Functional Programming, Recursion, Theory]
date: 2025-11-07
---

*I already wrote this some time ago - but to get the blog started - and so that I don't forget.*

I stumbled upon this concept yesterday evening and wanted to share it with you, as it beautifully describes the evolution of my perception of loops. It's also a great example of how difficult it is to look back and truly understand how we ever managed to write code before learning certain abstractions.

## The Evolution of Loop Abstractions

I can vividly remember the introduction of `GOSUB` in BASIC—suddenly, you couldn't tell where the code would jump after executing a subroutine (since it depended on the call site). It seemed like pure magic, or devilry! Similarly, in Turbo Pascal, `do` and `while` loops were common, but a `for` loop? Far too abstract!

I never really struggled with `for-each` — this kind of traversal felt natural through iterators. But its counterpart, calling coroutines using `yield`, was pure magic again.

What I'm getting at is this: **each abstraction ultimately makes it easier to formulate code**, because you can focus on the business logic rather than worrying about how to squeeze your idea into source code.

## Loops, Recursion, and Fixed Points

Still not convinced? Let me put it this way: **every loop can be replaced by recursion (and vice versa)** — and recursion is nothing more than a **fixed point** (remember from 7th grade math? `f(x) = x`) of a higher-order function.

### Classic Example: Factorial

Let's start with the recursive factorial function:

{% highlight haskell %}
factorial n = if n == 0 then 1 else n * factorial (n - 1)
{% endhighlight %}

So far, so good. Now let's form a higher-order function by extracting the recursive call:

{% highlight haskell %}
mkFactorial f n = if n == 0 then 1 else n * f (n - 1)
{% endhighlight %}

With this, we've found the fixed point, because now we have:

{% highlight haskell %}
factorial === mkFactorial factorial
{% endhighlight %}

### The Mind-Bending Step

And now comes the mind-bending part—which, honestly, I haven't fully internalized yet.

When a function converges, you approach the fixed point by repeatedly substituting recursively: `x = f(f(f(f(...f(x)))))`

This means:

{% highlight haskell %}
fix f = f (fix f)
{% endhighlight %}

Therefore:

{% highlight haskell %}
factorial === fix mkFactorial
{% endhighlight %}

## Everything Clear?

This elegant formulation shows that iteration, recursion, and fixed points are all different perspectives on the same fundamental concept. Each abstraction layer we climb makes it easier to express our intent without getting bogged down in the mechanics of how the iteration actually happens.

The fixed point combinator `fix` captures the essence of recursion in its purest form — a beautiful example of how mathematical thinking can illuminate programming concepts.
