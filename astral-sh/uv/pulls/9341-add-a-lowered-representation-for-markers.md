```yaml
number: 9341
title: Add a lowered representation for markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: charlie/eval
head: charlie/lower
created_at: 2024-11-22T00:24:07Z
updated_at: 2024-11-26T10:00:54Z
url: https://github.com/astral-sh/uv/pull/9341
synced_at: 2026-01-10T12:00:00Z
```

# Add a lowered representation for markers

---

_Pull request opened by @charliermarsh on 2024-11-22 00:24_

## Summary

This PR introduces a set of parallel structs to `MarkerValueString`, `MarkerValueExtra`, and `MarkerValueVersion` that remove various pieces of information and representations that shouldn't be available in the marker algebra. To start, I've _just_ removed the invalid extra component from `MarkerValueExtra` -- there are no other changes to the representation. So, throughout the marker algebra, we can't represent and thus don't have to worry about clauses with invalid extras.

The subsequent changes I plan to make are:

1. Removing `python_version`, since we exclusively use `python_version_full` in the algebra.
2. Removing the deprecated aliases, such that we re-map to the correct marker value.
3. Consolidating `sys_platform` and `platform_release` (the original motivation).


---

_Comment by @charliermarsh on 2024-11-22 00:25_

This does mean that we'll "drop" invalid extras, like `extra == "1abc"` (not a valid package name), rather than retaining them in the output representation.

---

_@charliermarsh reviewed on 2024-11-22 00:25_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/tree.rs`:988 on 2024-11-22 00:25_

Just an example of the simplification that we get in return. This specific case is obviously not a big win, but changing the representation (such that we have an internal representation that's separate from the exact grammar and how they're parsed) is the crucial piece.

---

_Review requested from @konstin by @charliermarsh on 2024-11-22 02:35_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-11-22 02:35_

---

_Label `internal` added by @charliermarsh on 2024-11-22 02:35_

---

_@ibraheemdev approved on 2024-11-24 15:34_

---

_Merged by @charliermarsh on 2024-11-25 03:41_

---

_Closed by @charliermarsh on 2024-11-25 03:41_

---

_Branch deleted on 2024-11-25 03:41_

---

_Branch restored on 2024-11-25 03:42_

---

_@konstin reviewed on 2024-11-26 10:00_

nit: Maybe `ParsedMarker<...>` vs `CanonicalMarlker<...>`?

Code looks good

---
