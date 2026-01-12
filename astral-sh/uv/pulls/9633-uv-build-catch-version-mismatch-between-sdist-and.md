```yaml
number: 9633
title: "uv build: Catch version mismatch between sdist and wheel "
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/uv-build-catch-invalid-filename
created_at: 2024-12-04T09:37:38Z
updated_at: 2024-12-04T14:21:03Z
url: https://github.com/astral-sh/uv/pull/9633
synced_at: 2026-01-12T16:08:54Z
```

# uv build: Catch version mismatch between sdist and wheel 

---

_@konstin_

When building a wheel from a source distribution or both a source distribution and a wheel, the versions in their filenames must be the same.

By inspecting the filenames, we also assert that the filenames from the build a valid (we don't enforce normalization though, just that uv can parse them).

Note that we're not yet checking that also the `pyproject.toml` version, if declared, and METADATA version matches.

---

_Label `enhancement` added by @konstin on 2024-12-04 09:37_

---

_Review requested from @charliermarsh by @konstin on 2024-12-04 09:37_

---

_Review comment by @konstin on `crates/uv/src/commands/build_frontend.rs`:86 on 2024-12-04 09:38_

Add some extra context because it's not the input filename that's wrong, it's the output filename

---

_@konstin reviewed on 2024-12-04 09:38_

---

_@charliermarsh approved on 2024-12-04 14:17_

---

_Merged by @konstin on 2024-12-04 14:21_

---

_Closed by @konstin on 2024-12-04 14:21_

---

_Branch deleted on 2024-12-04 14:21_

---
