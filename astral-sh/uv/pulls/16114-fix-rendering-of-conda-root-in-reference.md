```yaml
number: 16114
title: "Fix rendering of `_CONDA_ROOT` in reference"
type: pull_request
state: merged
author: Gankra
labels:
  - documentation
assignees: []
merged: true
base: main
head: gankra/fixdoc
created_at: 2025-10-03T13:37:41Z
updated_at: 2025-10-03T15:38:12Z
url: https://github.com/astral-sh/uv/pull/16114
synced_at: 2026-01-12T16:12:06Z
```

# Fix rendering of `_CONDA_ROOT` in reference

---

_@Gankra_

All other references are correct, just slipped through in the actual docs.

---

_Label `documentation` added by @Gankra on 2025-10-03 13:37_

---

_Comment by @zanieb on 2025-10-03 13:46_

This file is generated

---

_Comment by @Gankra on 2025-10-03 14:13_

Wait why isn't the CI complaining about the generation being out of sync >.>;

(Will fix the generator)

---

_Comment by @Gankra on 2025-10-03 14:20_

Just renaming the var is the simplest, pushed that up in case it's acceptable (the "right" fix is to have the macro use the string, which I'm looking at.)

---

_Comment by @Gankra on 2025-10-03 14:34_

Fixing the macro was straightforward, so, here's that version.

---

_@zanieb approved on 2025-10-03 15:37_

ty

sort of annoying it sorts including the `_` but whatever haha

---

_Merged by @zanieb on 2025-10-03 15:38_

---

_Closed by @zanieb on 2025-10-03 15:38_

---

_Branch deleted on 2025-10-03 15:38_

---

_Renamed from "Fix typo in `_CONDA_ROOT` docs" to "Fix rendering of `_CONDA_ROOT` in reference" by @zanieb on 2025-10-03 15:38_

---
