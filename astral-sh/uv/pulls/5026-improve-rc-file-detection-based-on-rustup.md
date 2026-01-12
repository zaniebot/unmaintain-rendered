```yaml
number: 5026
title: Improve rc file detection based on rustup
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/rustup
created_at: 2024-07-12T22:21:34Z
updated_at: 2024-07-12T22:40:32Z
url: https://github.com/astral-sh/uv/pull/5026
synced_at: 2026-01-12T16:06:35Z
```

# Improve rc file detection based on rustup

---

_@charliermarsh_

## Summary

Brings in some learnings from `rustup`: https://github.com/rust-lang/rustup/blob/fede22fea7b160868cece632bd213e6d72f8912f/src/cli/self_update/shell.rs#L197.

For example: we only need to write to `.zshenv` (but we have to respect `ZDOTDIR`). Additionally, for Fish, we need to respect `XDG_CONFIG_HOME`


---

_Label `bug` added by @charliermarsh on 2024-07-12 22:27_

---

_Label `preview` added by @charliermarsh on 2024-07-12 22:27_

---

_Merged by @charliermarsh on 2024-07-12 22:40_

---

_Closed by @charliermarsh on 2024-07-12 22:40_

---

_Branch deleted on 2024-07-12 22:40_

---
