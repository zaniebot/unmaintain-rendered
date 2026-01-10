```yaml
number: 14102
title: "Turn off `clippy::struct_excessive_bools` rule"
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/turn-off-excessive-bools-rule
created_at: 2025-06-17T09:10:14Z
updated_at: 2025-06-17T10:18:55Z
url: https://github.com/astral-sh/uv/pull/14102
synced_at: 2026-01-10T11:10:42Z
```

# Turn off `clippy::struct_excessive_bools` rule

---

_Pull request opened by @jtfmumm on 2025-06-17 09:10_

We always ignore the `clippy::struct_excessive_bools` rule and formerly annotated this at the function level. This PR specifies the allow in `workspace.lints.clippy` in `Cargo.toml`.



---

_Label `internal` added by @jtfmumm on 2025-06-17 09:10_

---

_Renamed from "Add `struct_excessive_bools = "allow"` to clippy workspace lint config" to "Turn off `clippy::struct_excessive_bools` rule" by @jtfmumm on 2025-06-17 09:11_

---

_@konstin approved on 2025-06-17 09:19_

---

_Merged by @jtfmumm on 2025-06-17 10:18_

---

_Closed by @jtfmumm on 2025-06-17 10:18_

---

_Branch deleted on 2025-06-17 10:18_

---
