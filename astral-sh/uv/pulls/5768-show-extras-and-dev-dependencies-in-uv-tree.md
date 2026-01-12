```yaml
number: 5768
title: "Show extras and dev dependencies in `uv tree`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/tree-extra
created_at: 2024-08-04T21:09:14Z
updated_at: 2024-08-05T19:15:03Z
url: https://github.com/astral-sh/uv/pull/5768
synced_at: 2026-01-12T16:07:00Z
```

# Show extras and dev dependencies in `uv tree`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5759.


---

_Label `enhancement` added by @charliermarsh on 2024-08-04 21:09_

---

_Label `preview` added by @charliermarsh on 2024-08-04 21:09_

---

_@charliermarsh reviewed on 2024-08-04 21:09_

---

_Review comment by @charliermarsh on `crates/uv/tests/tree.rs`:500 on 2024-08-04 21:09_

This means: `flask[dotenv]` was a dependency.

---

_@charliermarsh reviewed on 2024-08-04 21:09_

---

_Review comment by @charliermarsh on `crates/uv/tests/tree.rs`:509 on 2024-08-04 21:09_

This means: `python-dotenv` was included due to the `dotenv` extra.

---

_@charliermarsh reviewed on 2024-08-04 21:10_

---

_Review comment by @charliermarsh on `crates/uv/tests/tree.rs`:553 on 2024-08-04 21:10_

The inverted form is kind of bad, the annotations are sort of on the wrong node, but it's honestly really hard to improve. I considered omitting them altogether.

---

_Review requested from @konstin by @charliermarsh on 2024-08-04 21:12_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-04 21:12_

---

_@charliermarsh reviewed on 2024-08-04 21:15_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:2667 on 2024-08-04 21:15_

Honestly wondering if `--invert` should have a totally separate impl.

---

_@konstin approved on 2024-08-05 08:50_

---

_Merged by @charliermarsh on 2024-08-05 19:15_

---

_Closed by @charliermarsh on 2024-08-05 19:15_

---

_Branch deleted on 2024-08-05 19:15_

---
