```yaml
number: 2347
title: "Implement `flake8-raise`"
type: issue
state: closed
author: ngnpope
labels:
  - plugin
assignees: []
created_at: 2023-01-30T11:29:21Z
updated_at: 2023-01-31T23:09:41Z
url: https://github.com/astral-sh/ruff/issues/2347
synced_at: 2026-01-12T15:54:42Z
```

# Implement `flake8-raise`

---

_@ngnpope_

[GitHub](https://github.com/jdufresne/flake8-raise), [PyPI](https://pypi.org/project/flake8-raise/).


- [x] [`R100`](https://github.com/jdufresne/flake8-raise#r100-raise-in-except-handler-without-from): raise in except handler without from
  - Already implemented via `B904` from [`flake8-bugbear`](https://github.com/charliermarsh/ruff#flake8-bugbear-b)
  - Already implemented via `TRY200` from [`tryceratops`](https://github.com/charliermarsh/ruff#tryceratops-try)
    - _This is buggy and only triggers with `raise Exception()` and not `raise Exception` (no parentheses)_
  - _Maybe implementation for one of these should be scrapped with the error code aliased to the other?_ :thinking:
- [x] [`R101`](https://github.com/jdufresne/flake8-raise#r101-use-bare-raise-in-except-handler): use bare raise in except handler
  - Already implemented via `TRY201` from [`tryceratops`](https://github.com/charliermarsh/ruff#tryceratops-try)
- [ ] [`R102`](https://github.com/jdufresne/flake8-raise#r102-unnecessary-parentheses-on-raised-exception): unnecessary parentheses on raised exception

So this plugin is mostly implemented. `R102` looks like it should be a relatively easy rule to define.

---

_Label `plugin` added by @charliermarsh on 2023-01-30 12:00_

---

_Comment by @charliermarsh on 2023-01-30 15:10_

Just confirming that in addition to adding R102, we should:

1. Remove `B904`, and mark it as an alias of `TRY200`.
2. Fix `TRY200` to flag `raise Exception` (without parentheses).

---

_Comment by @ngnpope on 2023-01-30 15:52_

Yeah, sounds good. I guess it makes sense to keep `TRY200` over `B904` as `tryceratops` is exception-focused and `flake8-bugbear` is a random collection of stuff.

---

_Closed by @charliermarsh on 2023-01-31 23:09_

---
