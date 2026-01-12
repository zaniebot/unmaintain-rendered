```yaml
number: 14275
title: Minor refactoring of some flake-pyi rules
type: pull_request
state: merged
author: sbrugman
labels:
  - internal
assignees: []
merged: true
base: main
head: pyi-internal-tweaks
created_at: 2024-11-11T12:49:05Z
updated_at: 2024-11-11T13:10:49Z
url: https://github.com/astral-sh/ruff/pull/14275
synced_at: 2026-01-12T15:55:47Z
```

# Minor refactoring of some flake-pyi rules

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Some very minor internal tweaks that I had locally after working on the `flake8-pyi` autofixes and didn't want to pollute the diff with. 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-11 12:49_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/future_annotations_in_stub.rs`:50 on 2024-11-11 12:52_

Even clearer in my opinion would be

```suggestion
    if name != "__future__" {
        return;
    };
    
    if names.iter().all(|alias| &*alias.name != "annotations") {
        return;
    }
```

---

_@AlexWaygood approved on 2024-11-11 12:53_

Thanks

---

_Label `internal` added by @AlexWaygood on 2024-11-11 12:53_

---

_Comment by @github-actions[bot] on 2024-11-11 13:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Minor internal tweaks" to "Minor refactoring of some flake-pyi rules" by @AlexWaygood on 2024-11-11 13:10_

---

_Merged by @AlexWaygood on 2024-11-11 13:10_

---

_Closed by @AlexWaygood on 2024-11-11 13:10_

---
