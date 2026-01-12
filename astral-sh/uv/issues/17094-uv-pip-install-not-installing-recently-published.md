```yaml
number: 17094
title: "`uv pip install` not installing recently published package"
type: issue
state: closed
author: fubuloubu
labels:
  - bug
assignees: []
created_at: 2025-12-12T00:41:11Z
updated_at: 2025-12-12T00:45:29Z
url: https://github.com/astral-sh/uv/issues/17094
synced_at: 2026-01-12T16:02:43Z
```

# `uv pip install` not installing recently published package

---

_@fubuloubu_

### Summary

I just published this package a few mins ago: https://pypi.org/project/eip712/0.3.0

I go to install it (in a completely fresh venv created with `uv venv` and activated):
```sh
$ uv pip install "eip712==0.3.0"
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of eip712==0.3.0 and you require eip712==0.3.0, we can conclude that your requirements are
      unsatisfiable.
```

Really just completely at a loss at why this is happening

### Platform

Fedora 43

### Version

0.9.17

### Python version

3.10.19

---

_Label `bug` added by @fubuloubu on 2025-12-12 00:41_

---

_Comment by @fubuloubu on 2025-12-12 00:45_

Alright, it must've been something with my terminal, I've been slowly rolling from pyenv over to uv and something got messed up. Now it works.

---

_Closed by @fubuloubu on 2025-12-12 00:45_

---
