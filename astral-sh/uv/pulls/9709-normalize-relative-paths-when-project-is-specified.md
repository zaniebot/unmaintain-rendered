```yaml
number: 9709
title: "Normalize relative paths when `--project` is specified"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/relative
created_at: 2024-12-07T19:31:34Z
updated_at: 2024-12-07T20:16:15Z
url: https://github.com/astral-sh/uv/pull/9709
synced_at: 2026-01-12T16:08:56Z
```

# Normalize relative paths when `--project` is specified

---

_@charliermarsh_

## Summary

In the end, the problem is that `relative_to` has incorrect behavior if either path is non-normalize (e.g., `foo/bar/../project`). So I've fixed that method, but we _also_ now normalize `project` upfront, which _also_ fixes the issue.

Closes https://github.com/astral-sh/uv/issues/9692.


---

_Label `bug` added by @charliermarsh on 2024-12-07 19:31_

---

_Marked ready for review by @charliermarsh on 2024-12-07 19:31_

---

_Merged by @charliermarsh on 2024-12-07 20:16_

---

_Closed by @charliermarsh on 2024-12-07 20:16_

---

_Branch deleted on 2024-12-07 20:16_

---
