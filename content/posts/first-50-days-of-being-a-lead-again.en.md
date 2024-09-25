---
title: My first 50 days of being the lead software engineer (again)
date: 2023-09-21 06:56:50+00:00
draft: false
tags: ["Joyz, Inc.", "Software"]
---
Recent departures at our company led to a lack of technical leadership, especially in the infra/backend.

We had two choices. One is to scramble to hire a tech lead. Two is for me to assume the role temporarily while simultaneously functioning as the CEO.

We went for the second option.

This may seem like a red flag to some people. It is not ideal. Equally, I wasn't ready to throw someone into a job where they faced technical obstacles while understanding the users, the team, and the codebase. I was not confident enough to complete the hire in a month.

I decided to take the leap and act as the lead engineer in our company for the first time in 6 years. Here's some of what came out of the first 50 days.

## Done

### (re)Enabled one-step build for the Django monolith

We have a fully dockerised environment from local development to production. The problem was that the base Docker image used for packaging everything for deployment required a manual build from a local machine. This is troublesome in a few ways:

1. Host OS dependency. Docker isn't well-known for its ability to cross-build across the host OS. Not every developer has Ubuntu as their development machine, which can lead (and have led) to issues.
2. Coordination outside of git. You have non-trivial planning to do when multiple developers are working on changes that require dependency upgrades. Who is going to run the build, and when? After merging both branches or only one? This makes our Continuous Integration less continuous.

I'm glad to have made contributions in this area. This will help us stay closer to our aspiration to ship valuable code often.

### Fixing broken development branch

A major feature addition to our product was in the development branch + test environment. After the engineers behind the update have left, we've found:

1. The test cases were not thoroughly written. There were type mismatches and wrong dynamic import. You can easily spot these by writing tests that run the relevant lines of code.
2. Missing migration files. This meant that despite being released to an environment with a relational database, there was a mismatch between the code and the schema.

We did not know the extent of the malfunction at the beginning. Fast forward several weeks, and we now have the fixes, additional tests and type consistency across the changes.

Many of the errors could easily be caught with type annotations and `mypy`. We will fully embrace typed Python as I'll discuss later.

### Adding lcov file support + in-line coverage display

Ease of access matters. It's harder to convince yourself to achieve good code coverage in your tests if you have to open coverage results and visually compare them against your codebase. That's why we must lower friction. And the best way to reduce friction is to bring everything in context.

There's a wonderful extension in VS Code called Coverage Gutters. With a small change in the build/CI pipeline to export coverage into a lcov file, you can see the latest code coverage right in your editor. I've enabled this for our frontend repositories as well as the backend.

## Doing

### Another major feature update

There will be another feature addition in our study report pipeline that will please English teachers nationwide, powered by artificial intelligence.

### General code refactoring

We want a less mutual coupling between Django apps to enable separation of concern. Such a codebase is easier to collaborate on. The codebase is pretty extensively tested, making refactoring much more manageable than otherwise. Which is great news.

My strategy in all things refactoring is:

1. Isolate - break down larger files into smaller ones
2. Move tests to appropriate subdirectories in a hierarchical namespace
3. Move code
4. Consolidate code - put similar functions into the same module
5. Enforce DRY - Don't Repeat Yourself - principle

Many developers start with steps 4 or 5 before completing 1 to 3. I do not recommend that approach. Doing more than one of these steps simultaneously introduces permutations, and the complexity will grow exponentially. Don't trust your IQ, especially if your project is large. Make it linear. Take it slow.

It will likely take several rounds of isolate-consolidate-DRY cycles for a larger project to reach a good state.

We are still in the process of 1 to 3 for our own code, doing 4 and 5 when we feel safe. It's still early days! However, we already see its fruits when developing new features.

### Gradual typing

I'm a firm believer in typed Python.

Dynamic, classical Pythonic Python is great when it's used in the right place. Django ORM is a great example of what a dynamic programming language can do.

However, unless your code is explicitly designed to support unknown use cases, your code is better off being typed. Meaning: Your code should be statically typed in 99% of commercial programming.

By using typed Python, you will avoid nasty patterns such as throwing data around in undefined nested dictionaries. Also, scaling dynamic programming requires every contributor to the project to be adept at sensibly naming classes, functions and variables. I'm unsure whether we can ever make such an assumption, especially considering the rising diversity in technology and non-English speaking developers.

I looked at the `mypy.ini` in the project, and it was last edited 6 years ago - by - myself. We will need to take this one line at a time.

### Streamlining static file management

This topic will be worth an article on its own. My goals:

1. Have a uniform API for all CRUD operations for static files, local or cloud, version-controlled or user-generated.
2. Enable gradual end-to-end testing for both local and CI. A complete set of CRUD tests will be performed against the cloud in daily and release builds.

This was partially implemented but not enforced across the project. Eliminating low-level S3 calls everywhere but the shared utility will be the sensible first step.

### Move to dependabot

We first need to push the whole dependency to the latest-ish state. We are getting closer, but not quite there yet.

### Faster tests

This is another area where developer productivity could be boosted significantly. Slow tests impact local tests and the CI, causing an arbitrary wait in the process. It can break the flow state.

I am yet to run a full profiling of the tests, but my rough plans are:

1. Fix corner cases that fail only when setting the `--parallel` option
2. Spot and fix any I/O bound tests
3. Other CPU/RAM oriented optimisations - better cache, extending the minimum viable TestCase class, test fixture.

Python 3.12 and Django 5.0 will introduce test profiling, so upgrading to these would be a good place to get started.
