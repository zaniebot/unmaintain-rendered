```yaml
number: 1906
title: Problem with ripgrep 13.0.0 and Neovim
type: issue
state: closed
author: oncomouse
labels:
  - duplicate
assignees: []
created_at: 2021-06-20T15:08:12Z
updated_at: 2021-06-20T19:11:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1906
synced_at: 2026-01-12T16:13:24Z
```

# Problem with ripgrep 13.0.0 and Neovim

---

_@oncomouse_

#### What version of ripgrep are you using?

~~~
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
~~~

#### How did you install ripgrep?

Installed on Arch systems with `pacman` and on macOS systems using `brew`.

#### What operating system are you using ripgrep on?

macOS and Arch Linux, both current versions.

#### Describe your bug.

I'm using ripgrep with `jobstart()` in Neovim to search in files and populate the quickfix list. Starting with version 13.0.0, running ripgrep with `jobstart()` produces no results.

I have reinstalled ripgrep 12.1.1 and `jobstart()` again works.

This problem occurs in both Neovim 0.5 and 0.44.

#### What are the steps to reproduce the behavior?

The easiest way to reproduce is to run the following in Neovim:

```
:call jobstart('rg --vimgrep text', {'on_stdout':{j,d,e->append(line('.'),d)}})
```

If it helps, my full ripgrep + Neovim setup is here: https://github.com/oncomouse/vim-grep

#### What is the actual behavior?

No output is appended to the current buffer.

#### What is the expected behavior?

A list of lines matching "text" should be appended to the current buffer.


---

_Comment by @BurntSushi on 2021-06-20 19:11_

Dupe of #1892.

---

_Closed by @BurntSushi on 2021-06-20 19:11_

---

_Label `duplicate` added by @BurntSushi on 2021-06-20 19:11_

---
