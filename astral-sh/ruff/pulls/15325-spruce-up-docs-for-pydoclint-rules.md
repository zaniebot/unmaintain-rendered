```yaml
number: 15325
title: Spruce up docs for pydoclint rules
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
  - docstring
assignees: []
merged: true
base: main
head: alex/pydoclint-docs
created_at: 2025-01-07T19:07:11Z
updated_at: 2025-01-08T12:24:34Z
url: https://github.com/astral-sh/ruff/pull/15325
synced_at: 2026-01-10T20:34:00Z
```

# Spruce up docs for pydoclint rules

---

_Pull request opened by @AlexWaygood on 2025-01-07 19:07_

## Summary

Some minor copyediting prior to stabilising some of these rules for Ruff 0.9


---

_Label `documentation` added by @AlexWaygood on 2025-01-07 19:07_

---

_Label `docstring` added by @AlexWaygood on 2025-01-07 19:07_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-07 19:07_

---

_Review requested from @dylwil3 by @AlexWaygood on 2025-01-07 19:07_

---

_Comment by @github-actions[bot] on 2025-01-07 19:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:73 on 2025-01-07 23:51_

Somehow "redundant" makes it seem like they had more than one Returns section or something.

```suggestion
/// Checks for function docstrings with unnecessary "Returns" section.
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:176 on 2025-01-07 23:53_

Similarly

```suggestion
/// Checks for function docstrings with unnecessary "Yields" section.
```

---

_@dylwil3 approved on 2025-01-07 23:54_

LGTM - some optional tweaks

---

_@AlexWaygood reviewed on 2025-01-07 23:57_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:73 on 2025-01-07 23:57_

"unnecessary" works for me -- I think it should be _sections_ rather than _section_, though, since we're talking about "function docstrings", which is plural?

---

_@dylwil3 reviewed on 2025-01-08 00:27_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:73 on 2025-01-08 00:27_

I defer to the crown for matters linguistic 👑

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:126 on 2025-01-08 07:36_

I like the wording here better than the one for returns above. It's simpler (less plural form). Maybe we can align the two? 

---

_@MichaReiser approved on 2025-01-08 07:37_

Nice. I love your doc improvements. They're always next level.

---

_Merged by @AlexWaygood on 2025-01-08 12:22_

---

_Closed by @AlexWaygood on 2025-01-08 12:22_

---

_Branch deleted on 2025-01-08 12:22_

---
