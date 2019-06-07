---
title: "Reposition Screen after Jumping to a Mark"
date: 2019-06-07
---

I've been using [Marks](https://vim.fandom.com/wiki/Using_marks) more often
lately to help me jump around files easily. The downside I've been finding is
that it's hard to tell where your cursor is after jumping to your mark (even
though I'm using a [highlighted cursor line](https://vim.fandom.com/wiki/Highlight_current_line)).

As often happens with tech-related nuisances, I'm not the first one to notice
this issue. This snippet comes courtesy of [u/princker](https://www.reddit.com/user/princker/)'s
comment on a Reddit post titled, [_How to center screen after moving to a mark_](https://www.reddit.com/r/vim/comments/80vutj/how_to_center_screen_after_moving_to_a_mark/).

This will remap the normal **go to mark** mapping (usually `'<letter>`, such as
`'a` for **go to mark a**) for each individual letter so that the **go to mark**
mapping now says **go to mark _then_ move the marked line into the center of the
window**.

I use this mapping, but I prefer using `zt` to **move ... to the top of the
window** since I usually mark function definitions and I want to easily see the
function's contents without having to scroll down.

## Jump to Mark and Move Marked Line to Center

```
nnoremap <expr> ' "'" . nr2char(getchar()) . "zz"
nnoremap <expr> ` "`" . nr2char(getchar()) . "zz"
```

## Jump to Mark and Move Marked Line to Top

```
nnoremap <expr> ' "'" . nr2char(getchar()) . "zt"
nnoremap <expr> ` "`" . nr2char(getchar()) . "zt"
```

## Jump to Mark and Move Marked Line to Bottom

```
nnoremap <expr> ' "'" . nr2char(getchar()) . "zb"
nnoremap <expr> ` "`" . nr2char(getchar()) . "zb"
```
