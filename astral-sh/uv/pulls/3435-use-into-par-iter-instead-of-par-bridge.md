```yaml
number: 3435
title: "Use `into_par_iter` instead of `par_bridge`"
type: pull_request
state: merged
author: DaniPopes
labels: []
assignees: []
merged: true
base: main
head: par-bridge
created_at: 2024-05-07T18:11:16Z
updated_at: 2024-05-07T21:26:15Z
url: https://github.com/astral-sh/uv/pull/3435
synced_at: 2026-01-12T16:05:38Z
```

# Use `into_par_iter` instead of `par_bridge`

---

_@DaniPopes_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Use the native rayon range iterator instead of bridging the standard library's.

---

_Comment by @charliermarsh on 2024-05-07 18:15_

@ibraheemdev - do you mind reviewing this?

---

_Review comment by @ibraheemdev on `crates/uv-extract/src/sync.rs`:18 on 2024-05-07 18:42_

We could also probably use a `dashmap::DashSet` here, if you're interested in making that change as well.

---

_@ibraheemdev approved on 2024-05-07 18:43_

LGTM :+1:

---

_Merged by @charliermarsh on 2024-05-07 19:38_

---

_Closed by @charliermarsh on 2024-05-07 19:38_

---

_Branch deleted on 2024-05-07 21:26_

---
