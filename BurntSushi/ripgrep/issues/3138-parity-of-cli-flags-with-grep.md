```yaml
number: 3138
title: parity of cli flags with grep
type: issue
state: closed
author: nic-nvidia
labels:
  - wontfix
assignees: []
created_at: 2025-09-03T17:55:29Z
updated_at: 2025-09-03T18:06:23Z
url: https://github.com/BurntSushi/ripgrep/issues/3138
synced_at: 2026-01-12T16:13:25Z
```

# parity of cli flags with grep

---

_@nic-nvidia_

I use `alias grep=rg` in my bashrc and I want it to just work. Lately I've started using AI for a lot of OS operations, and it always wants to use the -E flag like `find some_dir | grep -E "\\.R$"`. Seems this flag is -e in rg rather than -E.

It would be nice if there was compatibility, maybe alias -E to -e for instance?

---

_Comment by @BurntSushi on 2025-09-03 17:59_

https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#posix4ever

Moreover, `-E` in grep is not equivalent to `-e` in ripgrep. Ironically, `-e` in ripgrep is equivalent to... `-e` in grep. `-E` means something different in grep and isn't applicable to ripgrep. Additionally, `-E` in ripgrep is already used in ripgrep for something else (specifying an encoding).

Doubly moreover, there are loads of differences between grep and ripgrep. While the intersection between them is certainly bigger than the symmetric difference, ripgrep cannot reasonably be expected to be used as a tool with a grep compatible interface. Or, indeed, an interface compatible with _any_ other tool. So even if I did what you asked here, it wouldn't be enough to accomplish the stated goal.

You should probably use a smarter AI.

---

_Closed by @BurntSushi on 2025-09-03 17:59_

---

_Label `wontfix` added by @BurntSushi on 2025-09-03 17:59_

---

_Comment by @nic-nvidia on 2025-09-03 18:01_

Alright, thanks for the explanation

---
