```yaml
number: 823
title: "Use `anstream` for all color control"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/anstream
created_at: 2024-01-07T01:27:34Z
updated_at: 2024-01-07T01:44:06Z
url: https://github.com/astral-sh/uv/pull/823
synced_at: 2026-01-12T16:04:13Z
```

# Use `anstream` for all color control

---

_@charliermarsh_

## Summary

We can use `anstream` for all color control, rather than going through `colored`. Note that we still need the `colored` crate, since `colored` and `anstream` solve different problems. (`anstream` recommends using `owo-colors` alongside it, but `colored` seems to work fine?)

Resolves the issue raised in https://github.com/astral-sh/puffin/pull/742 via `anstream` rather than `colored`.

Closes https://github.com/astral-sh/puffin/issues/782.


---

_Label `internal` added by @charliermarsh on 2024-01-07 01:27_

---

_Marked ready for review by @charliermarsh on 2024-01-07 01:27_

---

_Merged by @charliermarsh on 2024-01-07 01:44_

---

_Closed by @charliermarsh on 2024-01-07 01:44_

---

_Branch deleted on 2024-01-07 01:44_

---
