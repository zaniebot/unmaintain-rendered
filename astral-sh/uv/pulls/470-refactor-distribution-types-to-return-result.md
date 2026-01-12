```yaml
number: 470
title: "Refactor distribution types to return `Result`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/error
created_at: 2023-11-20T23:04:22Z
updated_at: 2023-11-20T23:08:55Z
url: https://github.com/astral-sh/uv/pull/470
synced_at: 2026-01-12T16:03:58Z
```

# Refactor distribution types to return `Result`

---

_@charliermarsh_

## Summary

A variety of small refactors to the distribution types crate to (1) return `Result` if we find an invalid wheel, rather than treating it as a source distribution with a `.whl` suffix, and (2) DRY up some repeated code around URLs.

---

_Label `internal` added by @charliermarsh on 2023-11-20 23:04_

---

_Merged by @charliermarsh on 2023-11-20 23:08_

---

_Closed by @charliermarsh on 2023-11-20 23:08_

---

_Branch deleted on 2023-11-20 23:08_

---
