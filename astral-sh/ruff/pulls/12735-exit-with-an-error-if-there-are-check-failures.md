```yaml
number: 12735
title: Exit with an error if there are check failures
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: add-exit-code
created_at: 2024-08-07T15:49:29Z
updated_at: 2024-08-08T07:19:26Z
url: https://github.com/astral-sh/ruff/pull/12735
synced_at: 2026-01-12T15:55:42Z
```

# Exit with an error if there are check failures

---

_@MichaReiser_

## Summary

This PR copies the behavior from Ruff to exit with 1 if there are linting errors and 0 otherwise. 

## Test Plan

* I ran Red Knot on a directory with linting errors and verified that the exit code is 1.
* I ran Red Knot on a directory without linting errors and verified that the exit code is 0.
<!-- How was it tested? -->


---

_Review requested from @carljm by @MichaReiser on 2024-08-07 15:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-07 15:49_

---

_Label `red-knot` added by @MichaReiser on 2024-08-07 15:49_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:214 on 2024-08-07 15:56_

No strong opinion as to which is preferable, but could you also write this as

```suggestion
#[derive(Copy, Clone)]
pub enum ExitStatus {
    /// Linting was successful and there were no linting errors.
    Success = 0,
    /// Linting was successful but there were linting errors.
    Failure = 1,
    /// Linting failed.
    Error = 2,
}

impl From<ExitStatus> for ExitCode {
    fn from(status: ExitStatus) -> Self {
        ExitCode::from(status as u8)
    }
}
```

---

_@AlexWaygood approved on 2024-08-07 15:56_

---

_Comment by @github-actions[bot] on 2024-08-07 16:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-08-07 16:19_

---

_Merged by @MichaReiser on 2024-08-08 07:10_

---

_Closed by @MichaReiser on 2024-08-08 07:10_

---

_Branch deleted on 2024-08-08 07:10_

---
