```yaml
number: 20601
title: "[Infinite loop] I002 and UP029"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-09-27T14:31:25Z
updated_at: 2025-09-30T21:11:36Z
url: https://github.com/astral-sh/ruff/issues/20601
synced_at: 2026-01-12T15:54:57Z
```

# [Infinite loop] I002 and UP029

---

_@dscorbett_

### Summary

Ruff fails to converge when [`missing-required-import` (I002)](https://docs.astral.sh/ruff/rules/missing-required-import/) and [`unnecessary-builtin-import` (UP029)](https://docs.astral.sh/ruff/rules/unnecessary-builtin-import/) are selected and an import from `builtins` is required. [Example](https://play.ruff.rs/ed7201ef-57e1-4cb9-aee0-62f82047dd89):
```console
$ echo 1 | ruff --isolated check - --select I002,UP029 --diff --config 'lint.isort.required-imports=["from builtins import str"]' --unsafe-fixes

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `-`, the rule codes I002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


Would fix 100 errors.
```

### Version

ruff 0.13.2 (b0bdf0334 2025-09-25)

---

_Label `bug` added by @ntBre on 2025-09-27 15:32_

---

_Label `fixes` added by @ntBre on 2025-09-27 15:32_

---

_Comment by @IDrokin117 on 2025-09-27 17:02_

I will take this one

---

_Assigned to @IDrokin117 by @ntBre on 2025-09-27 19:36_

---

_Closed by @ntBre on 2025-09-30 21:11_

---
