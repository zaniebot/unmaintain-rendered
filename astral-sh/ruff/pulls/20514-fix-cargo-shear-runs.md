```yaml
number: 20514
title: "Fix 'cargo shear' runs"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
assignees: []
merged: true
base: main
head: david/fix-cargo-shear
created_at: 2025-09-22T13:49:19Z
updated_at: 2025-09-22T14:04:58Z
url: https://github.com/astral-sh/ruff/pull/20514
synced_at: 2026-01-10T17:40:28Z
```

# Fix 'cargo shear' runs

---

_Pull request opened by @sharkdp on 2025-09-22 13:49_

## Summary

Previous error:

```
▶ cargo shear
Analyzing /home/shark/ruff

ruff_diagnostics -- crates/ruff_diagnostics/Cargo.toml:
  get-size2

ruff_index -- crates/ruff_index/Cargo.toml:
  get-size2

ruff_source_file -- crates/ruff_source_file/Cargo.toml:
  get-size2

ruff_text_size -- crates/ruff_text_size/Cargo.toml:
  get-size2

ty_ide -- crates/ty_ide/Cargo.toml:
  get-size2

ty_project -- crates/ty_project/Cargo.toml:
  get-size2


cargo-shear may have detected unused dependencies incorrectly due to its limitations.
They can be ignored by adding the crate name to the package's Cargo.toml:

[package.metadata.cargo-shear]
ignored = ["crate-name"]

or in the workspace Cargo.toml:

[workspace.metadata.cargo-shear]
ignored = ["crate-name"]
```

---

_Label `ty` added by @sharkdp on 2025-09-22 13:49_

---

_Label `ty` removed by @sharkdp on 2025-09-22 13:49_

---

_Label `ci` added by @sharkdp on 2025-09-22 13:49_

---

_Comment by @MichaReiser on 2025-09-22 14:01_

I assume this "broke" because of the cargo-shear update 28 minutes ago?

---

_Comment by @github-actions[bot] on 2025-09-22 14:01_

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

_@MichaReiser approved on 2025-09-22 14:02_

---

_Merged by @sharkdp on 2025-09-22 14:04_

---

_Closed by @sharkdp on 2025-09-22 14:04_

---

_Branch deleted on 2025-09-22 14:04_

---
