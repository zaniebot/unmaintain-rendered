```yaml
number: 3162
title: "Add UV_CONSTRAINT environment variable to provide value for `--constraint` "
type: pull_request
state: merged
author: Czaki
labels:
  - configuration
assignees: []
merged: true
base: main
head: add_UV_CONSTRAINT
created_at: 2024-04-20T16:07:21Z
updated_at: 2024-04-20T22:01:04Z
url: https://github.com/astral-sh/uv/pull/3162
synced_at: 2026-01-10T14:43:31Z
```

# Add UV_CONSTRAINT environment variable to provide value for `--constraint` 

---

_Pull request opened by @Czaki on 2024-04-20 16:07_

## Summary

This PR is adding `UV_CONSTRAINT` environment variable as analogous to `PIP_CONSTRAINT` to allow providing constraint file via environment variable. Implementing this will simplify adoption of uv in testing procedure in projects that I'm involved (testing using tox). 

This was my motivation for opening #1841 that is closed in favor of #1789 which was closed without implementing this feature. 

In this implementation, I have used space as a separator as analogous to `pip`. This introduces an obvious problem if the path contains space. Another option could be to use standard separator (`:` - UNIX like, `;` - Windows). Which one did you prefer? 

## Test Plan

It is my first contribution and first rust coding experience. It will be nice if one could point how I should implement testing this. 




---

_Comment by @Czaki on 2024-04-20 16:45_

Test failure looks like network error 

---

_Label `configuration` added by @charliermarsh on 2024-04-20 21:32_

---

_Comment by @charliermarsh on 2024-04-20 21:32_

I think space-delimiting is ok, since it's at least compatible with pip and the other arguments we added.

---

_Merged by @charliermarsh on 2024-04-20 21:32_

---

_Closed by @charliermarsh on 2024-04-20 21:32_

---

_Branch deleted on 2024-04-20 22:01_

---
