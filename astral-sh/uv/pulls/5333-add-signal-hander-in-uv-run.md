```yaml
number: 5333
title: "Add signal hander in `uv run`"
type: pull_request
state: closed
author: Di-Is
labels: []
assignees: []
draft: true
base: main
head: childprocess-graceful-shutdown
created_at: 2024-07-23T14:08:21Z
updated_at: 2024-08-01T11:22:48Z
url: https://github.com/astral-sh/uv/pull/5333
synced_at: 2026-01-10T13:37:23Z
```

# Add signal hander in `uv run`

---

_Pull request opened by @Di-Is on 2024-07-23 14:08_

Fix #5257 

## Summary

Implement a signal handler in `uv run`.

## Test Plan

TODO

---

_Comment by @zanieb on 2024-07-23 14:16_

Thank you! Could you share some references for the implementation here, i.e., why this approach?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-24 03:21_

---

_Comment by @charliermarsh on 2024-07-24 04:08_

I tried something a little different based on some research here: https://github.com/astral-sh/uv/pull/5395

---

_Comment by @Di-Is on 2024-07-24 12:31_

#5395 is better, therefore close issue.

---

_Closed by @Di-Is on 2024-07-24 12:31_

---

_Branch deleted on 2024-08-01 11:22_

---
