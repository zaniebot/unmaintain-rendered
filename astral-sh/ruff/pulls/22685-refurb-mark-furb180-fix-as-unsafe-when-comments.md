```yaml
number: 22685
title: "[refurb] Mark FURB180 fix as unsafe when comments would be deleted"
type: pull_request
state: closed
author: AndyBodnar
labels: []
assignees: []
base: main
head: fix-furb180-comments-unsafe
created_at: 2026-01-18T17:02:42Z
updated_at: 2026-01-18T17:12:47Z
url: https://github.com/astral-sh/ruff/pull/22685
synced_at: 2026-01-18T17:26:31Z
```

# [refurb] Mark FURB180 fix as unsafe when comments would be deleted

---

_@AndyBodnar_

Fixes #22631

The safe fix for FURB180 could delete comments that appear between keyword arguments. For example:

```python
class C(
    other_kwarg=1,
    # comment
    metaclass=abc.ABCMeta,
):
    pass
```

The fix deletes from the previous argument to the end of the metaclass keyword, which would remove the comment. This change checks if any comments exist in that deletion range and marks the fix as unsafe if so.

Added a test case covering this scenario.

---

_Comment by @ntBre on 2026-01-18 17:12_

Thanks for working on this! But this is already resolved in #22234, so I'll close this for now.

---

_Closed by @ntBre on 2026-01-18 17:12_

---
