```yaml
number: 14788
title: "Revert \"Use an ephemeral environment for `uv run --with` invocations (#14447)\""
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/reverts
created_at: 2025-07-21T13:00:42Z
updated_at: 2025-07-22T12:15:40Z
url: https://github.com/astral-sh/uv/pull/14788
synced_at: 2026-01-10T06:53:02Z
```

# Revert "Use an ephemeral environment for `uv run --with` invocations (#14447)"

---

_Pull request opened by @charliermarsh on 2025-07-21 13:00_

## Summary

This reverts commit 6df7dab2df6e5a9b3bf36183851dd9d7c0824c9f, which is causing more trouble than it is solving problems. Specifically, for tools like Jupyter and MyPy, because the shebang in the script of the `--with` environment uses the Python of the `--with` environment (and that environment does _not_ have the requisite `.pth` file), they're not able to see any of the base environment's dependencies. We considered copying over those executables and rewriting the shebangs, and that _could_ work for MyPy, but it ends up breaking Jupyter:
 
<img width="1952" height="1302" alt="Screenshot 2025-07-19 at 9 40 48 PM" src="https://github.com/user-attachments/assets/8f9a437d-9726-4283-9914-4beadd5b8e8d" />

Closes #14749.

Closes #14729.


---

_Review requested from @zanieb by @charliermarsh on 2025-07-21 13:02_

---

_Marked ready for review by @charliermarsh on 2025-07-21 13:02_

---

_Comment by @zanieb on 2025-07-22 12:15_

Hopefully not needed, but still viable following #14790 if we need it.

---

_Closed by @zanieb on 2025-07-22 12:15_

---
