```yaml
number: 2993
title: "Support unnamed requirements in `--require-hashes`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/package-id
created_at: 2024-04-11T14:29:15Z
updated_at: 2024-04-11T15:26:51Z
url: https://github.com/astral-sh/uv/pull/2993
synced_at: 2026-01-12T16:05:21Z
```

# Support unnamed requirements in `--require-hashes`

---

_@charliermarsh_

## Summary

This PR enables `--require-hashes` with unnamed requirements. The key change is that `PackageId` becomes `VersionId` (since it refers to a package at a specific version), and the new `PackageId` consists of _either_ a package name _or_ a URL. The hashes are keyed by `PackageId`, so we can generate the `RequiredHashes` before we have names for all packages, and enforce them throughout.

Closes #2979.

---

_Label `enhancement` added by @charliermarsh on 2024-04-11 14:29_

---

_Marked ready for review by @charliermarsh on 2024-04-11 14:29_

---

_@charliermarsh reviewed on 2024-04-11 14:33_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:1105 on 2024-04-11 14:33_

Note-to-self, this seems wrong.

---

_Merged by @charliermarsh on 2024-04-11 15:26_

---

_Closed by @charliermarsh on 2024-04-11 15:26_

---

_Branch deleted on 2024-04-11 15:26_

---
