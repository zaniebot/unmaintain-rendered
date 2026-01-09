---
number: 16865
title: "`tool update-shell` uses incorrect logic when `ZDOTDIR` is set by `~/.zshenv`"
type: issue
state: closed
author: benberryallwood
labels:
  - bug
assignees: []
created_at: 2025-11-26T18:29:28Z
updated_at: 2025-11-29T22:13:06Z
url: https://github.com/astral-sh/uv/issues/16865
synced_at: 2026-01-07T13:12:19-06:00
---

# `tool update-shell` uses incorrect logic when `ZDOTDIR` is set by `~/.zshenv`

---

_Issue opened by @benberryallwood on 2025-11-26 18:29_

### Summary

The logic in `uv_shell::Shell.configuration_files` is incorrect for Zsh in the case when `ZDOTDIR` is set, but there is an existing `~/.zshenv` file. `uv tool update-shell` will create a new file at `ZDOTDIR/.zshenv` whereas it should update the existing `~/.zshenv` file.

A comment links to [rustup's rc file detection](https://github.com/rust-lang/rustup/blob/fede22fea7b160868cece632bd213e6d72f8912f/src/cli/self_update/shell.rs#L197) which handles this case correctly.

---

Minimal reproducible example (assuming a system with no existing Zsh config):

```zsh
echo "export ZDOTDIR=~/.config/zsh" > ~/.zshenv
source ~/.zshenv
mkdir -p $ZDOTDIR
touch "$ZDOTDIR/.zshrc"
uv tool update-shell
```

This creates a file at `~/.config/zsh/.zshenv` which will never be sourced because `ZDOTDIR` is set while sourcing `.zshenv`, not before it.

### Platform

Darwin 25.1.0 x86_64

### Version

uv 0.9.13 (7ca92dcf6 2025-11-26)

### Python version

Python 3.14.0

---

_Label `bug` added by @benberryallwood on 2025-11-26 18:29_

---

_Comment by @benberryallwood on 2025-11-26 18:30_

I'd like to raise a PR with the fix if people agree with the change

---

_Comment by @LIghtJUNction on 2025-11-26 19:15_

> I'd like to raise a PR with the fix if people agree with the change

I think it's fine.



---

_Referenced in [astral-sh/uv#16866](../../astral-sh/uv/pulls/16866.md) on 2025-11-26 19:41_

---

_Closed by @zsol on 2025-11-29 22:13_

---
