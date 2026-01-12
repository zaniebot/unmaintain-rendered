```yaml
number: 15028
title: Fix some nightly lints
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-nightly-clippy
created_at: 2025-08-02T15:01:13Z
updated_at: 2025-08-02T18:59:24Z
url: https://github.com/astral-sh/uv/pull/15028
synced_at: 2026-01-12T16:11:32Z
```

# Fix some nightly lints

---

_@konstin_

Apply fixes for some `cargo check` and `cargo clippy` lints that are on in nightly Rust.

The following command now passes, the blanket allows had to many false-positives:

```
cargo +nightly clippy -- -A clippy::doc_markdown -A mismatched_lifetime_syntaxes -A clippy::explicit_deref_methods
```

`cargo +nightly check -- -A mismatched_lifetime_syntaxes` now passes without warnings.

---

_Label `internal` added by @konstin on 2025-08-02 15:01_

---

_@charliermarsh approved on 2025-08-02 15:27_

---

_Merged by @konstin on 2025-08-02 18:59_

---

_Closed by @konstin on 2025-08-02 18:59_

---

_Branch deleted on 2025-08-02 18:59_

---
