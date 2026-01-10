```yaml
number: 7808
title: "Fix `uv tree --invert` for platform dependencies"
type: pull_request
state: merged
author: j178
labels:
  - bug
assignees: []
merged: true
base: main
head: tree-marker
created_at: 2024-09-30T12:57:27Z
updated_at: 2024-10-03T02:40:27Z
url: https://github.com/astral-sh/uv/pull/7808
synced_at: 2026-01-10T12:53:56Z
```

# Fix `uv tree --invert` for platform dependencies

---

_Pull request opened by @j178 on 2024-09-30 12:57_

## Summary

`click` has one dependency of `colorama` only on Windows, `uv tree --invert` should not include `colorama` on non-Windows platforms, but currently:

```console
$ uv init
$ uv add click
$ uv tree --invert --python-platform macos
colorama v0.4.6
```

it should:
```console
$ uv tree --invert --python-platform macos
click v8.1.7
    └── project v0.1.0
```


---

_Assigned to @charliermarsh by @zanieb on 2024-09-30 13:51_

---

_Review requested from @charliermarsh by @zanieb on 2024-09-30 13:51_

---

_Label `bug` added by @zanieb on 2024-09-30 13:51_

---

_@charliermarsh reviewed on 2024-09-30 16:26_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/tree.rs`:137 on 2024-09-30 16:26_

Should we just add these to `non_roots` instead of `skipped`? Does it have the same effect?

---

_@charliermarsh approved on 2024-09-30 16:37_

---

_Comment by @charliermarsh on 2024-09-30 16:37_

Thank you!

---

_Merged by @charliermarsh on 2024-09-30 16:45_

---

_Closed by @charliermarsh on 2024-09-30 16:45_

---

_Branch deleted on 2024-10-03 02:40_

---
