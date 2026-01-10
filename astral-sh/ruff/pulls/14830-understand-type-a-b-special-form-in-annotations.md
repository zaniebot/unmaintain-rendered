```yaml
number: 14830
title: "Understand `type[A | B]` special form in annotations"
type: pull_request
state: merged
author: Glyphack
labels: []
assignees: []
merged: true
base: main
head: typing-type-union
created_at: 2024-12-07T15:13:40Z
updated_at: 2024-12-07T18:06:18Z
url: https://github.com/astral-sh/ruff/pull/14830
synced_at: 2026-01-10T20:42:27Z
```

# Understand `type[A | B]` special form in annotations

---

_Pull request opened by @Glyphack on 2024-12-07 15:13_

resolves https://github.com/astral-sh/ruff/issues/14703

I decided to use recursion to get the type, so if anything is added to the single element inference it will be applied for the union.
Also added this [change](https://github.com/astral-sh/ruff/issues/14703#issuecomment-2510286217) in this PR since it was easy.

---

_Review requested from @carljm by @Glyphack on 2024-12-07 15:13_

---

_Review requested from @MichaReiser by @Glyphack on 2024-12-07 15:13_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-12-07 15:13_

---

_Review requested from @sharkdp by @Glyphack on 2024-12-07 15:13_

---

_Converted to draft by @Glyphack on 2024-12-07 15:21_

---

_Comment by @github-actions[bot] on 2024-12-07 15:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Glyphack on 2024-12-07 16:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4682 on 2024-12-07 17:29_

nit: I think we can use a broader rule category here
```suggestion
                    "invalid-type-form",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md`:114 on 2024-12-07 17:29_

```suggestion
# error: [invalid-type-form]
```

---

_@carljm approved on 2024-12-07 17:29_

Looks great, thank you!

---

_Merged by @carljm on 2024-12-07 17:34_

---

_Closed by @carljm on 2024-12-07 17:34_

---

_Branch deleted on 2024-12-07 18:06_

---
