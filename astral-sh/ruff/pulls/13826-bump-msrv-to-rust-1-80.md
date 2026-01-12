```yaml
number: 13826
title: Bump MSRV to Rust 1.80
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust-1.80-msrv
created_at: 2024-10-19T19:50:24Z
updated_at: 2024-10-20T08:55:37Z
url: https://github.com/astral-sh/ruff/pull/13826
synced_at: 2026-01-12T15:55:45Z
```

# Bump MSRV to Rust 1.80

---

_@MichaReiser_

## Summary

Prerequisit for https://github.com/astral-sh/ruff/pull/13654

UV's MSRV is 1.81

## Test Plan

cargo test


---

_Review requested from @carljm by @MichaReiser on 2024-10-19 19:50_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-19 19:50_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-19 19:50_

---

_Label `internal` added by @MichaReiser on 2024-10-19 19:50_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-10-19 19:51_

---

_Comment by @github-actions[bot] on 2024-10-19 20:09_

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

_Review comment by @AlexWaygood on `crates/ruff_linter/Cargo.toml`:73 on 2024-10-19 22:35_

Did you mean to get rid of this dependency? It looks like you just moved it in this file

---

_@AlexWaygood approved on 2024-10-19 22:37_

I think there may be other modernisations we can do as a result of this. For example, I know that we can use non-deprecated `cargo` commands in `build.rs` files with this MSRV bump: all of these can be `cargo::` rather than `cargo:`:

```
crates/red_knot_python_semantic/build.rs:    println!("cargo::rerun-if-changed=resources/mdtest");
crates/red_knot_vendored/build.rs:    println!("cargo:rerun-if-changed={TYPESHED_SOURCE_DIR}");
crates/ruff/build.rs:    println!("cargo:rustc-env=RUST_HOST_TARGET={target}");
crates/ruff/build.rs:        "cargo:rerun-if-changed={}",
crates/ruff/build.rs:                "cargo:rerun-if-changed={}",
crates/ruff/build.rs:    println!("cargo:rustc-env=RUFF_COMMIT_HASH={}", next());
crates/ruff/build.rs:    println!("cargo:rustc-env=RUFF_COMMIT_SHORT_HASH={}", next());
crates/ruff/build.rs:    println!("cargo:rustc-env=RUFF_COMMIT_DATE={}", next());
crates/ruff/build.rs:            "cargo:rustc-env=RUFF_LAST_TAG={}",
crates/ruff/build.rs:            "cargo:rustc-env=RUFF_LAST_TAG_DISTANCE={}",
```

But I think it's fine to do that kind of thing as a followup

---

_@MichaReiser reviewed on 2024-10-20 08:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/Cargo.toml`:73 on 2024-10-20 08:25_

lol, Rust Rover must have inserted the dependency again :D

---

_@MichaReiser reviewed on 2024-10-20 08:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/Cargo.toml`:73 on 2024-10-20 08:26_

That also explains why the error count in ruff_linter suddenly dropped from 20 to 0 

---

_Comment by @MichaReiser on 2024-10-20 08:31_

I didn't know about the `cargo` change but I agree that we can do some of them as follow ups. I did go through the announcement blog posts to learn about obvious things (like the `once_cell`) that we can use now. 



---

_Merged by @MichaReiser on 2024-10-20 08:55_

---

_Closed by @MichaReiser on 2024-10-20 08:55_

---

_Branch deleted on 2024-10-20 08:55_

---
