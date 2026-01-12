```yaml
number: 4750
title: "Use already-installed tools in `uv tool run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/reuse-tool-env
created_at: 2024-07-02T23:34:37Z
updated_at: 2024-07-03T16:35:12Z
url: https://github.com/astral-sh/uv/pull/4750
synced_at: 2026-01-12T16:06:26Z
```

# Use already-installed tools in `uv tool run`

---

_@charliermarsh_

## Summary

This doesn't cache the tool environment; rather, it just uses the `tool install` environment if it satisfies the request.

Closes https://github.com/astral-sh/uv/issues/4742.


---

_Renamed from "Use already-installed tools in uv tool run" to "Use already-installed tools in `uv tool run`" by @charliermarsh on 2024-07-02 23:34_

---

_Label `preview` added by @charliermarsh on 2024-07-02 23:34_

---

_@zanieb reviewed on 2024-07-02 23:36_

---

_Review comment by @zanieb on `crates/uv/tests/tool_run.rs`:254 on 2024-07-02 23:36_

Whoopsie?

---

_@zanieb reviewed on 2024-07-02 23:36_

---

_Review comment by @zanieb on `crates/uv/tests/tool_run.rs`:261 on 2024-07-02 23:36_

Funny that it gets installed but not used.

---

_Comment by @zanieb on 2024-07-02 23:39_

Dangerous to review your pull requests so soon.

We should include tests with:

- compatible and incompatible constraints e.g. `uv tool run --from 'black>23' black`
- `--isolated` to opt-out and use the latest version
- an extra requirement via `--with`

---

_Comment by @charliermarsh on 2024-07-02 23:53_

Good call, added!

---

_Review requested from @zanieb by @charliermarsh on 2024-07-02 23:55_

---

_@zanieb approved on 2024-07-03 16:10_

---

_@zanieb reviewed on 2024-07-03 16:10_

---

_Review comment by @zanieb on `crates/uv/tests/tool_run.rs`:311 on 2024-07-03 16:10_

This would be nice and sensible

---

_Merged by @charliermarsh on 2024-07-03 16:35_

---

_Closed by @charliermarsh on 2024-07-03 16:35_

---

_Branch deleted on 2024-07-03 16:35_

---
