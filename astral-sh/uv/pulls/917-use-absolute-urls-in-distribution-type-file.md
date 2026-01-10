```yaml
number: 917
title: "Use absolute urls in `distribution_type::File`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/absolute-url-in-file
created_at: 2024-01-14T17:00:35Z
updated_at: 2024-01-14T17:15:25Z
url: https://github.com/astral-sh/uv/pull/917
synced_at: 2026-01-10T15:39:02Z
```

# Use absolute urls in `distribution_type::File`

---

_Pull request opened by @konstin on 2024-01-14 17:00_

Previously, the url on file could either be a relative or an absolute url, depending on the index, and we would finalize it lazily. Now we finalize the url when converting `pypi_types::File` to `distribution_types::File`. This change is required to make the hashes on `File` optional (https://github.com/astral-sh/puffin/pull/910), which are currently the only unique field usable for caching.

---

_Label `internal` added by @konstin on 2024-01-14 17:00_

---

_@konstin reviewed on 2024-01-14 17:01_

---

_Review comment by @konstin on `crates/distribution-types/src/file.rs`:33 on 2024-01-14 17:01_

Should we change this name?

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:12 on 2024-01-14 17:04_

Can we format this as `Requires-Python` or `'Requires-Python'`?

---

_@charliermarsh approved on 2024-01-14 17:05_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:33 on 2024-01-14 17:05_

I think it's ok.

---

_@charliermarsh reviewed on 2024-01-14 17:05_

---

_@charliermarsh reviewed on 2024-01-14 17:06_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:27 on 2024-01-14 17:06_

This is maybe why I didn't do this initially, it means we have to parse all URLs upfront which is gonna be unnecessary work for the majority of files.

---

_@konstin reviewed on 2024-01-14 17:14_

---

_Review comment by @konstin on `crates/distribution-types/src/file.rs`:12 on 2024-01-14 17:14_

Going with `requires-python` from [PEP 621](https://peps.python.org/pep-0621/#requires-python), which is the same name as used in the simple json api.

---

_Merged by @konstin on 2024-01-14 17:15_

---

_Closed by @konstin on 2024-01-14 17:15_

---

_Branch deleted on 2024-01-14 17:15_

---
