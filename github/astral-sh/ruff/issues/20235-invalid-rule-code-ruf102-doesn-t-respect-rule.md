---
number: 20235
title: "`invalid-rule-code` (`RUF102`) doesn't respect rule redirects"
type: issue
state: closed
author: ntBre
labels:
  - bug
  - preview
assignees: []
created_at: 2025-09-04T14:03:48Z
updated_at: 2025-09-10T21:27:08Z
url: https://github.com/astral-sh/ruff/issues/20235
synced_at: 2026-01-07T13:12:16-06:00
---

# `invalid-rule-code` (`RUF102`) doesn't respect rule redirects

---

_Issue opened by @ntBre on 2025-09-04 14:03_

### Summary

This came up in the [ecosystem check](https://github.com/astral-sh/ruff/pull/20232#issuecomment-3253818000) when trying to stabilize the rule for the 0.13 release. Using the `TC002` example with an added `TCH002` noqa (`TCH` has been remapped to `TC`):

```py
from __future__ import annotations

import pandas as pd  # noqa: TCH002


def func(df: pd.DataFrame) -> int:
    return len(df)
```

Selecting both `TC002` and `RUF102` shows that `TC002` is suppressed (the noqa code works and is not invalid), but `RUF102` still fires:

```shell
> ruff check try.py --select TC002 --select RUF102 --preview
RUF102 [*] Invalid rule code in `# noqa`: TCH002
 --> try.py:3:22
  |
1 | from __future__ import annotations
2 |
3 | import pandas as pd  # noqa: TCH002
  |                      ^^^^^^^^^^^^^^
  |
help: Remove the rule code

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Label `bug` added by @ntBre on 2025-09-04 14:04_

---

_Label `preview` added by @ntBre on 2025-09-04 14:04_

---

_Comment by @TaKO8Ki on 2025-09-04 17:23_

I will take this issue:)

---

_Referenced in [astral-sh/ruff#20245](../../astral-sh/ruff/pulls/20245.md) on 2025-09-04 18:00_

---

_Closed by @amyreese on 2025-09-10 21:27_

---
