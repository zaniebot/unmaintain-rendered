```yaml
number: 1605
title: Update the scenarios to use vendored build dependencies
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/find-links-scenarios
created_at: 2024-02-17T17:54:59Z
updated_at: 2024-02-19T21:55:04Z
url: https://github.com/astral-sh/uv/pull/1605
synced_at: 2026-01-10T15:33:24Z
```

# Update the scenarios to use vendored build dependencies

---

_Pull request opened by @zanieb on 2024-02-17 17:54_

Uses `--find-links` to discover vendored scenario build dependencies and allows us to use `--index-url` instead of `--extra-index-url` to avoid hitting the real PyPI in scenario tests.

---

_Label `testing` added by @zanieb on 2024-02-17 17:55_

---

_Comment by @zanieb on 2024-02-17 17:56_

We prefer `--find-links` packages over the index packages, right?

---

_Comment by @zanieb on 2024-02-17 19:20_

@BurntSushi perhaps you're more familiar with the Windows stack size work than me? Why would this cause stack overflows now?

---

_Merged by @zanieb on 2024-02-19 21:55_

---

_Closed by @zanieb on 2024-02-19 21:55_

---

_Branch deleted on 2024-02-19 21:55_

---
