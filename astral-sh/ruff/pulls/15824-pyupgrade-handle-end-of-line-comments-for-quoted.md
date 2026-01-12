```yaml
number: 15824
title: "[`pyupgrade`] Handle end-of-line comments for `quoted-annotation` (`UP037`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: quoted-annots
created_at: 2025-01-30T03:07:32Z
updated_at: 2025-01-30T06:03:06Z
url: https://github.com/astral-sh/ruff/pull/15824
synced_at: 2026-01-12T15:55:52Z
```

# [`pyupgrade`] Handle end-of-line comments for `quoted-annotation` (`UP037`)

---

_@dylwil3_

This PR uses the tokens of the parsed annotation available in the `Checker`, instead of re-lexing (using `SimpleTokenizer`) the annotation. This avoids some limitations of the `SimpleTokenizer`, such as not being able to handle number and string literals.

Closes #15816 .


---

_Label `bug` added by @dylwil3 on 2025-01-30 03:07_

---

_Label `fixes` added by @dylwil3 on 2025-01-30 03:07_

---

_Comment by @github-actions[bot] on 2025-01-30 03:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/rules/quoted_annotation.rs`:93 on 2025-01-30 04:39_

nit: I'd remove this comment as the information exists as docs to `checker.tokens` method

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/rules/quoted_annotation.rs`:96 on 2025-01-30 04:45_

The parentheses doesn't seem necessary here.

What would happen if the annotation only contains the comment character i.e., `foo: "#"` which would give `Comment, Newline`? And, similarly for empty annotation like `foo: ""` which would just give `Newline`. I think we should use `saturating_sub` here.

```suggestion
        .get(checker.tokens().len() - 2)
```

---

_@dhruvmanila reviewed on 2025-01-30 04:47_

---

_@dylwil3 reviewed on 2025-01-30 05:20_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyupgrade/rules/quoted_annotation.rs`:96 on 2025-01-30 05:20_

I don't think either of those cases can occur inside this lint check because it is applied to _parsed_ annotations, and those two examples are syntax errors.

https://github.com/astral-sh/ruff/blob/3125332ec1bc86494feef57c6a6c64d3591a4831/crates/ruff_linter/src/checkers/ast/mod.rs#L2341-L2356

But I agree about the saturating subtraction and parentheses!

---

_@dhruvmanila approved on 2025-01-30 05:48_

---

_Merged by @dylwil3 on 2025-01-30 06:03_

---

_Closed by @dylwil3 on 2025-01-30 06:03_

---
