```yaml
number: 17580
title: "[red-knot] Early return from `project.is_file_open` for vendored files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/is-file-open
created_at: 2025-04-23T12:15:30Z
updated_at: 2025-04-23T13:32:42Z
url: https://github.com/astral-sh/ruff/pull/17580
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Early return from `project.is_file_open` for vendored files

---

_@MichaReiser_

This is partially addresses https://github.com/astral-sh/ty/issues/114

It avoids reading `files` or `open_file_set`, which both have low durability for typeshed files. This prevents downgrading typeshed queries to durability low if we happen to emit a diagnostic in them. 

Ideally, we'd also short-circuit for third-party files but it's less clear to me how we would do that while supporting open-file-set (we could check the `included_paths`). That's why I decided to only optimize this for typeshed for now. 

The incremental benchmarks improve by 2% which suggests that we try to emit diagnostics for at least some typeshed files.

---

_Label `red-knot` added by @AlexWaygood on 2025-04-23 12:17_

---

_Comment by @github-actions[bot] on 2025-04-23 12:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-04-23 13:01_

---

_Review requested from @carljm by @MichaReiser on 2025-04-23 13:01_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-23 13:01_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-23 13:01_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-23 13:01_

---

_@AlexWaygood reviewed on 2025-04-23 13:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_project/src/lib.rs`:322 on 2025-04-23 13:05_

Why `true`? I thought the "open" files were the files that we were directly checking (and therefore emitting diagnostics on)? Shouldn't we be returning `false` here for vendored stub files, since we don't want to emit any diagnostics on those?

---

_@AlexWaygood reviewed on 2025-04-23 13:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_project/src/lib.rs`:322 on 2025-04-23 13:10_

(Apologies if I'm misunderstanding/misremembering what the "open" files are meant to represent!)

---

_@MichaReiser reviewed on 2025-04-23 13:13_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:322 on 2025-04-23 13:13_

Whoops yeah. That's what you get for trying to do 3 things at the same time. Sorry

---

_@AlexWaygood reviewed on 2025-04-23 13:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_project/src/lib.rs`:322 on 2025-04-23 13:25_

Whoop whoop, now it's a 3% speedup on the incremental benchmark and 1% on the cold benchmark! https://codspeed.io/astral-sh/ruff/branches/micha%2Fis-file-open

---

_@AlexWaygood approved on 2025-04-23 13:25_

---

_Renamed from "[red-knot] Try to early return from `project.is_file_open`" to "[red-knot] Early return from `project.is_file_open` for vendored files" by @MichaReiser on 2025-04-23 13:25_

---

_Merged by @MichaReiser on 2025-04-23 13:32_

---

_Closed by @MichaReiser on 2025-04-23 13:32_

---

_Branch deleted on 2025-04-23 13:32_

---
