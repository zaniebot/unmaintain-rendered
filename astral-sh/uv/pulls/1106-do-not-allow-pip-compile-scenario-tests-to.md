```yaml
number: 1106
title: "Do not allow `pip compile` scenario tests to discover other Python versions"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/limit-versions
created_at: 2024-01-25T21:42:43Z
updated_at: 2024-01-26T18:18:17Z
url: https://github.com/astral-sh/uv/pull/1106
synced_at: 2026-01-12T16:04:26Z
```

# Do not allow `pip compile` scenario tests to discover other Python versions

---

_@zanieb_

In https://github.com/astral-sh/puffin/pull/1040 we broke the pip compile scenarios designed to test failure when a required Python version is not available — resolution succeeded because all of the Python versions were available in CI. Following #1105 we have the ability to isolate tests from Python versions available in the system. Here, we limit the scenarios to only the Python version in the current environment, restoring our ability to test the error messages.

With https://github.com/zanieb/packse/pull/95, we will be able to specify scenarios with access to additional system Python versions. This will allow us to include test coverage where resolution can succeed by using a version available elsewhere on the system.  See #1111 for this follow-up.

---

_Label `testing` added by @zanieb on 2024-01-25 21:42_

---

_@charliermarsh approved on 2024-01-25 22:27_

---

_Merged by @zanieb on 2024-01-26 18:18_

---

_Closed by @zanieb on 2024-01-26 18:18_

---

_Branch deleted on 2024-01-26 18:18_

---
