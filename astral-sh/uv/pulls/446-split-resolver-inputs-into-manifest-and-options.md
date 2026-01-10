```yaml
number: 446
title: Split resolver inputs into manifest and options
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/split
created_at: 2023-11-17T18:46:20Z
updated_at: 2023-11-17T18:53:54Z
url: https://github.com/astral-sh/uv/pull/446
synced_at: 2026-01-10T15:50:28Z
```

# Split resolver inputs into manifest and options

---

_Pull request opened by @charliermarsh on 2023-11-17 18:46_

## Summary

This is a refactor to address a TODO in the build context whereby we aren't respecting the resolution options in recursive resolutions. Now, the options are split out from the resolution _manifest_, and shared across the build context tree.

---

_Label `internal` added by @charliermarsh on 2023-11-17 18:50_

---

_Merged by @charliermarsh on 2023-11-17 18:53_

---

_Closed by @charliermarsh on 2023-11-17 18:53_

---

_Branch deleted on 2023-11-17 18:53_

---
