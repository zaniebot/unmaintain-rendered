---
number: 16345
title: C419 is marked as stable but only checked in preview mode
type: issue
state: closed
author: Slarag
labels:
  - question
assignees: []
created_at: 2025-02-24T12:28:28Z
updated_at: 2025-02-24T13:16:40Z
url: https://github.com/astral-sh/ruff/issues/16345
synced_at: 2026-01-07T13:12:16-06:00
---

# C419 is marked as stable but only checked in preview mode

---

_Issue opened by @Slarag on 2025-02-24 12:28_

### Description

Rule C419 is marked as stable in the docs, but silently ignored unless `preview` flag is set (in cli or `pyproject.toml`.)

No warning is shown if C419 is selected, but preview mode is not enabled.

- `ruff check test.py --select C,C4,C419`: 
   ```python
   All checks passed!
   ```
- `ruff check test.py --select C,C4,C419 --preview`: C419 error found
  ```python
  test.py:7:16: C419 Unnecessary list comprehension
     |
  5 |     """My test function."""
  6 |     y = [1, 2, 3, 4, 5]
  7 |     return min([abs(x) for x in y])
    |                ^^^^^^^^^^^^^^^^^^^ C419
    |
    = help: Remove unnecessary comprehension

  Found 1 error.
  No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).

  ```

test.py:

```python
"""Test module."""


def hello():
    """My test function."""
    y = [1, 2, 3, 4, 5]
    return min([abs(x) for x in y])

```

- Empty `pyproject.toml`
- Ruff version: 0.9.7
- Python version: 3.11.9



---

_Comment by @MichaReiser on 2025-02-24 12:31_

You're right, the rule is stable but we extended its scope in preview (only for now) to also cover `min`, `max` and `sum`. 

> Many builtin functions (this rule currently covers `any` and `all` in stable, along with `min`, `max`, and `sum` in [preview](https://docs.astral.sh/ruff/preview/)) accept any iterable, including a generator

That's why this behavior is expected. 

---

_Label `question` added by @MichaReiser on 2025-02-24 12:31_

---

_Comment by @Slarag on 2025-02-24 12:42_

Ah ok, thanks for the quick response! Should have read the docs more thoroughly... 

My original problem was the following: I want to enable checks for E302/E303.
Therefore I set `preview = true` in my `pyproject.toml`, but this changed the behavior of C419.

So I set `explicit-preview-rules = true` and `select = ["C", "E", "E303"]` explicitly.  But preview features of C419 are still enabled.
I would expect preview features of C419 not be be enabled in this case.

---

_Comment by @MichaReiser on 2025-02-24 12:43_

> Ah ok, thanks for the quick response! Should have read the docs more thoroughly...

Yeah, it's a bit hard to spot. 

> So I set explicit-preview-rules = true and select = ["C", "E", "E303"] explicitly. But preview features of C419 are still enabled.
I would expect preview features of C419 not be be enabled in this case.

We, unfortunately, don't support such granular preview selection today

---

_Comment by @Slarag on 2025-02-24 12:58_

Ok, thank you!

To me as a user, it was not clear why I can explicitly enable E303 (a rule in preview mode), but not explicitly enable/disable preview mode for new features of an already stable rule when `explicit-preview-rules = true` is set.

Is it a good idea to enable those experimental features only if the rule is explicitly selected when `explicit-preview-rules = true` is set? From my point of view this would be a more consistent behavior, but I might be missing some important other considerations.

```python
[tool.ruff.lint]
preview = true

# Use explicit-preview-rules for a more granular selection
explicit-preview-rules = true

# This would not enable E303 and keep default behavior of C419
# Currently it does not enable E303, but experimental features of C419 are enabled
select = ["C", "E"]

#  This would enable E303 and experimental features of C419
select = ["C", "E", "E303", "C419"]
```

---

_Comment by @MichaReiser on 2025-02-24 13:16_

Our motivation for preview is to give users a possibility to use new features early but it's also in our interest that we get feedback early. But it's not the first time that this has come up and there's some more context in https://github.com/astral-sh/ruff/issues/12343#issuecomment-2230633910. 

I'll close this issue in favor of the one I just linked because they're about the same preview-behavior confusion.

---

_Closed by @MichaReiser on 2025-02-24 13:16_

---
