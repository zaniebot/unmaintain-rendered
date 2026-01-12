```yaml
number: 3409
title: "Introduce a `ruff_diagnostics` crate"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ruff-diagnostics
created_at: 2023-03-08T21:03:44Z
updated_at: 2023-03-09T20:49:00Z
url: https://github.com/astral-sh/ruff/pull/3409
synced_at: 2026-01-12T04:39:44Z
```

# Introduce a `ruff_diagnostics` crate

---

_Pull request opened by @charliermarsh on 2023-03-08 21:03_

## Summary

This PR moves `Diagnostic`, `DiagnosticKind`, and `Fix` into their own crate, which will enable us to further split up Ruff, since sub-linter crates (which need to implement functions that return `Diagnostic`) can now depend on `ruff_diagnostics` rather than Ruff.


---

_Label `internal` added by @charliermarsh on 2023-03-08 21:03_

---

_Converted to draft by @charliermarsh on 2023-03-08 21:13_

---

_Marked ready for review by @charliermarsh on 2023-03-08 21:37_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-08 21:48_

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/mod.rs`:106 on 2023-03-09 09:49_

Nit: Rename to `into_diagnostic`

![image](https://user-images.githubusercontent.com/1203881/223984303-f8c55425-a14a-435b-94e2-3e321d4a1515.png)

[Source](https://rust-lang.github.io/api-guidelines/naming.html#ad-hoc-conversions-follow-as_-to_-into_-conventions-c-conv)


Or you can change it to the following `create_diagnostics` function that avoids the need for the vector:

```rust
fn create_diagnostics(fixes: impl IntoIterator<Item=Fix>) -> Vec<Diagnostic> {
  fixes.into_iter().map(|fix| Diagnostic {
        // The choice of rule here is arbitrary.
        kind: NoNewLineAtEndOfFile.into(),
        location: fix.location,
        end_location: fix.end_location,
        fix: Some(fix),
        parent: None,
    }).collect()
}
```

Usage: 

```rust
let empty_diagnostics = create_diagnostics([]);
let non_empty = create_diagnostics([Fix {
    content: "Bar".to_string(),
    location: Location::new(1, 8),
    end_location: Location::new(1, 14),
}])
```

---

_@MichaReiser approved on 2023-03-09 09:54_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/mod.rs`:106 on 2023-03-09 20:39_

Awesome, thank you!

---

_@charliermarsh reviewed on 2023-03-09 20:39_

---

_Merged by @charliermarsh on 2023-03-09 20:48_

---

_Closed by @charliermarsh on 2023-03-09 20:48_

---

_Branch deleted on 2023-03-09 20:49_

---
