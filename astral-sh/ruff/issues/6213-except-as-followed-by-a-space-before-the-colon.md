```yaml
number: 6213
title: except-as followed by a space before the colon causes a debug assertion failure
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-07-31T22:37:04Z
updated_at: 2023-08-01T03:42:32Z
url: https://github.com/astral-sh/ruff/issues/6213
synced_at: 2026-01-10T11:09:48Z
```

# except-as followed by a space before the colon causes a debug assertion failure

---

_Issue opened by @addisoncrump on 2023-07-31 22:37_

Consider the following snippet:

```py
try:
  print("hello")
except A as e :
  print("oh no!")
```

Note the space following `e`.

This snippet causes ruff to fail a debug assertion:

```
~/git/ruff/target/debug/ruff --fix minimized-from-crash-1248f0f7231a99d2
warning: Detected debug build without --no-cache.
warning: Linting panicked minimized-from-crash-1248f0f7231a99d2: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'assertion failed: matches!(following.kind, SimpleTokenKind :: Colon)', crates/ruff/src/rules/pyflakes/fixes.rs:120:5
```

Fairly certain that this assertion is just not accounting for whitespace, which is a minor issue but may show up again in the future.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-01 03:31_

---

_Label `bug` added by @charliermarsh on 2023-08-01 03:31_

---

_Closed by @charliermarsh on 2023-08-01 03:42_

---
