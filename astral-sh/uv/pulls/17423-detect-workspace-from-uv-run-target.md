```yaml
number: 17423
title: "Detect workspace from `uv run` target"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/target-preview
created_at: 2026-01-12T19:17:31Z
updated_at: 2026-01-12T19:52:51Z
url: https://github.com/astral-sh/uv/pull/17423
synced_at: 2026-01-12T20:26:40Z
```

# Detect workspace from `uv run` target

---

_@charliermarsh_

## Summary

Given `uv run foo/bar.py`, we now detect the workspace starting at `foo/bar.py`, rather than the current working directory. I think this is much more intuitive as demonstrated by the new test.

This change is currently in preview, but would ship as a breaking change in v0.10.


---

_Label `preview` added by @charliermarsh on 2026-01-12 19:17_

---

_Review requested from @zanieb by @charliermarsh on 2026-01-12 19:17_

---

_Review requested from @konstin by @charliermarsh on 2026-01-12 19:17_

---

_Marked ready for review by @charliermarsh on 2026-01-12 19:17_

---

_@zanieb approved on 2026-01-12 19:19_

Can we document this somewhere? Perhaps in the `uv run` CLI help? A way to think about it... where would we explain this behavior once we change the default?

---

_Merged by @zanieb on 2026-01-12 19:52_

---

_Closed by @zanieb on 2026-01-12 19:52_

---

_Branch deleted on 2026-01-12 19:52_

---
