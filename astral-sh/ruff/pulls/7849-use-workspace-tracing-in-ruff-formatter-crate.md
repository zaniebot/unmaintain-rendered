```yaml
number: 7849
title: "Use workspace `tracing` in `ruff_formatter` crate"
type: pull_request
state: merged
author: cnpryer
labels:
  - internal
assignees: []
merged: true
base: main
head: ruff-formatter-tracing
created_at: 2023-10-07T18:53:44Z
updated_at: 2023-10-08T14:02:35Z
url: https://github.com/astral-sh/ruff/pull/7849
synced_at: 2026-01-12T15:55:25Z
```

# Use workspace `tracing` in `ruff_formatter` crate

---

_@cnpryer_

I noticed that `tracing::instrument` wasn't available with only the `"std"` feature enabled when trying to run `cargo doc -p ruff_formatter`.

I could be misunderstanding something, but I couldn't even run the tests for the crate.

```
ruff on  ruff-formatter-tracing [$] is 📦 v0.0.292 via 🦀 v1.72.0 
❯ cargo test -p ruff_formatter          
   Compiling ruff_formatter v0.0.0 (/Users/chrispryer/github/ruff/crates/ruff_formatter)
error[E0433]: failed to resolve: could not find `instrument` in `tracing`
   --> crates/ruff_formatter/src/printer/mod.rs:57:16
    |
57  |     #[tracing::instrument(name = "Printer::print", skip_all)]
    |                ^^^^^^^^^^ could not find `instrument` in `tracing`
    |
note: found an item that was configured out
   --> /Users/chrispryer/.cargo/registry/src/index.crates.io-6f17d22bba15001f/tracing-0.1.37/src/lib.rs:959:29
    |
959 | pub use tracing_attributes::instrument;
    |                             ^^^^^^^^^^
    = note: the item is gated behind the `attributes` feature

For more information about this error, try `rustc --explain E0433`.
error: could not compile `ruff_formatter` (lib) due to previous error
warning: build failed, waiting for other jobs to finish...
error: could not compile `ruff_formatter` (lib test) due to previous error
```

Maybe the idea is to keep this crate minimal, but I figured I'd at least point this out.

---

_@charliermarsh approved on 2023-10-08 13:50_

Thanks, I'm also confused by this. I know that if you a workspace dependency, and you add features, it's _additive_ on the features enabled at the workspace level, but this is a bit different.

---

_Merged by @charliermarsh on 2023-10-08 13:50_

---

_Closed by @charliermarsh on 2023-10-08 13:50_

---

_Label `internal` added by @charliermarsh on 2023-10-08 13:50_

---

_Branch deleted on 2023-10-08 14:01_

---

_Comment by @cnpryer on 2023-10-08 14:02_

Curious if it's because defaults were disabled (`"attributes"` is a default).

---
