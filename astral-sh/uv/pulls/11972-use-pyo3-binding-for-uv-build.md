```yaml
number: 11972
title: "Use `pyo3` binding for uv_build"
type: pull_request
state: closed
author: j178
labels: []
assignees: []
draft: true
base: konsti/standalone-build-backend-package
head: pyo3-uv-build
created_at: 2025-03-05T06:33:58Z
updated_at: 2025-03-07T14:07:54Z
url: https://github.com/astral-sh/uv/pull/11972
synced_at: 2026-01-12T16:10:05Z
```

# Use `pyo3` binding for uv_build

---

_@j178_

## Summary

This is an experiment with the `pyo3` binding for the `uv_build` backend introduced in #11446, as recommended by @T-256 in this comment: https://github.com/astral-sh/uv/pull/11446#discussion_r1975144096

---

_Review requested from @konstin by @zanieb on 2025-03-05 16:38_

---

_Comment by @konstin on 2025-03-06 10:35_

How do a binary with a CLI and a library with pyo3 bindings compare in terms of platform compatibility and binary site? Especially platform compatibility and how easy it is to build for all sorts of configurations is a main driver for the `uv_build` design.

---

_Closed by @zanieb on 2025-03-06 19:27_

---

_Comment by @konstin on 2025-03-07 13:13_

Did github just autoclose this PR instead of updating the base branch to main?

---

_Comment by @zanieb on 2025-03-07 14:07_

Oh weird, I did not close this — sorry about that. It must have closed when I rebased and merged?

---
