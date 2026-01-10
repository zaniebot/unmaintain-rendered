```yaml
number: 5422
title: Normalize marker expression order
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
assignees: []
merged: true
base: main
head: ibraheem/marker-smart-constructors
created_at: 2024-07-24T20:13:52Z
updated_at: 2024-08-23T15:51:58Z
url: https://github.com/astral-sh/uv/pull/5422
synced_at: 2026-01-10T13:09:50Z
```

# Normalize marker expression order

---

_Pull request opened by @ibraheemdev on 2024-07-24 20:13_

## Summary

Normalize the order of marker expressions on construction. This removes the distinction between expressions like `os_name == 'Linux'` vs. `'Linux' == os_name` throughout the codebase. One caveat here is that the `in` operator does not have a direct inverse, so we introduce `MarkerOperator::Contains` to handle that case.

I wanted to land this smaller change before some more intrusive changes as it simplifies the existing code quite a bit.

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-07-24 20:13_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-24 20:13_

---

_Label `internal` added by @ibraheemdev on 2024-07-24 20:14_

---

_@charliermarsh approved on 2024-07-24 20:45_

---

_Merged by @ibraheemdev on 2024-07-24 22:28_

---

_Closed by @ibraheemdev on 2024-07-24 22:28_

---

_Branch deleted on 2024-07-24 22:28_

---

_@konstin reviewed on 2024-07-25 09:19_

I like it!

---

_@BurntSushi reviewed on 2024-07-25 11:47_

Yes.

---
