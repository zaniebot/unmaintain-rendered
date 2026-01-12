```yaml
number: 13379
title: Fix placement of inline parameter comments
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: micha/fix-parameter-comment-placement
created_at: 2024-09-17T09:50:58Z
updated_at: 2024-09-18T06:26:08Z
url: https://github.com/astral-sh/ruff/pull/13379
synced_at: 2026-01-12T15:55:44Z
```

# Fix placement of inline parameter comments

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/13369

Before: The formatter made all parameter comments leading comments unless the parameter had type annotations. 
Now: Only make comments leading comments if they are between `*` and `**` and args/kwargs.


This bug fix doesn't require a new style guide because it doesn't change the placement of comments that were incorrectly made leading comments. 
It only changes the formatting for unformatted code.

## Test Plan

Added tests, verified the black snapshot.


## Black

Black preserves the placement of all comments.

```python
def args_many_comments_with_type_annotation(
    # before
    *
    # between * and name
    args  # trailing args
    # before colon
    :  # after colon
    # before type
    int,  # trailing type
    # after type
):
    pass
```

I don't think it's worth the complexity, considering that we haven't received any user reports that the comments were moved, and comments between `*` and the name and the name and `:` seem undesirable. 


---

_Label `bug` added by @MichaReiser on 2024-09-17 09:51_

---

_Label `formatter` added by @MichaReiser on 2024-09-17 09:51_

---

_Comment by @github-actions[bot] on 2024-09-17 10:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-09-17 10:04_

There are a few more parameter cases that we aren't handling correctly. Putting back in draft

---

_Converted to draft by @MichaReiser on 2024-09-17 10:04_

---

_Comment by @MichaReiser on 2024-09-17 10:36_

CC: @carljm because you once mentioned that you're interested in how our formatter works.

---

_Marked ready for review by @MichaReiser on 2024-09-17 10:40_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-17 10:40_

---

_@charliermarsh approved on 2024-09-17 18:19_

Wow been a while here. Looks reasonable though.

---

_Comment by @MichaReiser on 2024-09-17 18:45_

> Wow been a while here. Looks reasonable though.

Haha. That was my reaction too when I started working on this 🙈

---

_Merged by @MichaReiser on 2024-09-18 06:26_

---

_Closed by @MichaReiser on 2024-09-18 06:26_

---

_Branch deleted on 2024-09-18 06:26_

---
