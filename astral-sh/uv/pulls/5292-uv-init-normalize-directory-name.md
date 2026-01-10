```yaml
number: 5292
title: "`uv init` normalize directory name"
type: pull_request
state: merged
author: j178
labels:
  - bug
assignees: []
merged: true
base: main
head: init-normalize
created_at: 2024-07-22T15:08:02Z
updated_at: 2024-07-22T16:18:34Z
url: https://github.com/astral-sh/uv/pull/5292
synced_at: 2026-01-10T13:42:52Z
```

# `uv init` normalize directory name

---

_Pull request opened by @j178 on 2024-07-22 15:08_

## Summary

Resolves #5255

---

_@konstin approved on 2024-07-22 15:43_

---

_Comment by @charliermarsh on 2024-07-22 15:48_

Do we reject packages with invalid names, like `uv init "foo bar"`? What happens there?

---

_Comment by @konstin on 2024-07-22 16:00_

We may need to revisit the normalization for dots (e.g. `foo.bar`) later, but always using underscores doesn't seems correcter than the current solution

---

_Label `bug` added by @konstin on 2024-07-22 16:01_

---

_Comment by @j178 on 2024-07-22 16:02_

> Do we reject packages with invalid names, like `uv init "foo bar"`? What happens there?

error: Not a valid package or extra name: "foo bar". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.

It gets rejected during parsing to `PackageName`.

---

_Merged by @konstin on 2024-07-22 16:09_

---

_Closed by @konstin on 2024-07-22 16:09_

---

_Branch deleted on 2024-07-22 16:18_

---
