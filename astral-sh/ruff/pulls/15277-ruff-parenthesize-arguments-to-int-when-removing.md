```yaml
number: 15277
title: "[`ruff`] Parenthesize arguments to `int` when removing `int` would change semantics in `unnecessary-cast-to-int` (`RUF046`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: cast-int
created_at: 2025-01-05T21:04:44Z
updated_at: 2025-01-07T21:45:23Z
url: https://github.com/astral-sh/ruff/pull/15277
synced_at: 2026-01-12T15:55:50Z
```

# [`ruff`] Parenthesize arguments to `int` when removing `int` would change semantics in `unnecessary-cast-to-int` (`RUF046`)

---

_@dylwil3_

When removing `int` in calls like `int(expr)` we may need to keep parentheses around `expr` even when it is a function call or subscript, since there may be newlines in between the function/value name and the opening parentheses/bracket of the argument.

This PR implements that logic.

Closes #15263


---

_Label `bug` added by @dylwil3 on 2025-01-05 21:04_

---

_Label `fixes` added by @dylwil3 on 2025-01-05 21:04_

---

_Label `preview` added by @dylwil3 on 2025-01-05 21:04_

---

_Comment by @github-actions[bot] on 2025-01-05 21:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@InSyncWithFoo reviewed on 2025-01-05 21:36_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/ruff/RUF046.py`:180 on 2025-01-05 21:36_

Could you add a few tests with parenthesized `func`s?

```python
int((round)  # Comment
(42)
)

int((round  # Comment
)(42)
)

int(  # Comment
(  # Comment
    (round
)  # Comment
)(42)
)

# etc.
```

---

_@dylwil3 reviewed on 2025-01-05 21:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/ruff/RUF046.py`:180 on 2025-01-05 21:38_

Good call! I'll probably have to use the end of the parenthesized range of the function name as the start of the check for newlines.

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/ruff/RUF046.py`:180 on 2025-01-05 22:00_

An `outermost_parenthesized_range()` function would probably do us some good. I can recall some of my old PRs that could have used such a function (#14978, #15097).

---

_@InSyncWithFoo reviewed on 2025-01-05 22:00_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:287 on 2025-01-06 06:11_

Can you add couple of test cases for subscript expressions?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__preview__RUF046_RUF046.py.snap`:1196 on 2025-01-06 06:19_

I think we'd need to retain the parentheses for this case as otherwise it's removing the first comment after `int(` on line 190.

---

_@dhruvmanila reviewed on 2025-01-06 06:19_

---

_@dylwil3 reviewed on 2025-01-06 17:56_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__preview__RUF046_RUF046.py.snap`:1196 on 2025-01-06 17:56_

looks like this is an issue even in much simpler cases like

```python
int( # a comment
    round(42)
)
```
I'll try to add some logic to preserve the end-of-line comment.

---

_@MichaReiser reviewed on 2025-01-07 08:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__preview__RUF046_RUF046.py.snap`:1196 on 2025-01-07 08:01_

Note: An alternative option is to mark the fix as unsafe because of the comment.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_cast_to_int.rs`:287 on 2025-01-07 17:39_

As far as I can tell, type inference is not robust enough to ever apply this rule to a subscript. So I can't write a test that would trigger this branch. On the other hand, I think I should leave this match arm in case type inference is later improved to cover this case.

---

_@dylwil3 reviewed on 2025-01-07 17:39_

---

_@dylwil3 reviewed on 2025-01-07 18:01_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__preview__RUF046_RUF046.py.snap`:1196 on 2025-01-07 18:01_

went with the latter option

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-01-07 18:02_

---

_@charliermarsh approved on 2025-01-07 21:39_

Thanks!

---

_Merged by @charliermarsh on 2025-01-07 21:43_

---

_Closed by @charliermarsh on 2025-01-07 21:43_

---
