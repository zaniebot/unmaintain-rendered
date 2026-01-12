```yaml
number: 606
title: Impl Version debug as display
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: impl-version-debug-as-display
created_at: 2023-12-11T12:48:43Z
updated_at: 2023-12-11T15:38:16Z
url: https://github.com/astral-sh/uv/pull/606
synced_at: 2026-01-12T16:04:04Z
```

# Impl Version debug as display

---

_@konstin_

Currently, `dbg!` is hard to read because versions are verbose, showing all optional fields, and we have a lot of versions. Changing debug formatting to displaying the version number (which can be losslessly converted to the struct and back) makes this more readable.

See e.g. https://gist.github.com/konstin/38c0f32b109dffa73b3aa0ab86b9662b

**Before**

```text
version: Version {
    epoch: 0,
    release: [
        1,
        2,
        3,
    ],
    pre: None,
    post: None,
    dev: None,
    local: None,
},
```

**After**

```text
version: "1.2.3",
```

---

_@charliermarsh approved on 2023-12-11 15:13_

---

_@zanieb approved on 2023-12-11 15:19_

---

_@BurntSushi approved on 2023-12-11 15:23_

---

_Merged by @konstin on 2023-12-11 15:38_

---

_Closed by @konstin on 2023-12-11 15:38_

---

_Branch deleted on 2023-12-11 15:38_

---
