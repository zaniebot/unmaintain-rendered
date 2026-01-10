```yaml
number: 20762
title: RUF100 fix removes remaining comment
type: issue
state: closed
author: adamtheturtle
labels:
  - documentation
  - fixes
assignees: []
created_at: 2025-10-08T10:48:17Z
updated_at: 2026-01-09T17:01:11Z
url: https://github.com/astral-sh/ruff/issues/20762
synced_at: 2026-01-10T11:09:59Z
```

# RUF100 fix removes remaining comment

---

_Issue opened by @adamtheturtle on 2025-10-08 10:48_

### Summary

I use `ruff` and `pylint`.
I used to have to ignore errors for each on the same line, so I added the following ignore:

```python
"""Demonstrate ruff removal of pylint disable."""

import ast


class A(ast.NodeVisitor):
    """Subclass of ast.NodeVisitor."""

    def visit_ImportFrom(  # noqa: N802, pylint: disable=invalid-name
        self,
        node: ast.ImportFrom,
    ) -> None:
        """Override method."""
```

`ruff check --select=ALL ruff_example.py` gives:

```
RUF100 [*] Unused `noqa` directive (unused: `N802`)
  --> ruff_example.py:9:28
   |
 7 |     """Subclass of ast.NodeVisitor."""
 8 |
 9 |     def visit_ImportFrom(  # noqa: N802, pylint: disable=invalid-name
   |                            ^^^^^^^^^^^^
10 |         self,
11 |         node: ast.ImportFrom,
   |
help: Remove unused `noqa` directive

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Adding `--fix` unexpectedly removes the `pylint` disable part of the comment (meaning that `pylint` errors).

### Version

0.14.0

---

_Comment by @ntBre on 2025-10-08 13:25_

I was looking through other `RUF100` issues, and it looks like this was actually an intentional change in https://github.com/astral-sh/ruff/issues/12251 to assume that the rest of the comment is a description of the noqa code. The recommended workaround is to use a second `#` in the comment to separate the suppression comments like:

```py
def visit_ImportFrom(  # noqa: N802 # pylint: disable=invalid-name
```

`RUF100` won't delete past the second `#` in this case: [playground](https://play.ruff.rs/363341bf-d6a1-42e3-b367-39ec140dab6d).

---

_Label `question` added by @ntBre on 2025-10-08 13:25_

---

_Comment by @adamtheturtle on 2025-10-08 15:02_

Thank you for taking a look.

I think that this should be documented in https://docs.astral.sh/ruff/rules/unused-noqa/.
I also think perhaps the fix should be marked unsafe in the case that it will remove other comments.

---

_Label `documentation` added by @ntBre on 2025-10-08 19:54_

---

_Label `fixes` added by @ntBre on 2025-10-08 19:54_

---

_Label `question` removed by @ntBre on 2025-10-08 19:54_

---

_Comment by @ntBre on 2025-10-08 19:54_

Yeah, I think those both make sense!

---

_Comment by @MichaReiser on 2025-10-10 05:27_

I agree with the documentation but I don't think the fix should be marked as unsafe. Marking fixes as unsafe comes at a cost, they stop working out of the box. In this case, the fix does work as intended and follows noqa semantics. 

---

_Closed by @amyreese on 2026-01-09 17:01_

---
