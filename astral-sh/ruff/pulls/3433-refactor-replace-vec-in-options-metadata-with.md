```yaml
number: 3433
title: "refactor: Replace `Vec` in options metadata with static array"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor/options-metadata
created_at: 2023-03-10T08:33:04Z
updated_at: 2023-03-11T09:03:58Z
url: https://github.com/astral-sh/ruff/pull/3433
synced_at: 2026-01-12T04:39:44Z
```

# refactor: Replace `Vec` in options metadata with static array

---

_Pull request opened by @MichaReiser on 2023-03-10 08:33_

This refactor replaces the usages of a `Vec` inside `get_available_options` with a static array slice. I used this refactor as an exercise to familiarize myself with options.

The old implementation used the `ConfigurationOptions` trait with a `get_available_options` method to retrieve the child-options of a group. The downside was that `get_available_options` allocated and returned a new vector with each invocation.

This implementation replaces the `ConfigurationOptions` trait with a `OptionsGroup` struct that references a static option entries slice and implements the `get` operation to find a child option.

## Test Plan

```bash
cargo dev generate-options
```

Generates no changes

```bash
> cargo run --bin ruff -q config flake8-annotations
mypy-init-return
suppress-dummy-args
suppress-none-returning
allow-star-arg-any
ignore-fully-untyped
```

---

_Assigned to @MichaReiser by @MichaReiser on 2023-03-10 08:33_

---

_Label `internal` added by @MichaReiser on 2023-03-10 08:33_

---

_Review comment by @MichaReiser on `crates/ruff/Cargo.toml`:16 on 2023-03-10 08:33_

We want doctests and `crate-type` is no longer necessary now that the wasm code lives in `ruff_wasm`

---

_@MichaReiser reviewed on 2023-03-10 08:33_

---

_Review comment by @MichaReiser on `crates/ruff/src/registry.rs`:821 on 2023-03-10 08:34_

We can make `LintSource` copy because its size is less than 8 bytes

---

_@MichaReiser reviewed on 2023-03-10 08:34_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/options_base.rs`:38 on 2023-03-10 21:03_

Maybe a ridiculous question, but is there any way to get Rust to compile these snippets, to ensure they're kept up to date?

---

_@charliermarsh reviewed on 2023-03-10 21:03_

---

_@charliermarsh approved on 2023-03-10 21:06_

---

_@MichaReiser reviewed on 2023-03-10 21:07_

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/options_base.rs`:38 on 2023-03-10 21:07_

Even better. The snippets run as part of our test suite ([doctests](https://doc.rust-lang.org/rustdoc/write-documentation/documentation-tests.html)) This is one of my favorite parts of Rust: writing documentation with examples gives you testing for free ☺️

---

_@charliermarsh reviewed on 2023-03-10 21:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/options_base.rs`:38 on 2023-03-10 21:08_

Ohh I assumed something like this was happening, but `cargo check --all` didn't have any effect for me after introducing errors.

---

_Merged by @MichaReiser on 2023-03-11 09:03_

---

_Closed by @MichaReiser on 2023-03-11 09:03_

---

_Branch deleted on 2023-03-11 09:03_

---
