```yaml
number: 3398
title: "Make `ruff_cli` binary a small wrapper around `lib`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor/ruff-bin
created_at: 2023-03-08T07:51:16Z
updated_at: 2023-03-08T11:12:01Z
url: https://github.com/astral-sh/ruff/pull/3398
synced_at: 2026-01-12T15:55:12Z
```

# Make `ruff_cli` binary a small wrapper around `lib`

---

_@MichaReiser_

This PR refactors `ruff_cli` so that the binary is only a small wrapper around the `run` function exposed by the `ruff_cli` lib. 

My motivation for this change is that it previously was necessary to add some files to the `lib.rs` and `main.rs` files, and Rust then started complaining that some types are unused when building the `lib` or `binary`. 

This PR also refactors the dev cli arguments from `--dry-run` and `--check` to a single mode argument because `--dry-run` and `--check` are mutually exclusive. 

---

_Review requested from @konstin by @MichaReiser on 2023-03-08 07:51_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-08 07:51_

---

_Label `internal` added by @MichaReiser on 2023-03-08 07:51_

---

_@MichaReiser reviewed on 2023-03-08 07:52_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:44 on 2023-03-08 07:52_

Most of it is moved from `main.rs`. The main difference is that it now exposes a `run` function with the arguments. 

---

_Review comment by @konstin on `crates/ruff_cli/src/bin/ruff.rs`:31 on 2023-03-08 10:21_

Not for this PR, but i'd like it if just `ruff` ran linting from the project root pyproject.toml (or the git root), similar to `cargo fmt` or `cargo clippy`

---

_@konstin reviewed on 2023-03-08 10:21_

---

_@konstin approved on 2023-03-08 10:22_

---

_Merged by @MichaReiser on 2023-03-08 11:11_

---

_Closed by @MichaReiser on 2023-03-08 11:11_

---

_Branch deleted on 2023-03-08 11:12_

---
