```yaml
number: 11522
title: Insert blank lines before comments in E305
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/line
created_at: 2024-05-23T20:42:52Z
updated_at: 2024-05-27T17:05:16Z
url: https://github.com/astral-sh/ruff/pull/11522
synced_at: 2026-01-12T15:55:38Z
```

# Insert blank lines before comments in E305

---

_@charliermarsh_

## Summary

Given:

```python
class A:
    pass

# ====== Cool constants ========
BANANA = 100
APPLE = 200
```

We now insert the blank line _before_ the comment, rather than inserting two blank lines _after_ the comment.

Closes https://github.com/astral-sh/ruff/issues/11508.


---

_Review requested from @MichaReiser by @charliermarsh on 2024-05-23 20:42_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-05-23 20:42_

---

_Review request for @MichaReiser removed by @charliermarsh on 2024-05-23 20:43_

---

_Label `bug` added by @charliermarsh on 2024-05-23 20:43_

---

_Comment by @github-actions[bot] on 2024-05-23 20:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E305_E30.py.snap`:20 on 2024-05-24 04:38_

I think this is incorrect. The blank lines got added between the last statement and the comment within the same block (function body).

---

_@dhruvmanila reviewed on 2024-05-24 04:38_

---

_@dhruvmanila reviewed on 2024-05-24 04:39_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E305_E30.py.snap`:38 on 2024-05-24 04:39_

Same as above.

The diagnostic highlights the correct location but the fix seems to be incorrect.

```
/Users/dhruv/playground/ruff/src/E305.py:7:1: E305 [*] Expected 2 blank lines after class or function definition, found (1)
  |
6 |     # another comment
7 | a = 1
  | ^ E305
8 | # end
  |
  = help: Add missing blank line(s)

ℹ Safe fix
1 1 | class Class():
2 2 |     pass
3 3 | 
  4 |+
4 5 |     # comment
5 6 | 
6 7 |     # another comment
```

---

_Comment by @kaddkaka on 2024-05-24 07:38_

Why is ruff mistaken about what scope the comment belongs to? Should we be wary of more similar bugs with comments elsewhere (for example in implementation of other rules/fixes/formatting)?

---

_Comment by @dhruvmanila on 2024-05-24 07:40_

> Why is ruff mistaken about what scope the comment belongs to? Should we be wary of more similar bugs with comments elsewhere (for example in implementation of other rules/fixes/formatting)?

No, you don't need to worry about this elsewhere. It's probably just a logic error in this PR specifically.

---

_Comment by @charliermarsh on 2024-05-27 17:05_

Closing for now as I don't have time to fix the comment scoping issue. It's a shame because we already implement this very robustly in the formatter!

---

_Closed by @charliermarsh on 2024-05-27 17:05_

---
