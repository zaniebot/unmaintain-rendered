```yaml
number: 20686
title: Update benchmarking CI for cargo-codspeed v4
type: pull_request
state: merged
author: ntBre
labels:
  - ci
assignees: []
merged: true
base: main
head: brent/codspeed-v4
created_at: 2025-10-02T18:25:30Z
updated_at: 2025-10-02T18:56:29Z
url: https://github.com/astral-sh/ruff/pull/20686
synced_at: 2026-01-12T15:57:07Z
```

# Update benchmarking CI for cargo-codspeed v4

---

_@ntBre_

Summary
--

A new codspeed [release](https://github.com/CodSpeedHQ/codspeed-rust/releases/tag/v4.0.0) came out, and our CI was failing. It looks like the
previously-positional list of benchmarks now corresponds to a `--bench` [flag](https://github.com/CodSpeedHQ/codspeed-rust/blob/7159cf86b9432243565e488c827c1663b5385fde/crates/cargo-codspeed/src/app.rs#L39-L43).

Test Plan
--

CI on this PR

## TODO

- [x] Drop whitespace change commit, just wanted to make sure the benchmarks ran

---

_Label `ci` added by @ntBre on 2025-10-02 18:26_

---

_Comment by @github-actions[bot] on 2025-10-02 18:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-02 18:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dylwil3 reviewed on 2025-10-02 18:31_

---

_Review comment by @dylwil3 on `crates/ruff/tests/lint.rs`:1 on 2025-10-02 18:31_

should this and the below be part of the diff? did `rustfmt` change or something?

---

_@ntBre reviewed on 2025-10-02 18:33_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:1 on 2025-10-02 18:33_

I need to drop that commit, I just wanted to make sure the benchmarks would actually run by making some random changes

---

_Marked ready for review by @ntBre on 2025-10-02 18:39_

---

_Review requested from @carljm by @ntBre on 2025-10-02 18:39_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-02 18:39_

---

_Review requested from @sharkdp by @ntBre on 2025-10-02 18:39_

---

_Review requested from @dcreager by @ntBre on 2025-10-02 18:39_

---

_Review request for @dcreager removed by @ntBre on 2025-10-02 18:39_

---

_Review request for @carljm removed by @ntBre on 2025-10-02 18:39_

---

_Review request for @sharkdp removed by @ntBre on 2025-10-02 18:39_

---

_@dylwil3 approved on 2025-10-02 18:40_

LGTM once the whitespace is removed - thanks for figuring this out!

---

_Comment by @github-actions[bot] on 2025-10-02 18:42_

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

_Merged by @ntBre on 2025-10-02 18:47_

---

_Closed by @ntBre on 2025-10-02 18:47_

---

_Branch deleted on 2025-10-02 18:47_

---
