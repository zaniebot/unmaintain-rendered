---
number: 20639
title: "D102: RUF100 ping-pong"
type: issue
state: closed
author: smurfix
labels:
  - question
assignees: []
created_at: 2025-09-29T21:28:58Z
updated_at: 2025-09-29T21:57:58Z
url: https://github.com/astral-sh/ruff/issues/20639
synced_at: 2026-01-10T01:23:01Z
---

# D102: RUF100 ping-pong

---

_Issue opened by @smurfix on 2025-09-29 21:28_

### Summary

This complains with RUF100. When I remove the comment, it instead complains with D102.

```
"Test."
from __future__ import annotations


class Foo:
    "Test."

    def test(self):  # noqa:D102
        return 1
```

---

_Comment by @ntBre on 2025-09-29 21:40_

I can't reproduce this one in the [playground](https://play.ruff.rs/b2916cf8-8a16-4141-bf34-31aa4a0df39d) with both D102 and RUF100 selected. Are you possibly selecting only RUF100? It takes into account the other selected rules to know if a comment is unused:

```
> ruff check --select RUF100 --output-format=concise
try.py:8:22: RUF100 [*] Unused `noqa` directive (non-enabled: `D102`)
Found 1 error.
[*] 1 fixable with the `--fix` option.
> ruff check --select RUF100,D102
All checks passed!
```

---

_Label `question` added by @ntBre on 2025-09-29 21:40_

---

_Comment by @smurfix on 2025-09-29 21:57_

Ugh. No, but related. My fault. Sorry about the noise.

---

_Closed by @smurfix on 2025-09-29 21:57_

---
