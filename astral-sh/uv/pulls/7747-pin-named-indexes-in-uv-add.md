```yaml
number: 7747
title: "Pin named indexes in `uv add`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/index-api-uv-pin
created_at: 2024-09-27T19:45:05Z
updated_at: 2024-10-15T23:22:46Z
url: https://github.com/astral-sh/uv/pull/7747
synced_at: 2026-01-10T12:53:54Z
```

# Pin named indexes in `uv add`

---

_Pull request opened by @charliermarsh on 2024-09-27 19:45_

## Summary

This PR adds an index pin with `uv add` when the user provides exactly one named index. We don't pin if the user provides an unnamed index, or if they provide multiple indexes.

We probably _could_ pin on multiple indexes by writing the sources _after_ resolution, if that's desirable. But we have no idea which index the user _expects_ each package to come from.

Possible extensions:

- `uv add --no-pin` to avoid this pinning.
- Warn if they provide a single, unnamed index? I'm not sure if that's worth a warn. Open to input.

---

_Label `enhancement` added by @charliermarsh on 2024-09-27 19:45_

---

_Label `cli` added by @charliermarsh on 2024-09-27 19:45_

---

_Marked ready for review by @charliermarsh on 2024-09-27 19:45_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-28 19:02_

---

_Review requested from @konstin by @charliermarsh on 2024-09-28 19:02_

---

_@zanieb reviewed on 2024-09-30 20:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:370 on 2024-09-30 20:00_

Can we add an `inspect` debug log?

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:5284 on 2024-09-30 20:03_

This is a little complicated in this test case because we're in the example "Adding a subsequent index with the same name should replace it."

Is it worth having a simpler test case that's not replacing an existing index at the same time?

---

_@zanieb reviewed on 2024-09-30 20:03_

---

_@konstin approved on 2024-10-01 06:54_

---

_Merged by @charliermarsh on 2024-10-15 23:22_

---

_Closed by @charliermarsh on 2024-10-15 23:22_

---

_Branch deleted on 2024-10-15 23:22_

---
