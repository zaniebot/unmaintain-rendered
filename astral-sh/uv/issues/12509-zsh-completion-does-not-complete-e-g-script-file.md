```yaml
number: 12509
title: zsh completion does not complete e.g. script file names
type: issue
state: closed
author: slhck
labels:
  - bug
assignees: []
created_at: 2025-03-27T09:55:05Z
updated_at: 2025-03-27T13:59:11Z
url: https://github.com/astral-sh/uv/issues/12509
synced_at: 2026-01-12T16:01:05Z
```

# zsh completion does not complete e.g. script file names

---

_@slhck_

### Summary

I want to have `uv` let me just complete script file names, e.g.:

```
uv run analyze-bloodwork.py
```

But it just does not complete when typing, e.g. `a` and then `<TAB>`:

![Image](https://github.com/user-attachments/assets/76b3b181-7f39-4c72-a85b-14d4c5e63913)

`uv run` itself autocompletes with its options:

<img width="1018" alt="Image" src="https://github.com/user-attachments/assets/5d21db53-65e0-46f9-8435-415250f6d6a6" />

I'm using the builtin completions from Homebrew, which are the same as when running `uv generate-shell-completion zsh`, with zsh 5.9.


### Platform

macOS, Darwin 23.6.0 arm64

### Version

uv 0.6.9 (Homebrew 2025-03-20)

### Python version

Python 3.11.7

---

_Label `bug` added by @slhck on 2025-03-27 09:55_

---

_Comment by @The-Compiler on 2025-03-27 13:58_

Duplicate of #8432?

---

_Closed by @charliermarsh on 2025-03-27 13:59_

---
