```yaml
number: 1101
title: Set buffer size when unzipping
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/size
created_at: 2024-01-25T17:56:48Z
updated_at: 2024-01-26T14:25:35Z
url: https://github.com/astral-sh/uv/pull/1101
synced_at: 2026-01-12T16:04:25Z
```

# Set buffer size when unzipping

---

_@charliermarsh_

The zip archive includes an uncompressed size header, which we can use to preallocate.

---

_Label `performance` added by @charliermarsh on 2024-01-25 17:57_

---

_Merged by @charliermarsh on 2024-01-25 17:58_

---

_Closed by @charliermarsh on 2024-01-25 17:58_

---

_Branch deleted on 2024-01-25 17:58_

---

_@konstin reviewed on 2024-01-26 13:15_

---

_Review comment by @konstin on `crates/puffin-extract/src/lib.rs`:54 on 2024-01-26 13:15_

Is it correct to create a buf writer with `usize::MAX`?

---

_@charliermarsh reviewed on 2024-01-26 13:29_

---

_Review comment by @charliermarsh on `crates/puffin-extract/src/lib.rs`:54 on 2024-01-26 13:29_

I thought this case implied that the size was larger than `usize`? I will conservatively _not_ set a capacity instead.

---

_@charliermarsh reviewed on 2024-01-26 14:25_

---

_Review comment by @charliermarsh on `crates/puffin-extract/src/lib.rs`:54 on 2024-01-26 14:25_

https://github.com/astral-sh/puffin/pull/1123

---
