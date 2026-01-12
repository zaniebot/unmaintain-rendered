```yaml
number: 293
title: Make cache non-optional in most crates
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cache
created_at: 2023-11-02T17:19:43Z
updated_at: 2023-11-02T17:40:22Z
url: https://github.com/astral-sh/uv/pull/293
synced_at: 2026-01-12T16:03:51Z
```

# Make cache non-optional in most crates

---

_@charliermarsh_

This PR makes the cache non-optional in most of Puffin, which simplifies the code, allows us to reuse the cache within a single command (even with `--no-cache`), and also allows us to use the cache for disk storage across an invocation.

I left the cache as optional for the `Virtualenv` and `InterpreterInfo` abstractions, since those are generic enough that it seems nice to have a non-cached version, but it's kind of arbitrary.

---

_Marked ready for review by @charliermarsh on 2023-11-02 17:20_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-02 17:22_

---

_Review requested from @konstin by @charliermarsh on 2023-11-02 17:22_

---

_@zanieb approved on 2023-11-02 17:32_

lgtm

---

_Merged by @charliermarsh on 2023-11-02 17:40_

---

_Closed by @charliermarsh on 2023-11-02 17:40_

---

_Branch deleted on 2023-11-02 17:40_

---
