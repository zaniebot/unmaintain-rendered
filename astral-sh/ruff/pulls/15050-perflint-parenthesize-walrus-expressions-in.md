```yaml
number: 15050
title: "[`perflint`] Parenthesize walrus expressions in autofix for `manual-list-comprehension` (`PERF401`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - documentation
  - fixes
assignees: []
merged: true
base: main
head: list-compr-walrus
created_at: 2024-12-18T21:33:23Z
updated_at: 2024-12-19T12:56:46Z
url: https://github.com/astral-sh/ruff/pull/15050
synced_at: 2026-01-12T15:55:50Z
```

# [`perflint`] Parenthesize walrus expressions in autofix for `manual-list-comprehension` (`PERF401`)

---

_@dylwil3_

This PR ensures that, when transforming a for-loop into a comprehension for the rule [manual-list-comprehension (PERF401)](https://docs.astral.sh/ruff/rules/manual-list-comprehension/#manual-list-comprehension-perf401), we parenthesize if-stmt tests when they are assignment expressions.

Closes #15047


---

_Label `bug` added by @dylwil3 on 2024-12-18 21:33_

---

_Label `documentation` added by @dylwil3 on 2024-12-18 21:33_

---

_Label `fixes` added by @dylwil3 on 2024-12-18 21:33_

---

_@dylwil3 reviewed on 2024-12-18 21:34_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:363 on 2024-12-18 21:34_

This was my understanding of the last paragraph [here](https://docs.python.org/3/reference/expressions.html#assignment-expressions), but someone should check me.

---

_Comment by @github-actions[bot] on 2024-12-18 21:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-12-19 12:18_

---

_@dhruvmanila reviewed on 2024-12-19 12:26_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:363 on 2024-12-19 12:26_

Yeah, I think that's correct. In any other position, the assignment expression should already surrounded by parentheses. There's the slice expression which doesn't requires parentheses because of the square brackets (`if data[x := 2]: ...`) but that shouldn't create a syntax error with the current autofix (https://play.ruff.rs/81286e9b-248e-4707-9abf-fdd6f38e95cb).

---

_Merged by @dylwil3 on 2024-12-19 12:56_

---

_Closed by @dylwil3 on 2024-12-19 12:56_

---
