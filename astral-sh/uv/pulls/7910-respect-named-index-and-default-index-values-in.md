```yaml
number: 7910
title: "Respect named `--index` and `--default-index` values in `tool.uv.sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/index-api-cli
created_at: 2024-10-03T22:21:38Z
updated_at: 2024-10-15T23:56:25Z
url: https://github.com/astral-sh/uv/pull/7910
synced_at: 2026-01-12T16:08:04Z
```

# Respect named `--index` and `--default-index` values in `tool.uv.sources`

---

_@charliermarsh_

## Summary

If you pass a named index via the CLI, you can now reference it as a named source. This required some surprisingly large refactors, since we now need to be able to track whether a given index was provided on the CLI vs. elsewhere (since, e.g., we don't want users to be able to reference named indexes defined in global configuration).

Closes https://github.com/astral-sh/uv/issues/7899.


---

_Converted to draft by @charliermarsh on 2024-10-03 22:21_

---

_Label `enhancement` added by @charliermarsh on 2024-10-03 22:21_

---

_Marked ready for review by @charliermarsh on 2024-10-03 23:27_

---

_Review requested from @zanieb by @charliermarsh on 2024-10-03 23:27_

---

_Merged by @charliermarsh on 2024-10-15 23:56_

---

_Closed by @charliermarsh on 2024-10-15 23:56_

---

_Branch deleted on 2024-10-15 23:56_

---
