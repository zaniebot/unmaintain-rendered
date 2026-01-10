```yaml
number: 4701
title: "Use `requires-python` semantics for `--universal`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/requires-python-universal
created_at: 2024-07-01T18:33:14Z
updated_at: 2024-07-01T19:16:42Z
url: https://github.com/astral-sh/uv/pull/4701
synced_at: 2026-01-10T13:48:28Z
```

# Use `requires-python` semantics for `--universal`

---

_Pull request opened by @charliermarsh on 2024-07-01 18:33_

## Summary

This doesn't actually change any behaviors, but it does make it a bit easier to solve #4669, because we don't have to support "version narrowing" for the non-`RequiresPython` variants in here. Right now, the semantics are kind of muddied, because the `target` variant is _sometimes_ interpreted as an exact version and sometimes as a lower bound.


---

_Label `internal` added by @charliermarsh on 2024-07-01 18:33_

---

_Merged by @charliermarsh on 2024-07-01 19:16_

---

_Closed by @charliermarsh on 2024-07-01 19:16_

---

_Branch deleted on 2024-07-01 19:16_

---
