```yaml
number: 21465
title: "panic: `internal error: entered unreachable code: IPython escape command expression is only allowed for % and !`"
type: issue
state: closed
author: correctmost
labels:
  - bug
  - parser
assignees: []
created_at: 2025-11-14T23:45:47Z
updated_at: 2025-11-24T05:40:28Z
url: https://github.com/astral-sh/ruff/issues/21465
synced_at: 2026-01-12T15:54:57Z
```

# panic: `internal error: entered unreachable code: IPython escape command expression is only allowed for % and !`

---

_@correctmost_

### Summary

This panic was originally reported without a reproducer at https://github.com/facebook/pyrefly/issues/1559 .  I was able to track it down with some targeted fuzzing:

```
panic: Panicked at crates/ruff_python_parser/src/parser/expression.rs:2782:13 when checking `/home/user/crash.ipynb`: `internal error: entered unreachable code: IPython escape command expression is only allowed for % and !`
--> crash.ipynb:1:1
```

**crash.ipynb**
```ipython
{
  "cells": [
    {
      "cell_type": "code",
      "metadata": {},
      "outputs": [],
      "source": [
        "with a,?b\n?"
      ]
    }
  ],
  "metadata": {},
  "nbformat": 4,
  "nbformat_minor": 2
}
```

### Version

ruff 0.14.5

---

_Comment by @ntBre on 2025-11-15 01:22_

Thank you!

---

_Label `bug` added by @ntBre on 2025-11-15 01:22_

---

_Label `parser` added by @ntBre on 2025-11-15 01:22_

---

_Closed by @dhruvmanila on 2025-11-24 05:40_

---
