```yaml
number: 3569
title: Fix install_registry_source_dist_cached on Gentoo
type: pull_request
state: merged
author: mgorny
labels:
  - testing
assignees: []
merged: true
base: main
head: dist_cached-test-inc
created_at: 2024-05-14T05:45:44Z
updated_at: 2024-05-14T18:27:22Z
url: https://github.com/astral-sh/uv/pull/3569
synced_at: 2026-01-10T14:37:54Z
```

# Fix install_registry_source_dist_cached on Gentoo

---

_Pull request opened by @mgorny on 2024-05-14 05:45_

## Summary

Increment the removed file counts in filters
in install_registry_source_dist_cached test, to make it work again on Gentoo.  The tested counts were updated
in 9a92a3ad3771a78d36214aa958f51bf7bb992f80, but the filters were not. That said, the respective count increased in Gentoo as well, so adjust both input and output strings.  I'm updating Windows as a guesswork, though I suspect that filter may not be necessary anymore, given that CI was passing.

## Test Plan

`cargo test` on Gentoo :-).

---

_Review requested from @charliermarsh by @konstin on 2024-05-14 17:39_

---

_@charliermarsh approved on 2024-05-14 17:51_

---

_Comment by @charliermarsh on 2024-05-14 17:51_

Thank you so much, I'm sorry about that.

---

_Label `testing` added by @charliermarsh on 2024-05-14 17:51_

---

_Merged by @charliermarsh on 2024-05-14 17:51_

---

_Closed by @charliermarsh on 2024-05-14 17:51_

---

_Branch deleted on 2024-05-14 18:27_

---

_Comment by @mgorny on 2024-05-14 18:27_

Thanks!

---
