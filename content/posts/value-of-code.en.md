---
title: A Measure of Code's Value
date: 2024-09-27T11:55:55+09:00
tags: ["Software"]
---

As a software engineer and startup founder, I often ask: what makes software valuable?

Code has no intrinsic value - its worth is entirely contextual.
For instance, `x = a + b` isn’t inherently more valuable than `x = a * b`.

However, we can measure its extrinsic value: how often the software runs.
Or, put more formally:

**Number of times the software is run per unit time**

By this measure, Windows is far more valuable than macOS.
And the Linux kernel, which powers the majority of web servers and every single Android device, might be the most valuable of all.

This metric applies at any scale: operating systems, apps, plugins, functions, or even single lines of code.

Crucially, this measure has nothing to do with code quality - whether it’s well-written, extensible, or debuggable.
It’s also driven by business factors like scale, marketing, and support.

Based on this theory, there are a couple of derivations we can make.

**Refactoring usually increases value density.**
Think of (extrinsic value of software) / (lines of code).
While refactoring _can_ raise the number of lines of code in some instances, if you succeed in maximising code reuse under DRY principle, you usually see a decline in LOC.
If the software retains its functionality after refactoring and by extension its value, each line of code now carries a higher value than before.

We can also measure **economic efficacy: how well a business turns the value of software it owns into revenue**. Consider the Windows vs Linus case. Despite Linux’s wider usage, Windows dominates in revenue due to its business model.

When making decisions about software, we often find ourselves between customer value and developer experience.
The value of code bridges business needs and technical challenges, helping align customer value with developer experience.
