```yaml
number: 19052
title: "Make dependency `get-size2` truly optional in `ruff_python_ast`"
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
assignees: []
merged: true
base: main
head: get-size-feat
created_at: 2025-06-30T19:38:30Z
updated_at: 2025-07-01T02:50:59Z
url: https://github.com/astral-sh/ruff/pull/19052
synced_at: 2026-01-12T15:56:30Z
```

# Make dependency `get-size2` truly optional in `ruff_python_ast`

---

_@dylwil3_

Gates all uses of `get-size2` behind the feature `get-size` in the crate `ruff_python_ast`. Also requires that `ruff_text_size` is pulled in with the feature `get-size` enabled if we enable the same-named feature for `ruff_python_ast`.


---

_Label `internal` added by @dylwil3 on 2025-06-30 19:38_

---

_Renamed from "Enables get-size feature on `ruff_text_size` when enabled on `ruff_python_ast`" to "Enable get-size feature on `ruff_text_size` when enabled on `ruff_python_ast`" by @dylwil3 on 2025-06-30 19:38_

---

_Renamed from "Enable get-size feature on `ruff_text_size` when enabled on `ruff_python_ast`" to "Make dependency `get-size2` truly optional in `ruff_python_ast`" by @dylwil3 on 2025-06-30 19:46_

---

_Comment by @github-actions[bot] on 2025-06-30 19:56_

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

_@ntBre approved on 2025-06-30 21:48_

Thanks! I confirmed I can now run `cargo test -p ruff_python_ast` where I could not on `main`.

---

_@ibraheemdev approved on 2025-06-30 23:57_

I'm a little surprised we don't have a CI job that catches this, but I guess it's not that high of a priority.

---

_Merged by @dylwil3 on 2025-07-01 02:50_

---

_Closed by @dylwil3 on 2025-07-01 02:50_

---
