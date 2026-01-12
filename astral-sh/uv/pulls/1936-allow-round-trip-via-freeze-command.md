```yaml
number: 1936
title: "Allow round-trip via `freeze` command"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/freeze
created_at: 2024-02-23T19:43:04Z
updated_at: 2024-02-23T20:01:55Z
url: https://github.com/astral-sh/uv/pull/1936
synced_at: 2026-01-12T16:04:47Z
```

# Allow round-trip via `freeze` command

---

_@charliermarsh_

## Summary

We're printing the `Display` representation of `InstalledDist`, which isn't guaranteed to be (and in fact isn't) a valid PEP 508 requirement, making it impossible to use the `freeze` output as an input to an install.

Closes https://github.com/astral-sh/uv/issues/1931.


---

_Renamed from "Allow round-trip via freeze command" to "Allow round-trip via `freeze` command" by @charliermarsh on 2024-02-23 19:43_

---

_Label `bug` added by @charliermarsh on 2024-02-23 19:43_

---

_Merged by @charliermarsh on 2024-02-23 20:01_

---

_Closed by @charliermarsh on 2024-02-23 20:01_

---

_Branch deleted on 2024-02-23 20:01_

---
