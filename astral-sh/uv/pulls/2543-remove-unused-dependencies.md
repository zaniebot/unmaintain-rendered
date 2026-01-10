```yaml
number: 2543
title: Remove unused dependencies
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: shear-dependencies
created_at: 2024-03-19T16:12:23Z
updated_at: 2024-03-19T17:10:10Z
url: https://github.com/astral-sh/uv/pull/2543
synced_at: 2026-01-10T14:49:08Z
```

# Remove unused dependencies

---

_Pull request opened by @MichaReiser on 2024-03-19 16:12_

## Summary

I tried out `cargo shear` to see if there are any unused dependencies that `cargo udeps` isn't reporting. It turned out, there are a few. This PR removes those dependencies. 

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2024-03-19 16:12_

---

_@charliermarsh approved on 2024-03-19 17:10_

Great!

---

_Merged by @charliermarsh on 2024-03-19 17:10_

---

_Closed by @charliermarsh on 2024-03-19 17:10_

---

_Branch deleted on 2024-03-19 17:10_

---
