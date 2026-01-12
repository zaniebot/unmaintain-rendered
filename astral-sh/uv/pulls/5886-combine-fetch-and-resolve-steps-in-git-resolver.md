```yaml
number: 5886
title: Combine fetch and resolve steps in Git resolver
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fetch
created_at: 2024-08-07T18:43:01Z
updated_at: 2024-08-08T12:52:32Z
url: https://github.com/astral-sh/uv/pull/5886
synced_at: 2026-01-12T16:07:04Z
```

# Combine fetch and resolve steps in Git resolver

---

_@charliermarsh_

## Summary

Whenever we call `resolve`, we immediately call `fetch` after. And in some cases `resolve` actually calls `fetch` internally. It seems a lot simpler to just merge these into one method that returns a `Fetch` (which itself contains the fully-resolved URL).

Closes https://github.com/astral-sh/uv/issues/5876.


---

_Merged by @charliermarsh on 2024-08-07 22:35_

---

_Closed by @charliermarsh on 2024-08-07 22:35_

---

_Branch deleted on 2024-08-07 22:35_

---

_@charliermarsh reviewed on 2024-08-08 12:52_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:2439 on 2024-08-08 12:52_

This is a bug... How did I miss that?

---
