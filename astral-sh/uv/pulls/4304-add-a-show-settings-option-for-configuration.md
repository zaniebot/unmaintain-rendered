```yaml
number: 4304
title: "Add a `--show-settings` option for configuration testing"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/configuration-iii
created_at: 2024-06-13T13:21:33Z
updated_at: 2024-06-14T03:14:24Z
url: https://github.com/astral-sh/uv/pull/4304
synced_at: 2026-01-10T13:54:02Z
```

# Add a `--show-settings` option for configuration testing

---

_Pull request opened by @charliermarsh on 2024-06-13 13:21_

## Summary

The fixtures here are pretty large, but it lets us test what we actually care about (the resolved settings) rather than inferring the resolved settings from behavior, which I think is a big improvement.

I also broke the tests down into more granular cases.


---

_Label `testing` added by @charliermarsh on 2024-06-13 13:21_

---

_Comment by @zanieb on 2024-06-13 13:24_

How hard would it be to port a better display from Ruff? Happy to track in an issue.

---

_Comment by @charliermarsh on 2024-06-13 13:27_

It would be a pain. And I actually think it would not be what I want for this specific use-case though I can understand why it would be useful.

---

_Comment by @charliermarsh on 2024-06-13 13:29_

I will track in an issue.

---

_Merged by @charliermarsh on 2024-06-14 03:14_

---

_Closed by @charliermarsh on 2024-06-14 03:14_

---

_Branch deleted on 2024-06-14 03:14_

---
