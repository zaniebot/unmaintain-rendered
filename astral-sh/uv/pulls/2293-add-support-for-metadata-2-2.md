```yaml
number: 2293
title: Add support for Metadata 2.2
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/metadata-2.2
created_at: 2024-03-08T02:51:03Z
updated_at: 2024-03-08T16:02:33Z
url: https://github.com/astral-sh/uv/pull/2293
synced_at: 2026-01-12T16:04:57Z
```

# Add support for Metadata 2.2

---

_@charliermarsh_

## Summary

PyPI now supports Metadata 2.2, which means distributions with Metadata 2.2-compliant metadata will start to appear. The upside is that if a source distribution includes a `PKG-INFO` file with (1) a metadata version of 2.2 or greater, and (2) no dynamic fields (at least, of the fields we rely on), we can read the metadata from the `PKG-INFO` file directly rather than running _any_ of the PEP 517 build hooks.

Closes https://github.com/astral-sh/uv/issues/2009.


---

_Comment by @charliermarsh on 2024-03-08 03:10_

This works for everything except local, archived distributions. Want to do one refactor on `main` that should make those "just work" here too.

---

_Review requested from @konstin by @charliermarsh on 2024-03-08 04:03_

---

_Marked ready for review by @charliermarsh on 2024-03-08 04:04_

---

_Label `enhancement` added by @charliermarsh on 2024-03-08 04:05_

---

_Review comment by @konstin on `crates/pypi-types/src/metadata.rs`:80 on 2024-03-08 09:59_

These should be searched and replaced with metadata23 with a comment that that's the maximum version we support.

---

_Review comment by @konstin on `crates/pypi-types/src/metadata.rs`:143 on 2024-03-08 10:01_

We need to split, parse and check for >=2.2,<3, otherwise uv breaks when there's a metadata update

---

_Review comment by @konstin on `crates/uv-distribution/src/source/mod.rs`:989 on 2024-03-08 10:03_

Could you also add a debug log here, so users running with `-v` can figure out why this source dist takes the slow path

---

_@konstin approved on 2024-03-08 10:05_

yesss

---

_Merged by @charliermarsh on 2024-03-08 16:02_

---

_Closed by @charliermarsh on 2024-03-08 16:02_

---

_Branch deleted on 2024-03-08 16:02_

---
