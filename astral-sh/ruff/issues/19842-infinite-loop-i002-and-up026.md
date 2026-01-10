---
number: 19842
title: "[Infinite loop] I002 and UP026"
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-08-09T13:35:47Z
updated_at: 2025-09-12T20:45:27Z
url: https://github.com/astral-sh/ruff/issues/19842
synced_at: 2026-01-10T01:23:00Z
---

# [Infinite loop] I002 and UP026

---

_Issue opened by @dscorbett on 2025-08-09 13:35_

### Summary

Ruff fails to converge when [`missing-required-import` (I002)](https://docs.astral.sh/ruff/rules/missing-required-import/) and [`deprecated-mock-import` (UP026)](https://docs.astral.sh/ruff/rules/deprecated-mock-import/) are selected and `import mock` is required. [Example](https://play.ruff.rs/5daddb7d-af26-4fa7-9004-09f500553946):
```console
$ echo 1 | ruff --isolated check - --select I002,UP026 --config 'lint.isort.required-imports=["import mock"]' --fix

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `-`, the rule codes I002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
from unittest import mock
1
-:1:1: I002 Missing required import: `import mock`
Found 101 errors (100 fixed, 1 remaining).
[*] 1 fixable with the `--fix` option.
```

### Version

ruff 0.12.8 (f51a228f0 2025-08-07)

---

_Label `bug` added by @dylwil3 on 2025-08-11 04:10_

---

_Comment by @ntBre on 2025-08-11 13:10_

Related: https://github.com/astral-sh/ruff/pull/19413

---

_Comment by @TaKO8Ki on 2025-09-08 10:17_

I will work on this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-09-08 13:45_

---

_Referenced in [astral-sh/ruff#20327](../../astral-sh/ruff/pulls/20327.md) on 2025-09-10 10:57_

---

_Closed by @dylwil3 on 2025-09-12 20:45_

---
