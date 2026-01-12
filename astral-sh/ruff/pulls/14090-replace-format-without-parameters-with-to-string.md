```yaml
number: 14090
title: "Replace `format!` without parameters with `.to_string()`"
type: pull_request
state: merged
author: sbrugman
labels:
  - internal
assignees: []
merged: true
base: main
head: nit-format-without-parameters
created_at: 2024-11-04T12:22:47Z
updated_at: 2024-11-04T14:18:39Z
url: https://github.com/astral-sh/ruff/pull/14090
synced_at: 2026-01-12T15:55:46Z
```

# Replace `format!` without parameters with `.to_string()`

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

From https://github.com/astral-sh/ruff/pull/14008/commits/56047ffa53ffd0fb96737b977fcd0612c5103721 I realised that `to_string` is preferred over `format!` when no arguments are provided. This PR consistently does so for existing violations.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

`cargo test`
<!-- How was it tested? -->


---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-04 12:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_placeholder.rs`:77 on 2024-11-04 12:28_

here you might be able to do something like

```rs
    fn fix_title(&self) -> String {
        let title = match &self.kind {
            Placeholder::Pass => "Remove unnecessary `pass`",
            Placeholder::Ellipsis => "Remove unnecessary `...`",
        };
        title.to_string()
    }
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_ifexp.rs`:57 on 2024-11-04 12:30_

similarly here, we could probably do something like

```rs
    fn fix_title(&self) -> Option<String> {
        let title = if self.is_compare {
            "Remove unnecessary `True if ... else False`"
        } else {
            "Replace with `bool(...)"
        };
        Some(title.to_string())
    }
```

which I would find marginally more readable

---

_@AlexWaygood approved on 2024-11-04 12:31_

Nice cleanup! A couple of optional nits :-)

---

_Comment by @github-actions[bot] on 2024-11-04 12:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @MichaReiser on 2024-11-04 12:58_

---

_@sbrugman reviewed on 2024-11-04 13:25_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_placeholder.rs`:77 on 2024-11-04 13:25_

Thanks, changed this according to your suggestions.

---

_@AlexWaygood approved on 2024-11-04 14:04_

Thanks!

---

_Merged by @AlexWaygood on 2024-11-04 14:09_

---

_Closed by @AlexWaygood on 2024-11-04 14:09_

---

_Branch deleted on 2024-11-04 14:10_

---
