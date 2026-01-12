```yaml
number: 8052
title: add instructions on line-level suppression to file-level suppression warning
type: pull_request
state: merged
author: roganartu
labels:
  - internal
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-10-18T19:34:15Z
updated_at: 2023-10-18T22:47:09Z
url: https://github.com/astral-sh/ruff/pull/8052
synced_at: 2026-01-12T15:55:25Z
```

# add instructions on line-level suppression to file-level suppression warning

---

_@roganartu_

## Summary

In #6157 a warning was introduced when users use `ruff: noqa` suppression in-line instead of at the file-level. I had this trigger today after forgetting about it, and the warning is an excellent improvement.

I knew immediately what the issue was because I raised it previously, but on reading the warning I'm not sure it would be so obvious to all users. This PR extends the error with a short sentence explaining that line-level suppression should omit the `ruff:` prefix.

## Test Plan

Not sure it's necessary for such a trivial change :)


---

_Comment by @github-actions[bot] on 2023-10-18 19:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb approved on 2023-10-18 20:07_

I like it

---

_Merged by @charliermarsh on 2023-10-18 22:46_

---

_Closed by @charliermarsh on 2023-10-18 22:46_

---

_Label `internal` added by @charliermarsh on 2023-10-18 22:47_

---

_Comment by @charliermarsh on 2023-10-18 22:47_

Thanks -- this makes sense to me!

---
