```yaml
number: 2524
title: "Update --debug to state if it's using stdin or the file system"
type: issue
state: closed
author: JohnC32
labels:
  - enhancement
  - rollup
assignees: []
created_at: 2023-06-01T17:20:50Z
updated_at: 2023-11-25T20:03:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2524
synced_at: 2026-01-12T16:13:24Z
```

# Update --debug to state if it's using stdin or the file system

---

_@JohnC32_

Please update --debug switch to indicate if ripgrep is searching stdin or a file system directory (and print which one).

Rationale:

I used ripgrep from within a Makefile and in a terminal, it worked fine. However, when I used it from within Emacs via `M-x compile RET make RET` ripgrep returned no results. Reading the help, led me to add --debug, but it didn't say why ripgrep wasn't working as I expected. 

After a bit of debugging I found that ripgrep reads stdin (emacs sets this up) which caused ripgrep to behave differently when running GNU make from within Emacs. I later saw in the doc that this is intended behavior, so no issue with that. Having --debug explain what is going on would have saved me a bit of debugging.

Thanks

---

_Label `enhancement` added by @BurntSushi on 2023-11-22 00:56_

---

_Label `rollup` added by @BurntSushi on 2023-11-22 01:07_

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---
