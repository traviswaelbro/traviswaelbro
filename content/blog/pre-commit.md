---
title: "Using pre-commit to Automate Git Hooks"
date: 2018-10-18
---

_**Note**: This heavily references the pre-commit documentation, but it is by no means a
comprehensive overview of everything that pre-commit can do. Check out
[the pre-commit website](https://pre-commit.com) to find out more for yourself._

## What is pre-commit?

> pre-commit is a framework for managing and maintaining multi-language pre-commit hooks.

Basically, [pre-commit](https://pre-commit.com) is a Python package that
provides a maintainable way to manage your git hooks. Git hooks allow you to run
_some code (or commands)_ on certain git events, such as before and after
commits, before and after pushes, and more.

_**Note:** pre-commit uses the term "hooks" to mean "run the piece of code that matches
the hook's `id`". In all cases below, I will be using "hooks" to mean the same._

Pre-commit itself supports just a few git hooks at this time, but it is a very
powerful tool that can automate a lot of your validation, linting, and more to
keep your codebase in check before things even hit the git server.

The pre-commit package has [many pre-defined hooks](https://pre-commit.com/hooks.html)
that you can add to your repository to have it take care of the grunt work for
you.

## How do I use it?

### Installation

Installation instructions can be found on [the pre-commit website](https://pre-commit.com/#install)
but it's really just as simple as running:

```
pip install pre-commit
```

### Project Configuration

Once pre-commit is installed, you will need to [add pre-commit plugins to your project](https://pre-commit.com/#plugins).
To get started, create a file called `.pre-commit-config.yaml` in the root of
your project. This file is used to specify which repositories and hooks will be
run against your project.

The basic layout will end up looking something like:

```
repos:
-   repo: <repository_url>
    rev: <branch, revision, or tag to clone>
    hooks:
    -  id: hook1
    -  id: hook2
    -  id: hook3
-   repo: <repository2_url>
    rev: <revision or tag to clone>
    hooks:
    -  id: hook1
```

> See all available hook mappings in [pre-commit's documentation](https://pre-commit.com/#pre-commit-configyaml---hooks)

For a simple example, here is one from the pre-commit documentation:

```
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v1.2.3
    hooks:
    -   id: trailing-whitespace
```

For a more detailed example, here is one from [pre-commit's demo repository](https://github.com/pre-commit/demo-repo/blob/master/.pre-commit-config.yaml):

```
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v1.2.3
    hooks:
    -   id: flake8
    -   id: end-of-file-fixer
    -   id: trailing-whitespace
    -   id: name-tests-test
-   repo: https://github.com/pre-commit/mirrors-scss-lint
    rev: v0.57.0
    hooks:
    -   id: scss-lint
-   repo: https://github.com/pre-commit/mirrors-ruby-lint
    rev: v2.3.1
    hooks:
    -   id: ruby-lint
-   repo: https://github.com/pre-commit/mirrors-jshint
    rev: v2.9.5
    hooks:
    - id: jshint
```

#### Confining hooks to run at certain stages

From [the pre-commit documentation](https://pre-commit.com/#confining-hooks-to-run-at-certain-stages):

> Hooks can however be confined to a stage by setting the stages property in
> your .pre-commit-config.yaml. The stages property is an array and can contain
> any of commit, push, and commit-msg.
> 
> _new in 1.8.0_ An additional manual stage is available for one off execution
> that won't run in any hook context. This special stage is useful for taking
> advantage of pre-commit's cross-platform / cross-language package management
> without running it on every commit. Hooks confied to stages: [manual] can be
> executed by running pre-commit run --hook-stage manual <hookid>.

### Usage

Once your `.pre-commit-config.yaml` file is set up the way you like, you need to
tell pre-commit to install itself into your project's git hooks (`.git/hooks/`).

```
pre-commit install
```

Now, pre-commit will run the appropriate hooks at the appropriate times!

> Every time you clone a project using pre-commit running pre-commit install
> should always be the first thing you do.

You can also manually run hooks in several ways.

Run all of the repository's hooks

```
pre-commit run --all-files
```

Run an individual hook
```
pre-commit run <hooks-id>
pre-commit run docformatter
```

#### Passing Arguments

You can [pass arguments to your hooks](https://pre-commit.com/#passing-arguments-to-hooks)
by including an `args` property in your `hooks` call, such as:

```
-  repo: https://github.com/pre-commit/pre-commit-hooks
   rev: v1.2.3
   hooks:
   -  id: my-hook-script-id
      args: [--myarg1=1, --myarg1=2]
```

When you next run pre-commit, your script will be called:

```
path/to/script-or-system-exe --myarg1=1 --myarg1=2 dir/file1 dir/file2 file3
```

If the args property is empty or not defined, your script will be called:

```
path/to/script-or-system-exe dir/file1 dir/file2 file3
```

Here's an intermediate example of a commit-only hook that will reject an
attempted commit if it doesn't pass `docformatter`:

```
repos:
-   repo: https://github.com/myint/docformatter
    rev: master
    hooks:
    - id: docformatter
      stages: [commit]
      args: [-i, --wrap-summaries=80, --wrap-descriptions=80, --pre-summary-newline, --make-summary-multi-line]
```

## Wrapping Up

Pre-commit is extremely flexible and relatively easy to set up on your project.
It has a small learning curve, but if you can manage to get through it, then you
can automatically enforce a huge amount of rules on your codebase. Use it for
everything from linting, auto-formatting, validation, and just about anything
else you might want to automate.

Need to set up something custom that isn't covered by [an existing pre-commit hook](https://pre-commit.com/hooks.html)?
Check out [pre-commit's demo repository](https://github.com/pre-commit/demo-repo/blob/master/.pre-commit-config.yaml)
for some examples that might be useful in creating your own hooks that
pre-commit can run to do something more unique for your needs.

<a href="https://twitter.com/intent/tweet?text=Using pre-commit to Automate Git Hooks&url=https%3A%2F%2Ftraviswaelbro.github.io%2Fblogs%2Fpre-commit%2F" target="_blank" class="twitter-button">Tweet</a>
