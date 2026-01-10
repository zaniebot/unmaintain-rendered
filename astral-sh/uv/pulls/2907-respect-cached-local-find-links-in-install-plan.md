```yaml
number: 2907
title: "Respect cached local `--find-links` in install plan"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/f
created_at: 2024-04-08T18:46:23Z
updated_at: 2024-04-08T18:58:34Z
url: https://github.com/astral-sh/uv/pull/2907
synced_at: 2026-01-10T14:43:31Z
```

# Respect cached local `--find-links` in install plan

---

_Pull request opened by @charliermarsh on 2024-04-08 18:46_

## Summary

I think this is kind of just an oversight. If a wheel is available via `--find-links`, and the index is "local", we never find it in the cache.

## Test Plan

`cargo test`

---

_Label `bug` added by @charliermarsh on 2024-04-08 18:46_

---

_Marked ready for review by @charliermarsh on 2024-04-08 18:46_

---

_@charliermarsh reviewed on 2024-04-08 18:46_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:2617 on 2024-04-08 18:46_

On main, we see:

```text
Resolved 1 package in [TIME]
Downloaded 1 package in [TIME]
Installed 1 package in [TIME]
 + tqdm==1000.0.0
```

---

_Merged by @charliermarsh on 2024-04-08 18:58_

---

_Closed by @charliermarsh on 2024-04-08 18:58_

---

_Branch deleted on 2024-04-08 18:58_

---
