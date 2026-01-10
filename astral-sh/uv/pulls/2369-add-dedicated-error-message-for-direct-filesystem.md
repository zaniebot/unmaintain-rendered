```yaml
number: 2369
title: Add dedicated error message for direct filesystem paths in requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/p
created_at: 2024-03-11T22:19:19Z
updated_at: 2024-03-11T22:45:13Z
url: https://github.com/astral-sh/uv/pull/2369
synced_at: 2026-01-10T14:49:08Z
```

# Add dedicated error message for direct filesystem paths in requirements

---

_Pull request opened by @charliermarsh on 2024-03-11 22:19_

## Summary

This is analogous to #669, but for cases in which the package name is a filesystem path. In such cases, we'll fail when parsing the _package name_, since it doesn't start with a valid character, as opposed to failing when we go to parse the remaining version specifier.

Inspired by https://github.com/astral-sh/uv/issues/2356.


---

_Label `error messages` added by @charliermarsh on 2024-03-11 22:20_

---

_Marked ready for review by @charliermarsh on 2024-03-11 22:20_

---

_Review comment by @konstin on `crates/pep508-rs/src/lib.rs`:778 on 2024-03-11 22:42_

Does this support `pip install path/to/my.whl`? I suspect that this is a big fraction of the cases

---

_@konstin approved on 2024-03-11 22:42_

---

_@charliermarsh reviewed on 2024-03-11 22:45_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:778 on 2024-03-11 22:45_

Yeah, that already works on main, because “path” is parsed as the package name, then we fail on the slash. The case covered in this PR is when there’s no leasing alpha character (like “./path”).

---

_Merged by @charliermarsh on 2024-03-11 22:45_

---

_Closed by @charliermarsh on 2024-03-11 22:45_

---

_Branch deleted on 2024-03-11 22:45_

---
