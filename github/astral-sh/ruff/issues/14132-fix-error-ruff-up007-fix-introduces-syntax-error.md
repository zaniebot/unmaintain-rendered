---
number: 14132
title: "[Fix error]: Ruff UP007 fix introduces syntax error with nested string type annotations"
type: issue
state: closed
author: Paillat-dev
labels:
  - bug
  - help wanted
  - great writeup
assignees: []
created_at: 2024-11-06T12:37:17Z
updated_at: 2025-01-27T18:41:05Z
url: https://github.com/astral-sh/ruff/issues/14132
synced_at: 2026-01-07T13:12:16-06:00
---

# [Fix error]: Ruff UP007 fix introduces syntax error with nested string type annotations

---

_Issue opened by @Paillat-dev on 2024-11-06 12:37_

When running Ruff's auto-fix for UP007 (non-pep604-annotation), it introduces a syntax error when handling nested quotes in type annotations. The specific case occurs when using string literals for forward references that contain quotes within quotes.

## Minimal Reproducible Example
```python
from typing import Union

class AClass:
    ...

def myfunc(param: "tuple[Union[int, 'AClass', None], str]"):
    print(param)
```

## Current Behavior
Running `ruff check . --fix` produces:
```
error: Fix introduced a syntax error. Reverting all changes.
This indicates a bug in Ruff.
```

The code itself is valid Python and works correctly with type checkers. The nested quote structure (`'AClass'` inside `"tuple[...]"`) is a legitimate pattern for forward references in type annotations, even tough the quotes are redundant.

## Expected Behavior
Ruff should either:
1. Correctly handle the nested quotes when applying UP007 fixes, or
2. Skip applying UP007 fixes for complex nested quote cases to avoid introducing syntax errors

## Environment
- Ruff version: 0.7.2
- Command: `ruff check . --fix`

## Configuration
```toml
[tool.ruff]
select = ["ALL"]
# (other settings omitted for brevity)
```

## Additional Context
This appears to be an edge case in quote handling for type annotations. While the syntax may look unusual, it's valid Python and is correctly interpreted by type checkers. The UP007 fixer might need to be more careful when handling nested quotes in type annotation strings.

---

_Comment by @MichaReiser on 2024-11-06 13:19_

Wow, nice write up! 

This is related to https://github.com/astral-sh/ruff/issues/13934

---

_Label `great writeup` added by @MichaReiser on 2024-11-06 13:19_

---

_Label `bug` added by @MichaReiser on 2024-11-06 13:19_

---

_Label `help wanted` added by @MichaReiser on 2024-11-06 13:19_

---

_Comment by @dhruvmanila on 2024-11-07 04:51_

I _think_ this is related to https://github.com/astral-sh/ruff/issues/10586 instead. When the checker is visiting the `Union[...]` which is inside a string annotation, the `Generator` is not considering the outer quote. The rule mentioned here (`UP007`) doesn't use the `quote_annotation` function.

---

_Referenced in [astral-sh/ruff#15726](../../astral-sh/ruff/pulls/15726.md) on 2025-01-24 22:54_

---

_Assigned to @ntBre by @ntBre on 2025-01-26 16:19_

---

_Closed by @ntBre on 2025-01-27 18:41_

---
