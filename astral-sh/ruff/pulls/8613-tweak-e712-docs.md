```yaml
number: 8613
title: "Tweak `E712` docs"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: e712-doc-tweak
created_at: 2023-11-11T09:30:01Z
updated_at: 2024-03-06T18:06:41Z
url: https://github.com/astral-sh/ruff/pull/8613
synced_at: 2026-01-10T22:47:01Z
```

# Tweak `E712` docs

---

_Pull request opened by @tjkuson on 2023-11-11 09:30_

Recommends `if predicate:` over `if predicate is True:` in the rule message.

Closes #8164.

---

_Marked ready for review by @tjkuson on 2023-11-11 09:32_

---

_Comment by @github-actions[bot] on 2023-11-11 10:09_

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

_Review requested from @zanieb by @charliermarsh on 2023-11-12 20:09_

---

_Label `documentation` added by @charliermarsh on 2023-11-12 20:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-03-06 17:09_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pycodestyle/rules/literal_comparisons.rs`:74 on 2024-03-06 17:11_

With this edit, the "What it does" section is empty

---

_@AlexWaygood requested changes on 2024-03-06 17:11_

---

_@AlexWaygood reviewed on 2024-03-06 17:17_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pycodestyle/rules/literal_comparisons.rs`:112 on 2024-03-06 17:17_

Hmm, I have a slight preference for the existing messages with these. `if cond is True` is something you (occasionally) need to write, and that's still a "direct comparison".

Maybe something like this? (And ditto for the other branches)

```suggestion
                format!("Avoid equality comparisons to `True`; use `if cond:` for truth checks")
```

---

_@tjkuson reviewed on 2024-03-06 17:34_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/pycodestyle/rules/literal_comparisons.rs`:74 on 2024-03-06 17:34_

Sorry for missing this, now added.

---

_@tjkuson reviewed on 2024-03-06 17:35_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/pycodestyle/rules/literal_comparisons.rs`:112 on 2024-03-06 17:35_

Added suggestion.

---

_Review requested from @AlexWaygood by @tjkuson on 2024-03-06 17:35_

---

_@AlexWaygood reviewed on 2024-03-06 17:37_

Looks good, but I think you'll need to update the snapshots!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pycodestyle/rules/literal_comparisons.rs`:74 on 2024-03-06 17:43_

nit:

```suggestion
/// Checks for equality comparisons to boolean literals.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E712_E712.py.snap`:12 on 2024-03-06 17:45_

It's possibly outside the scope of this PR (you can leave it for now, if you like), but this suggestion is pretty confusing. There's no variable named "cond" in scope here, and I don't think it's obvious that this means "replace `if res == True` with `if res`", especially when the carets only point to `True` rather than `res == True`.

---

_@AlexWaygood reviewed on 2024-03-06 17:46_

---

_@AlexWaygood approved on 2024-03-06 17:48_

Thanks! A followup PR to address https://github.com/astral-sh/ruff/pull/8613/files#r1514915070 would be very welcome, if you're up to it!

---

_Merged by @AlexWaygood on 2024-03-06 17:54_

---

_Closed by @AlexWaygood on 2024-03-06 17:54_

---

_Comment by @tjkuson on 2024-03-06 17:55_

> Thanks! A followup PR to address https://github.com/astral-sh/ruff/pull/8613/files#r1514915070 would be very welcome, if you're up to it!

Sure! I'll give it go.

---

_Branch deleted on 2024-03-06 17:55_

---
