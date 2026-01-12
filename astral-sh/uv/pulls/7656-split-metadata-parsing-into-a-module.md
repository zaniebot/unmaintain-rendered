```yaml
number: 7656
title: Split metadata parsing into a module
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/split-metadata-parsing
created_at: 2024-09-24T11:51:52Z
updated_at: 2024-09-24T15:25:51Z
url: https://github.com/astral-sh/uv/pull/7656
synced_at: 2026-01-12T16:07:56Z
```

# Split metadata parsing into a module

---

_@konstin_

In preparation to vendoring Metadata23 from https://github.com/PyO3/python-pkginfo-rs/blob/main/src/metadata.rs for https://github.com/astral-sh/uv/pull/7475#pullrequestreview-2323104398 .

The logic itself stayed the same, I split the code into modules and renamed `Metadata23` to `MetadataResolver` to free `Metadata23` for the full core metadata 2.3 we use for uploading.

---

_Renamed from "Split metadata parsing" to "Split metadata parsing into a module" by @konstin on 2024-09-24 11:54_

---

_Review requested from @charliermarsh by @konstin on 2024-09-24 15:15_

---

_Comment by @konstin on 2024-09-24 15:16_

Since this is blocking the whole stack, I'll merge this now and follow up if needed.

---

_Merged by @konstin on 2024-09-24 15:16_

---

_Closed by @konstin on 2024-09-24 15:16_

---

_Branch deleted on 2024-09-24 15:16_

---

_@charliermarsh reviewed on 2024-09-24 15:19_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/metadata/metadata_resolver.rs`:24 on 2024-09-24 15:19_

Why is this one not called Metadata 2.3?

---

_@konstin reviewed on 2024-09-24 15:25_

---

_Review comment by @konstin on `crates/pypi-types/src/metadata/metadata_resolver.rs`:24 on 2024-09-24 15:25_

I'm adding a `Metadata23` (the full one) in the follow-up PR.

---
