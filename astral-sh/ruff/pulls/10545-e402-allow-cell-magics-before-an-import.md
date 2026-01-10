```yaml
number: 10545
title: "[`E402`] Allow cell magics before an import"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/e402-ipy-stmt-fix
created_at: 2024-03-24T10:02:12Z
updated_at: 2024-03-24T10:50:01Z
url: https://github.com/astral-sh/ruff/pull/10545
synced_at: 2026-01-10T22:47:02Z
```

# [`E402`] Allow cell magics before an import

---

_Pull request opened by @dhruvmanila on 2024-03-24 10:02_

## Summary

This PR fixes the bug to allow Ipython escape commands to be present before an import statement and avoid raising `E402`.

fixes: #10357 

## Test Plan

Add test case and make sure they don't raise the `E402` violation.

On `main`, we get the following:

```
crates/ruff_linter/resources/test/fixtures/pycodestyle/E402.ipynb:cell 8:2:1: E402 Module level import not at top of cell
  |
1 | %%time
2 | import pathlib
  | ^^^^^^^^^^^^^^ E402
  |

crates/ruff_linter/resources/test/fixtures/pycodestyle/E402.ipynb:cell 10:3:1: E402 Module level import not at top of cell
  |
1 | %%time
2 | %%time
3 | import pathlib
  | ^^^^^^^^^^^^^^ E402
```


---

_Label `bug` added by @dhruvmanila on 2024-03-24 10:02_

---

_Label `bug` removed by @dhruvmanila on 2024-03-24 10:03_

---

_Label `rule` added by @dhruvmanila on 2024-03-24 10:03_

---

_Comment by @github-actions[bot] on 2024-03-24 10:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2024-03-24 10:50_

---

_Closed by @dhruvmanila on 2024-03-24 10:50_

---

_Branch deleted on 2024-03-24 10:50_

---
