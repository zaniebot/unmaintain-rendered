```yaml
number: 10017
title: "Denote required platforms in `[tool.uv]` metadata"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: charlie/incomplete
head: charlie/required-platforms
created_at: 2024-12-19T01:23:34Z
updated_at: 2024-12-20T19:14:26Z
url: https://github.com/astral-sh/uv/pull/10017
synced_at: 2026-01-10T11:44:31Z
```

# Denote required platforms in `[tool.uv]` metadata

---

_Pull request opened by @charliermarsh on 2024-12-19 01:23_

## Summary

This is an extension of #9928 that makes the set of "required platforms" configurable, thereby making the resolver less tightly-coupled to the list of platforms.

I'm wondering if there's a way for us to reuse `environments` for this somehow.


---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:528 on 2024-12-19 01:54_

\cc @zanieb @konstin 

---

_@charliermarsh reviewed on 2024-12-19 01:54_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:509 on 2024-12-19 11:15_

We should make clearer that the specific requirement is that for each marker, there exists at least one wheel that can be installed in the marker's environment.

---

_@konstin reviewed on 2024-12-19 11:20_

---

_Comment by @charliermarsh on 2024-12-20 19:14_

We merged https://github.com/astral-sh/uv/pull/10046 instead for now. We may return to this later.

---

_Closed by @charliermarsh on 2024-12-20 19:14_

---
