---
title: "Project-Level Searching in Vim"
date: 2019-05-28
---

[Basic searching](https://vim.fandom.com/wiki/Searching) in Vim is mostly
straightforward once you spend a few minutes with it. It works quickly and
easily - unless you need to search outside of the current file. Then the
water gets a little muddier and there are a multitude of different
recommendations from every corner of the internet. I've found a good workflow
for managing my own project-level searches, but I still use a few different
methods.

For the vast majority of project-level searches, I'm happy using
[FZF](https://github.com/junegunn/fzf.vim) with CTAGS. I'm normally looking
for method definitions or usages, so a keymap running a simple `:Tags <C-R>`
(word under cursor) suffices.

> *Note: I've configured CTAGS to automatically generate via git hooks. A big
> thanks to Tim Pope, whose [2011 blog post](https://tbaggery.com/2011/08/08/effortless-ctags-with-git.html)
> details how to achieve this. You can also set up an
> [autocmd to update your CTAGS file](https://vim.fandom.com/wiki/Autocmd_to_update_ctags_file)
> if that's better for you.*

In the past, I've used [Ripgrep](https://github.com/BurntSushi/ripgrep) to
handle fuzzy grep searching the entire project, but that's become much less
efficient now that I'm working on projects with larger codebases. Since then,
I've been mostly just exiting Vim and using standard shell grep because I
initially had a lot of trouble getting project-level search working from
within Vim.

All that changed recently when I saw a hint in
[a tweet from Mastering Vim](https://twitter.com/MasteringVim/status/1132003298372988928)
about `vimgrep` (which I've used a very little bit in the past), after which
I found [Drew Neil's Vimcast](http://vimcasts.org/episodes/search-multiple-files-with-vimgrep/)
on the subject. Armed with a better understanding, I finally sat down to figure
out a better way, which has filled the missing gap for my workflow.

You can **see the details in the section below**, but now that I can use Vim's
internal grep on a project level, I'm an even happier camper than before. I'm
still using FZF to quickly search CTAGs, but for searching outside of CTAGS,
using `vimgrep` has worked with great success.

```
vimgrep <search> **/*.<filetype>
```

---

## Internal vs. External Grep

Before getting into details about how to use each of Vim's grep commands, let's
discuss their differences and why you might want to use one versus the other.

**Internal Grep (`:vimgrep`)**

The `:vimgrep` command is implemented inside Vim.

In my opinion, the main draw of `vimgrep` is that it uses Vim’s built-in regex
engine, so you can reuse the patterns that work with Vim’s standard search
command. 

This especially comes in handy when searching for more complex patterns, with
special characters. You can test the search within a file you know to have the
pattern, then reuse the pattern in the grep command. Conversely, command line
grep tools often have a different evaluation of special characters, so you might
have to modify the command if using external (command line) grep.

The downside of `vimgrep` is that it's sometimes noticeably slower then external
grep commands.

**External Grep (`:grep`)**

The `:grep` command is a call to a grep command implemented outside Vim.

By default, this just uses the standard shell grep, which means the benefit is
faster execution - sometimes significantly faster. For simple pattern searches,
you'll usually be fine to just use `grep`.

However, for complex search patterns (with special characters) and/or special
include/exclude directories, I prefer the syntax of the `vimgrep` command.

See some examples of each in the sections below.

---

## Internal Grep (`vimgrep`)

From Vim's help dialog:

```
:vim[grep][!] {pattern} {file} ...
:vim[grep][!] /{pattern}/[g][j] {file} ...
```

The `vimgrep` command populates the quickfix list with the results of your
search. You can traverse the results with a few commands which can be easily
mapped. If you're using [vim-unimpaired](https://github.com/tpope/vim-unimpaired),
then you can use the key mappings already built into that plugin.

| Ex command   | unimpaired map | effect                        |
|--------------|----------------|-------------------------------|
| :cprev[ious] | [q             | reverse through quickfix list |
| :cnext       | ]q             | advance through quickfix list |
| :cfirst      | [Q             | go to start of quickfix list  |
| :clast       | ]Q             | go to end of quickfix list    |

In most of my project level searches, I'll have matching results across many
different files. Normally this is a nuisance to jump between files, but not with
`vimgrep`. As you advance through the results, Vim will go through the results
in the first file, then it will jump to the next file with results.

![Example vimgrep GIF](https://d26aqo05ggejx9.cloudfront.net/vimgrep-string-filetype.gif)

### Examples:

#### Search the current file

```
:vimgrep my_string %
:vimgrep /my_string/ %
```

#### Resursive Search of the working directory

```
:vimgrep my_string **
:vimgrep /my_string/ **
```

#### Recursive Search of files of a certain type within the working directory

```
:vimgrep my_string **/*.py
:vimgrep /my_string/ **/*.py
```

---

## External Grep (`grep`)

External grep is Vim's way of simply calling the command line grep process in
a shell child process. Vim will just pass whatever you give it, take the
results back from the shell, and put it in the quickfix list.

From Vim's help dialog:

```
:gr[ep][!] [arguments]
```

### Examples:

#### Search the current file

```
:grep my_string %
```

#### Resursive Search of the working directory

```
:grep -rn my_string **
```

#### Recursive Search of files of a certain type within the working directory

```
:grep -rn my_string ** --include=*.py
```

---

## Additional Reading

* [Demystifying Multi-file Searches in Vim (and the command line)](https://seesparkbox.com/foundry/demystifying_multi_file_searches_in_vim_and_the_command_line)
  by [Ethan Muller](https://twitter.com/ethanmuller?lang=en)
* [How to grep in Vim](https://codeyarns.com/2017/09/15/how-to-grep-in-vim/) by [Ashwinn, of Code Yarns](https://codeyarns.com/author/ashwinn/)
