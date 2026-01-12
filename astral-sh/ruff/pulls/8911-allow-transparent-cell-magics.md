```yaml
number: 8911
title: Allow transparent cell magics
type: pull_request
state: merged
author: dhruvmanila
labels:
  - linter
assignees: []
merged: true
base: main
head: dhruv/cell-magics
created_at: 2023-11-29T21:26:14Z
updated_at: 2023-12-07T20:15:45Z
url: https://github.com/astral-sh/ruff/pull/8911
synced_at: 2026-01-12T15:55:27Z
```

# Allow transparent cell magics

---

_@dhruvmanila_

## Summary

This PR updates the logic for `is_magic_cell` to include certain cell magics. These cell magics would contain Python code following the line defining the command. The code could define a variable which can then be referenced in other cells. Currently, we would ignore the cell completely leading to undefined-name violation.

As discussed in https://github.com/astral-sh/ruff/issues/8354#issuecomment-1832221009

## Test Plan

Add new test case to validate this scenario.


---

_Comment by @dhruvmanila on 2023-11-29 21:26_

Current dependencies on/for this PR:
* main
  * **PR #8911** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8911?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8911?utm_source=stack-comment).

---

_@charliermarsh reviewed on 2023-11-29 21:33_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/cell.rs`:193 on 2023-11-29 21:33_

So how does the parser handle this case?

---

_@dhruvmanila reviewed on 2023-11-29 21:37_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/cell.rs`:193 on 2023-11-29 21:37_

The parser would parse the first line `%%time` as a cell magic statement and the second line as an assignment statement.

This is technically a bug in the parser as both lines should be a single node instead but as we always ignored cell magic, it never came up.

---

_@charliermarsh reviewed on 2023-11-29 21:38_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/cell.rs`:193 on 2023-11-29 21:38_

Where does the cell magic end? Does it just include the `%%time` regardless of what follows? Like how would the parser deal with:

```python
%%time x = 1
y = x
```

---

_@dhruvmanila reviewed on 2023-11-29 21:40_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/cell.rs`:193 on 2023-11-29 21:40_

Each statement is a single node, so `%%time x = 1` would a single node and `y = x` would be an assignment statement. This code would raise undefined name for `x`. This is intentional as that requires support for inline parsing which might require more thinking.

---

_@charliermarsh reviewed on 2023-11-29 21:48_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/cell.rs`:193 on 2023-11-29 21:48_

I'm mostly wondering how the parser knows where the magic statement ends.

---

_@dhruvmanila reviewed on 2023-11-29 21:50_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/cell.rs`:193 on 2023-11-29 21:50_

Oh, it's the newline. The lexer consumes everything till the newline and considers this as part of the command.

---

_@charliermarsh reviewed on 2023-11-29 21:57_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/cell.rs`:193 on 2023-11-29 21:57_

Ah okay, makes sense. Could there be continuations though?

---

_@dhruvmanila reviewed on 2023-11-29 21:59_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/cell.rs`:193 on 2023-11-29 21:59_

Yeah, but that is ignored. So,
```python
%%time \
	x = 1
y = x
```

will parse in same way as your example.

---

_Comment by @github-actions[bot] on 2023-11-29 22:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2023-12-07 18:54_

@dhruvmanila - What are your thoughts on moving this one forward? Are there concerns?

---

_Marked ready for review by @dhruvmanila on 2023-12-07 19:40_

---

_Label `linter` added by @dhruvmanila on 2023-12-07 19:41_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-12-07 19:41_

---

_@charliermarsh approved on 2023-12-07 20:08_

---

_Merged by @dhruvmanila on 2023-12-07 20:15_

---

_Closed by @dhruvmanila on 2023-12-07 20:15_

---

_Branch deleted on 2023-12-07 20:15_

---
