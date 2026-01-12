```yaml
number: 9096
title: add support for conflicting groups
type: pull_request
state: merged
author: BurntSushi
labels:
  - resolver
assignees: []
merged: true
base: main
head: ag/conflicts
created_at: 2024-11-13T19:43:40Z
updated_at: 2024-11-23T15:59:51Z
url: https://github.com/astral-sh/uv/pull/9096
synced_at: 2026-01-12T16:08:39Z
```

# add support for conflicting groups

---

_@BurntSushi_

This PR adds support for conflicting groups. This is a follow-up PR to
#8976, which added support for conflicting extras.

Most of this PR is actually refactoring the "conflicting" types to
support both extras and groups. The actual change to make conflicting
groups work with the resolver is comparatively small. No changes to the
forking logic are made.

This PR also exposes `conflicts` in `pyproject.toml`.

Closes #6981


---

_Review requested from @konstin by @BurntSushi on 2024-11-13 19:51_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-13 19:51_

---

_Review requested from @zanieb by @BurntSushi on 2024-11-13 19:52_

---

_@charliermarsh approved on 2024-11-13 19:53_

---

_Merged by @BurntSushi on 2024-11-14 13:02_

---

_Closed by @BurntSushi on 2024-11-14 13:02_

---

_Branch deleted on 2024-11-14 13:02_

---

_Label `resolver` added by @BurntSushi on 2024-11-23 15:59_

---
