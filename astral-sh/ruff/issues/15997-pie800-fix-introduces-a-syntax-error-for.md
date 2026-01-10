```yaml
number: 15997
title: "PIE800 fix introduces a syntax error for `{**{},}`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-02-06T15:39:03Z
updated_at: 2025-02-07T07:52:11Z
url: https://github.com/astral-sh/ruff/issues/15997
synced_at: 2026-01-10T11:09:57Z
```

# PIE800 fix introduces a syntax error for `{**{},}`

---

_Issue opened by @dscorbett on 2025-02-06 15:39_

### Description

The fix for [`unnecessary-spread` (PIE800)](https://docs.astral.sh/ruff/rules/unnecessary-spread/) in Ruff 0.9.4 introduces a syntax error when unpacking an empty dictionary that appears before a comma.
```console
$ echo '{**{},}' | ruff --isolated check --select PIE800 - --diff

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `-`, the rule codes PIE800, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```

---

_Label `bug` added by @AlexWaygood on 2025-02-06 16:00_

---

_Label `fixes` added by @AlexWaygood on 2025-02-06 16:00_

---

_Closed by @MichaReiser on 2025-02-07 07:52_

---
