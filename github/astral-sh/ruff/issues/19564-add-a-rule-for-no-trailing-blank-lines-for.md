---
number: 19564
title: Add a rule for no-trailing-blank-lines for docstrings
type: issue
state: open
author: chazmo03
labels:
  - rule
  - docstring
  - needs-decision
assignees: []
created_at: 2025-07-25T20:04:11Z
updated_at: 2025-07-29T21:55:11Z
url: https://github.com/astral-sh/ruff/issues/19564
synced_at: 2026-01-07T13:12:16-06:00
---

# Add a rule for no-trailing-blank-lines for docstrings

---

_Issue opened by @chazmo03 on 2025-07-25 20:04_

### Summary

Trailing blank lines _within_ multi-line docstrings should be flagged and removed. This already happens for single-line docstrings (thanks to `unnecessary-multiline-docstring`), so it seems sensible to do the same for multi-line docstrings as well.

See example:

```py
def function() -> None:
    """This single-line docstring would be fixed to remove the trailing blank lines.


    """

def function() -> None:
    """This multi-line docstring would NOT be fixed to remove the trailing blank lines.

    So long as there was a second non-blank line.


    """

```

---

_Label `rule` added by @ntBre on 2025-07-25 20:17_

---

_Label `needs-decision` added by @ntBre on 2025-07-25 20:17_

---

_Comment by @ntBre on 2025-07-25 20:18_

That makes sense to me. Interestingly, it looks like the formatter will also perform the first fix: https://play.ruff.rs/1e04904d-d7e3-4c3d-a71d-1965834e77c0?secondary=Format

but not the second.

---

_Comment by @MichaReiser on 2025-07-28 09:28_

We might also want to consider adding this behavior to an existing rule, e.g. https://docs.astral.sh/ruff/rules/surrounding-whitespace/

---

_Label `docstring` added by @MichaReiser on 2025-07-28 09:28_

---

_Comment by @chazmo03 on 2025-07-29 21:55_

@MichaReiser combining it into `surrounding-whitespace (D210)` makes sense to me.

---

_Referenced in [astral-sh/ruff#20738](../../astral-sh/ruff/issues/20738.md) on 2025-10-07 19:08_

---
