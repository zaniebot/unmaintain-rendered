```yaml
number: 14350
title: "Tests for `without_trailing_slash` do not assert if borrowed"
type: issue
state: closed
author: zanieb
labels:
  - testing
assignees: []
created_at: 2025-06-29T15:54:32Z
updated_at: 2025-07-01T18:39:19Z
url: https://github.com/astral-sh/uv/issues/14350
synced_at: 2026-01-12T16:01:47Z
```

# Tests for `without_trailing_slash` do not assert if borrowed

---

_@zanieb_

The tests for `without_trailing_slash` appear to assert that the string is borrowed or owned, but the test passes regardless of which variant is used because equality is not enforced on the outer type. This is misleading, and we should probably fix the test cases?

---

_Label `testing` added by @zanieb on 2025-06-29 15:54_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-07-01 12:40_

---

_Closed by @zanieb on 2025-07-01 18:39_

---
