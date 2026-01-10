```yaml
number: 15408
title: Upgrade Rust toolchain to 1.84.0
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: rust-184
created_at: 2025-01-10T19:51:39Z
updated_at: 2025-01-11T08:52:00Z
url: https://github.com/astral-sh/ruff/pull/15408
synced_at: 2026-01-10T20:34:00Z
```

# Upgrade Rust toolchain to 1.84.0

---

_Pull request opened by @MichaReiser on 2025-01-10 19:51_

## Summary

Upgrade the rust toolchain to 1.84.0. This PR does not bump the MSRV.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-01-10 19:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-10 19:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-10 19:51_

---

_Label `internal` added by @MichaReiser on 2025-01-10 19:51_

---

_@MichaReiser reviewed on 2025-01-10 19:59_

---

_Review comment by @MichaReiser on `Cargo.toml`:215 on 2025-01-10 19:59_

```
    Checking ruff v0.9.1 (/Users/micha/astral/ruff/crates/ruff)
warning: allocating a local array larger than 16384 bytes
  |
  = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#large_stack_arrays
  = note: `-W clippy::large-stack-arrays` implied by `-W clippy::pedantic`
  = help: to override `-W clippy::pedantic` add `#[allow(clippy::large_stack_arrays)]`

warning: `ruff_linter` (lib test) generated 1 warning
```

but `ruff_linter` is kind of big.

I subscribed to the issue

---

_Comment by @github-actions[bot] on 2025-01-10 20:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_@carljm approved on 2025-01-10 20:26_

---

_@AlexWaygood approved on 2025-01-10 20:54_

---

_Merged by @MichaReiser on 2025-01-11 08:51_

---

_Closed by @MichaReiser on 2025-01-11 08:51_

---

_Branch deleted on 2025-01-11 08:52_

---
