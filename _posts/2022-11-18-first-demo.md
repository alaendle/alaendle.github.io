---
layout: post
title: "Demo blog post"
subtitle: "Hello from here"
tags: [Haskell]
date: 2022-12-20
---

Even if this is a somehow artificial problem - what is the *best* way to sum different return types?

Take a look at this example:

{% highlight haskell %}
simpleDivA :: Int -> Int -> Either String Int
simpleDivA a b = if b == 0 then Left "by zero" else Right $ a `div` b

simpleDivB :: Int -> Int -> Either Int Int
simpleDivB a b = if b == 0 then Left 0 else Right $ a `div` b

simple :: Either (String|Int) Int  -- <-- pseudocode
simple = do
    x <- simpleDivA 0 10
    y <- simpleDivB 10 2
    pure $ x + y
{% endhighlight %}

The simplest solution to this might be to define and use a sum-type.

{% highlight haskell %}
simpleDivA :: Int -> Int -> Either String Int
simpleDivA a b = if b == 0 then Left "by zero" else Right $ a `div` b

simpleDivB :: Int -> Int -> Either Int Int
simpleDivB a b = if b == 0 then Left 0 else Right $ a `div` b

data MyError = S String | I Int deriving Show

simple :: Either MyError Int
simple = do
    x <- left S $ simpleDivA 0 10
    y <- left I $ simpleDivB 10 2
    pure $ x + y
{% endhighlight %}

But this coupling and redundancy just feels wrong, so give [vinyl](https://hackage.haskell.org/package/vinyl-0.14.3) a try:

...