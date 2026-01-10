```yaml
number: 17222
title: Replace deprecated benchmarking mode
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/codspeed
created_at: 2025-12-22T21:41:52Z
updated_at: 2026-01-05T18:52:31Z
url: https://github.com/astral-sh/uv/pull/17222
synced_at: 2026-01-10T05:49:14Z
```

# Replace deprecated benchmarking mode

---

_Pull request opened by @woodruffw on 2025-12-22 21:41_

## Summary

Minor, noticed this with #17221. CodSpeed has deprecated `instrumentation` and replaced it with `simulation`, which has the same meaning:

https://codspeed.io/docs/instruments/cpu/overview#legacy-terminology

## Test Plan

No functional changes.

---

_Assigned to @woodruffw by @woodruffw on 2025-12-22 21:41_

---

_Label `internal` added by @woodruffw on 2025-12-22 21:41_

---

_Review requested from @charliermarsh by @woodruffw on 2025-12-22 21:42_

---

_@charliermarsh approved on 2025-12-22 21:43_

---

_Comment by @zanieb on 2025-12-22 21:56_

Should we change the CI job name too? 🤔 

---

_Comment by @woodruffw on 2025-12-22 22:00_

> Should we change the CI job name too? 🤔

Good call, renamed!

---

_Merged by @konstin on 2026-01-05 18:52_

---

_Closed by @konstin on 2026-01-05 18:52_

---

_Branch deleted on 2026-01-05 18:52_

---
