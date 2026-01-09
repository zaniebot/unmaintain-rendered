---
number: 16802
title: "[Infinite loop] I002 and UP035"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - isort
assignees: []
created_at: 2025-03-17T12:35:35Z
updated_at: 2025-07-31T19:17:28Z
url: https://github.com/astral-sh/ruff/issues/16802
synced_at: 2026-01-07T13:12:16-06:00
---

# [Infinite loop] I002 and UP035

---

_Issue opened by @dscorbett on 2025-03-17 12:35_

### Summary

Ruff fails to converge when [`missing-required-import` (I002)](https://docs.astral.sh/ruff/rules/missing-required-import/) and [`deprecated-import` (UP035)](https://docs.astral.sh/ruff/rules/deprecated-import/) are selected and a deprecated import is required.

```console
$ echo 1 | ruff --isolated check - --select I002,UP035 --config 'lint.isort.required-imports=["from collections import Sequence"]' --fix

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `-`, the rule codes I002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
from collections.abc import Sequence
1
-:1:1: I002 Missing required import: `from collections import Sequence`
Found 101 errors (100 fixed, 1 remaining).
[*] 1 fixable with the `--fix` option.
```

### Version

ruff 0.11.0 (2cd25ef64 2025-03-14)

---

_Label `bug` added by @ntBre on 2025-03-17 12:48_

---

_Label `isort` added by @AlexWaygood on 2025-03-17 12:50_

---

_Label `configuration` added by @AlexWaygood on 2025-03-17 12:50_

---

_Label `configuration` removed by @AlexWaygood on 2025-03-17 12:50_

---

_Comment by @ntBre on 2025-03-17 12:50_

I haven't looked closely enough to know if this is a good approach, but the fix could possibly be similar to the changes in https://github.com/astral-sh/ruff/pull/15480.

---

_Comment by @dylwil3 on 2025-03-17 13:50_

I think in this particular case another option could be to just not offer the _fix_ for `UP035` here, or even maybe ignore the rule altogether for required imports.

---

_Comment by @AlexWaygood on 2025-03-17 14:15_

> or even maybe ignore the rule altogether for required imports.

this makes sense to me. If we do this, we could even say in the docs for `UP035` that one way of configuring the rule (excluding certain deprecated-import upgrades) is to specify the `lint.isort.required-imports` setting.

---

_Referenced in [astral-sh/ruff#18729](../../astral-sh/ruff/issues/18729.md) on 2025-06-17 17:48_

---

_Referenced in [astral-sh/ruff#19413](../../astral-sh/ruff/pulls/19413.md) on 2025-07-28 18:46_

---

_Closed by @ntBre on 2025-07-31 19:17_

---

_Referenced in [astral-sh/ruff#21115](../../astral-sh/ruff/pulls/21115.md) on 2025-10-29 19:21_

---
