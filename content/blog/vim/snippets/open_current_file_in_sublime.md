---
title: "Open Current File in Sublime Text from Vim"
date: 2019-09-05
---

Sometimes, I just want to open the current file in Sublime Text for simplicity sake.

Usually, this is due to some automated commands I have in Vim, but not Sublime Text - like
[black](https://github.com/python/black) which I might not want to run on certain legacy files.

This is a simple command, but one that I often forget and don't see directly
documented elsewhere online. So, here it is:

```
:!subl %:p
```

---

**Breakdown:**

`!` - Run the following as a standard shell expression. See `:h !` for more.

`subl` - An alias to open Sublime Text through its [command line tool](https://www.sublimetext.com/docs/3/osx_command_line.html). 

`%:p` - Full path to the current file. [Thanks Vi StackExchange](https://vi.stackexchange.com/a/1885/17569)
