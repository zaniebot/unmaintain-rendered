```yaml
number: 2194
title: "Make `--help` shorter"
type: issue
state: closed
author: konsumlamm
labels:
  - wontfix
assignees: []
created_at: 2022-04-24T11:10:21Z
updated_at: 2022-04-24T12:31:43Z
url: https://github.com/BurntSushi/ripgrep/issues/2194
synced_at: 2026-01-12T16:13:24Z
```

# Make `--help` shorter

---

_@konsumlamm_

`rg --help` currently prints 1145 lines. This is way too long imo, it often takes me a long time to find the flag I'm searching for. For example, `grep --help` by comparison only prints 93 lines. I suggest changing `rg --help` to only print the most common options and introduce a `--fullhelp` flag to print what the `--help` flag currently prints.

---

_Comment by @BurntSushi on 2022-04-24 11:51_

`rg -h` shows a more condensed output.

---

_Closed by @BurntSushi on 2022-04-24 11:51_

---

_Label `wontfix` added by @BurntSushi on 2022-04-24 11:58_

---

_Comment by @konsumlamm on 2022-04-24 12:31_

Ah, thanks, that's good to know. I simply assumed that `-h` and `--help` do the same. Now I also see that the help message actually mentions this:

> Use -h for short descriptions and --help for more details.

---
