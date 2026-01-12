```yaml
number: 2990
title: "[`flake8-tidy-imports`] extend autofix of relative imports"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: tid-imports-no-module
created_at: 2023-02-17T16:31:07Z
updated_at: 2023-02-17T23:05:04Z
url: https://github.com/astral-sh/ruff/pull/2990
synced_at: 2026-01-12T15:55:12Z
```

# [`flake8-tidy-imports`] extend autofix of relative imports

---

_@sbrugman_

This extends the autofix for TID252 to work with for relative imports without `module` (i.e. `from .. import`). Tested with `matplotlib` and `bokeh`.
(Previously it would panic on unwrap of the module) 

Note that pandas has [replaced](https://github.com/pandas-dev/pandas/commit/6057d7a93e6bb792508339dcf8e70998e7e78e1a) `absolufy-imports` with `ruff` now!

---

_@sbrugman reviewed on 2023-02-17 16:33_

---

_Review comment by @sbrugman on `crates/ruff/src/rules/flake8_tidy_imports/relative_imports.rs`:111 on 2023-02-17 16:33_

@charliermarsh Here I wanted to return `parts` as slice, but couldn't get the types to the other arms to match (`Vec<String>` vs `Vec<&str>`). Any pointers? Still learning idiomatic rust..

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_tidy_imports/relative_imports.rs`:110 on 2023-02-17 19:12_

The best I could come up with was to do the validation as part of this scope:

```rust
let module_name = if let Some(module) = module.or_else(|| {
    if parts.len() > 1 {
        Some(parts.last().unwrap())
    } else {
        None
    }
}) {
    let call_path = from_relative_import(&parts, module);
    let parts = call_path.as_slice();

    // Require import to be a valid PEP 8 module:
    // https://python.org/dev/peps/pep-0008/#package-and-module-names
    if parts.iter().any(|f| !is_lower_with_underscore(f)) {
        return None;
    }

    parts.join(".")
} else {
    // Require import to be a valid PEP 8 module:
    // https://python.org/dev/peps/pep-0008/#package-and-module-names
    if parts.iter().any(|f| !is_lower_with_underscore(f)) {
        return None;
    }

    parts.join(".")
};
```

But, I'm not convinced it's any better.

---

_@charliermarsh reviewed on 2023-02-17 19:12_

---

_@charliermarsh reviewed on 2023-02-17 19:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_tidy_imports/relative_imports.rs`:110 on 2023-02-17 19:28_

I ended up doing something like this, but let me know if you disagree with the current version. Just merging for expediency.

---

_Merged by @charliermarsh on 2023-02-17 19:35_

---

_Closed by @charliermarsh on 2023-02-17 19:35_

---

_Branch deleted on 2023-02-17 23:05_

---
