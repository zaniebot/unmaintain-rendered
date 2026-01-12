```yaml
number: 14788
title: "[red-knot] Move standalone expr inference to `for` non-name target"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/double-extend
created_at: 2024-12-05T12:15:57Z
updated_at: 2024-12-06T02:22:23Z
url: https://github.com/astral-sh/ruff/pull/14788
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Move standalone expr inference to `for` non-name target

---

_@dhruvmanila_

## Summary

Ref: https://github.com/astral-sh/ruff/pull/14754#discussion_r1871040646

## Test Plan

Remove the TODO comment and update the mdtest.


---

_Label `red-knot` added by @dhruvmanila on 2024-12-05 12:15_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-05 12:15_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-05 12:15_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-05 12:15_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-05 12:15_

---

_@MichaReiser reviewed on 2024-12-05 12:19_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1907 on 2024-12-05 12:19_

Why is it okay to only infer the iterator when the target isn't a name? Is it because it is otherwise handled in `infer_for_statement_definition` or is it something else?

---

_Comment by @github-actions[bot] on 2024-12-05 12:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-12-05 12:26_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1907 on 2024-12-05 12:26_

Yes, that's correct. It's handled while inferring the definition types. This pattern is also followed in other definitions like assignment, with items, etc.

---

_@dhruvmanila reviewed on 2024-12-05 12:29_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1907 on 2024-12-05 12:29_

The main reason for this requirement is the `self.extend` call which duplicated the diagnostics.

---

_@MichaReiser approved on 2024-12-05 12:35_

---

_Merged by @dhruvmanila on 2024-12-05 12:36_

---

_Closed by @dhruvmanila on 2024-12-05 12:36_

---

_Branch deleted on 2024-12-05 12:36_

---

_@carljm reviewed on 2024-12-05 22:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/invalid_syntax.md`:31 on 2024-12-05 22:27_

Why are there still three invalid-syntax errors? Can we add messages clarifying what the three errors are? I would expect two (one for `while` used as identifier, the other for `pass` used as identifier.)

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/invalid_syntax.md`:31 on 2024-12-06 02:22_

In brief, our parser cannot recover from certain syntax errors completely. For example, if there's a keyword (like `for`) in the middle of another statement (like function definition), then it's more likely that the rest of the tokens are going to be part of the `for` statement and not the function definition. But, it's not necessary that the remaining tokens are valid in the context of a `for` statement. This is what's happening here. There's a function definition before this part of the code that the parser doesn't recover well from:

```py
def True(for):
    pass

for while in pass:
    pass
```

Try commenting out the function definition and you'll get the expected number of errors. I could add this as a general comment to this file.

Playground: https://play.ruff.rs/a36e1a68-abd9-404d-bb79-146855c6d521

---

_@dhruvmanila reviewed on 2024-12-06 02:22_

---
