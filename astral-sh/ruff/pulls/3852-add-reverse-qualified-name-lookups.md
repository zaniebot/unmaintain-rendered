```yaml
number: 3852
title: Add reverse-qualified-name lookups
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/example
created_at: 2023-04-02T16:52:50Z
updated_at: 2023-05-24T03:26:02Z
url: https://github.com/astral-sh/ruff/pull/3852
synced_at: 2026-01-12T03:50:02Z
```

# Add reverse-qualified-name lookups

---

_Pull request opened by @charliermarsh on 2023-04-02 16:52_

_No description provided._

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4128 on 2023-04-02 16:53_

(Ignore `rebounds` entirely for the purpose of this exercise.)

---

_@charliermarsh reviewed on 2023-04-02 16:53_

---

_@charliermarsh reviewed on 2023-04-02 16:53_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/binding.rs`:273 on 2023-04-02 16:53_

I also considered making this a trait and implementing it on these three `BindingKind` variants.

---

_@charliermarsh reviewed on 2023-04-02 16:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/scope.rs`:55 on 2023-04-02 16:54_

This is the API I'm trying to enable: look up a fully qualified name without having to iterate over all bindings.

---

_@charliermarsh reviewed on 2023-04-02 16:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/binding.rs`:272 on 2023-04-02 16:55_

I also considered changing `FromImportation` to store the normalized fields and create `full_name` on-demand:

```rust
#[derive(Clone, Debug)]
pub struct FromImportation<'a> {
    /// The name to which the member is bound.
    /// Given `from foo import bar`, `name` would be "bar".
    /// Given `from foo import bar as baz`, `name` would be "baz".
    pub name: &'a str,
    /// The level of the import. `None` or `Some(0)` indicate an absolute import.
    pub level: Option<usize>,
    /// The module being imported.
    pub module: Option<&'a str>,
    /// The member being imported.
    pub member: &'a str,
}
```

---

_Closed by @charliermarsh on 2023-05-24 03:26_

---
