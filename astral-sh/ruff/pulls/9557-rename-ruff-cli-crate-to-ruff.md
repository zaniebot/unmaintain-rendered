```yaml
number: 9557
title: "Rename `ruff_cli` crate to `ruff`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ruff
created_at: 2024-01-16T20:53:32Z
updated_at: 2024-01-16T22:47:02Z
url: https://github.com/astral-sh/ruff/pull/9557
synced_at: 2026-01-10T22:57:09Z
```

# Rename `ruff_cli` crate to `ruff`

---

_Pull request opened by @charliermarsh on 2024-01-16 20:53_

## Summary

Long ago, we had a single `ruff` crate. We started to break that up, and at some point, we wanted to separate the CLI from the core library. So we created `ruff_cli`, which created a `ruff` binary. Later, the `ruff` crate was renamed to `ruff_linter` and further broken up into additional crates.

(This is all from memory -- I didn't bother to look through the history to ensure that this is 100% correct :))

Now that `ruff` no longer exists, this PR renames `ruff_cli` to `ruff`. The primary benefit is that the binary target and the crate name are now the same, which helps with downstream tooling like `cargo-dist`, and also removes some complexity from the crate and `Cargo.toml` itself.

## Test Plan

- Ran `rm -rf target/release`.
- Ran `cargo build --release`.
- Verified that `./target/release/ruff` was created.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-16 20:53_

---

_Label `internal` added by @charliermarsh on 2024-01-16 20:53_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-16 20:53_

---

_Comment by @MichaReiser on 2024-01-16 21:15_

This will mess with my brain mussels... 

---

_@MichaReiser approved on 2024-01-16 21:17_

Is there anything that needs changing in the release workflow?

---

_Comment by @github-actions[bot] on 2024-01-16 21:19_

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

_Comment by @charliermarsh on 2024-01-16 22:46_

@MichaReiser - I don't believe so because we build the binaries with Maturin (which works with the changes to `pyproject.toml`), and the binaries generated still have the same name.

---

_Merged by @charliermarsh on 2024-01-16 22:47_

---

_Closed by @charliermarsh on 2024-01-16 22:47_

---

_Branch deleted on 2024-01-16 22:47_

---
