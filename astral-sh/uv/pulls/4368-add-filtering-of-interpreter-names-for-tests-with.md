```yaml
number: 4368
title: Add filtering of interpreter names for tests with multiple Python versions
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/filter-python-versions
created_at: 2024-06-17T19:17:56Z
updated_at: 2024-06-18T12:16:28Z
url: https://github.com/astral-sh/uv/pull/4368
synced_at: 2026-01-12T16:06:11Z
```

# Add filtering of interpreter names for tests with multiple Python versions

---

_@zanieb_

Extends new filters for interpreter paths to apply to tests with multiple Python versions. Adds patch version filtering for them as well, which is needed for #4360 tests.

---

_@zanieb reviewed on 2024-06-17 19:18_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:226 on 2024-06-17 19:18_

Using multiple Python versions is now "first class" so we can support filtering them.

---

_@zanieb reviewed on 2024-06-17 19:19_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:193 on 2024-06-17 19:19_

These need to be in the front or they can get clobbered by other filters. Prompted the change to a double ended data structure.

---

_Marked ready for review by @zanieb on 2024-06-17 19:19_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-17 19:19_

---

_Review requested from @ibraheemdev by @zanieb on 2024-06-17 19:21_

---

_Comment by @zanieb on 2024-06-17 19:21_

This feels kind of sloppy but it's blocking work I want to release today.

---

_Merged by @zanieb on 2024-06-17 20:22_

---

_Closed by @zanieb on 2024-06-17 20:22_

---

_Branch deleted on 2024-06-17 20:22_

---

_@BurntSushi reviewed on 2024-06-18 12:16_

LGTM!

---
