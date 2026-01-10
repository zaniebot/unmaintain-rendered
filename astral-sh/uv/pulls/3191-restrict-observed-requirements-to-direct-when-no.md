```yaml
number: 3191
title: "Restrict observed requirements to direct when `--no-deps` is specified"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/look
created_at: 2024-04-22T17:07:48Z
updated_at: 2024-04-22T17:18:01Z
url: https://github.com/astral-sh/uv/pull/3191
synced_at: 2026-01-10T14:43:32Z
```

# Restrict observed requirements to direct when `--no-deps` is specified

---

_Pull request opened by @charliermarsh on 2024-04-22 17:07_


## Summary

This PR avoids: (1) using the lookahead resolver when `--no-deps` is specified (we'll never use those requirements), and (2) including any transitive requirements when searching for allowed URLs, etc., when `--no-deps` is specified.

Closes https://github.com/astral-sh/uv/issues/3183.


---

_Label `bug` added by @charliermarsh on 2024-04-22 17:07_

---

_Marked ready for review by @charliermarsh on 2024-04-22 17:07_

---

_Merged by @charliermarsh on 2024-04-22 17:17_

---

_Closed by @charliermarsh on 2024-04-22 17:17_

---

_Branch deleted on 2024-04-22 17:18_

---
