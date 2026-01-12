```yaml
number: 2583
title: Propagate attribute-like macros in define_rule_mapping
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cfg
created_at: 2023-02-05T17:11:07Z
updated_at: 2023-02-05T17:26:24Z
url: https://github.com/astral-sh/ruff/pull/2583
synced_at: 2026-01-12T15:55:08Z
```

# Propagate attribute-like macros in define_rule_mapping

---

_@charliermarsh_

This enables us to feature-flag rules, like:

```rust
ruff_macros::define_rule_mapping!(
    #[cfg(feature = "logical_lines")]
    E111 => rules::pycodestyle::rules::IndentationWithInvalidMultiple,
    ...
)
```

---

_@charliermarsh reviewed on 2023-02-05 17:17_

---

_Review comment by @charliermarsh on `ruff_macros/src/rule_code_prefix.rs`:36 on 2023-02-05 17:17_

AFAICT, `all_codes` is unused.

---

_Merged by @charliermarsh on 2023-02-05 17:26_

---

_Closed by @charliermarsh on 2023-02-05 17:26_

---

_Branch deleted on 2023-02-05 17:26_

---
