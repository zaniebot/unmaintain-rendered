```yaml
number: 18197
title: Add rustfmt.toml file
type: pull_request
state: merged
author: dcreager
labels:
  - internal
assignees: []
merged: true
base: main
head: dcreager/rustfmt
created_at: 2025-05-19T15:14:01Z
updated_at: 2025-05-19T15:41:00Z
url: https://github.com/astral-sh/ruff/pull/18197
synced_at: 2026-01-12T15:56:14Z
```

# Add rustfmt.toml file

---

_@dcreager_

My editor runs `rustfmt` on save to format Rust code, not `cargo fmt`.

With our recent bump to the Rust 2024 edition, the formatting that `rustfmt`/`cargo fmt` applies changed. Unfortunately, `rustfmt` and `cargo fmt` have different behaviors for determining which edition to use when formatting: `cargo fmt` looks for the Rust edition in `Cargo.toml`, whereas `rustfmt` looks for it in `rustfmt.toml`. As a result, whenever I save, I have to remember to manually run `cargo fmt` before committing/pushing.

There is an open issue asking for `rustfmt` to also look at `Cargo.toml` when it's present (https://github.com/rust-lang/rust.vim/issues/368), but it seems like they "closed" that issue just by bumping the default edition (six years ago, from 2015 to 2018).

In the meantime, this PR adds a `rustfmt.toml` file with our current Rust edition so that both invocation have the same behavior. I don't love that this duplicates information in `Cargo.toml`, but I've added a reminder comment there to hopefully ensure that we bump the edition in both places three years from now.

---

_Label `internal` added by @dcreager on 2025-05-19 15:14_

---

_Comment by @dcreager on 2025-05-19 15:22_

The `ruff_text_size` crate was hard-coding a Rust edition instead of inheriting it from the workspace.  Updated that crate to be consistent with the other internal crates

---

_Review requested from @MichaReiser by @dcreager on 2025-05-19 15:24_

---

_Review requested from @dhruvmanila by @dcreager on 2025-05-19 15:24_

---

_Comment by @github-actions[bot] on 2025-05-19 15:31_

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

_@MichaReiser approved on 2025-05-19 15:34_

---

_Merged by @dcreager on 2025-05-19 15:40_

---

_Closed by @dcreager on 2025-05-19 15:40_

---

_Branch deleted on 2025-05-19 15:41_

---
