```yaml
number: 15251
title: "Hint to use `extra-build-depedencies` when build fails with a missing…"
type: pull_request
state: closed
author: blueraft
labels: []
assignees: []
base: main
head: hint-extra-build-dependencies
created_at: 2025-08-13T15:55:00Z
updated_at: 2025-08-14T18:31:55Z
url: https://github.com/astral-sh/uv/pull/15251
synced_at: 2026-01-12T16:11:39Z
```

# Hint to use `extra-build-depedencies` when build fails with a missing…

---

_@blueraft_

… package.

## Summary

Closes #15118 

## Test Plan

`cargo test`

---

_@charliermarsh reviewed on 2025-08-13 21:42_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install.rs`:5046 on 2025-08-13 21:42_

I know it was already here, but for the second "anyio @ https://files.pythonhosted.org/packages/db/4d/3970183622f0330d3c23d9b8a5f52e365e50381fd484d08e3285104333d3/anyio-4.3.0.tar.gz", can we just show the package name instead of the full URL? Like "anyio".

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:5046 on 2025-08-13 21:44_

:D see https://github.com/astral-sh/uv/pull/15252#discussion_r2274558275 and https://github.com/astral-sh/uv/pull/15252#discussion_r2274711916

---

_@zanieb reviewed on 2025-08-13 21:44_

---

_Closed by @zanieb on 2025-08-14 18:31_

---
