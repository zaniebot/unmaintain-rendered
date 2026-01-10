```yaml
number: 2555
title: "Implement `--no-strip-extras` to preserve extras in compilation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/strip-extras
created_at: 2024-03-19T23:28:51Z
updated_at: 2024-03-24T11:12:38Z
url: https://github.com/astral-sh/uv/pull/2555
synced_at: 2026-01-10T14:49:08Z
```

# Implement `--no-strip-extras` to preserve extras in compilation

---

_Pull request opened by @charliermarsh on 2024-03-19 23:28_

## Summary

We strip extras by default, but there are some valid use-cases in which they're required (see the linked issue). This PR doesn't change our default, but it does add `--no-strip-extras`, which lets users preserve extras in the output requirements.

Closes https://github.com/astral-sh/uv/issues/1595.

---

_Label `enhancement` added by @charliermarsh on 2024-03-19 23:28_

---

_Label `compatibility` added by @charliermarsh on 2024-03-19 23:28_

---

_Merged by @charliermarsh on 2024-03-19 23:59_

---

_Closed by @charliermarsh on 2024-03-19 23:59_

---

_Branch deleted on 2024-03-19 23:59_

---

_Comment by @palfrey on 2024-03-24 11:12_

Thanks!

---
