```yaml
number: 3401
title: "refactor: Extract `ruff_wasm`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor/extract-ruff-wasm
created_at: 2023-03-08T11:04:33Z
updated_at: 2023-03-09T10:07:41Z
url: https://github.com/astral-sh/ruff/pull/3401
synced_at: 2026-01-12T04:39:44Z
```

# refactor: Extract `ruff_wasm`

---

_Pull request opened by @MichaReiser on 2023-03-08 11:04_

This PR extracts the wasm bindings into a new `ruff_wasm` crate.

My motivation for this refactor is that IDEs struggle with conditionally compiled code. For example, CLion doesn't provide any code hints because the `lib_wasm.rs` file isn't part of a project. In my view, having a separate crate also simplifies the configuration because there are fewer conditional dependencies (only wasm dependencies).

 Moving the code into its own crate showed that the tests no longer compiled (we never exercised the wasm tests path).

---

_@MichaReiser reviewed on 2023-03-08 11:05_

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:169 on 2023-03-08 11:05_

This is static code that does not need to be generated.

---

_@MichaReiser reviewed on 2023-03-08 11:07_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/src/lib.rs`:54 on 2023-03-08 11:07_

Is there a way to deserialize a noqa code back to a rule? Although I'm not sure if that's important.

---

_Marked ready for review by @MichaReiser on 2023-03-08 11:28_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-08 11:29_

---

_Review comment by @charliermarsh on `crates/ruff_wasm/src/lib.rs`:54 on 2023-03-08 14:12_

No, I don't believe so, not right now at least.

---

_@charliermarsh reviewed on 2023-03-08 14:12_

---

_@charliermarsh approved on 2023-03-08 17:56_

Great change.

---

_Label `internal` added by @MichaReiser on 2023-03-09 09:59_

---

_Merged by @MichaReiser on 2023-03-09 10:07_

---

_Closed by @MichaReiser on 2023-03-09 10:07_

---

_Branch deleted on 2023-03-09 10:07_

---
