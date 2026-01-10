```yaml
number: 12502
title: "Respect build constraints in `uv sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2025-03-27T01:59:36Z
updated_at: 2025-03-27T21:11:52Z
url: https://github.com/astral-sh/uv/pull/12502
synced_at: 2026-01-10T11:10:40Z
```

# Respect build constraints in `uv sync`

---

_Pull request opened by @charliermarsh on 2025-03-27 01:59_

## Summary

There are still a few missing sites that we need to audit:

- `uv tool install` (https://github.com/astral-sh/uv/issues/12496)
- `uv tool run` (https://github.com/astral-sh/uv/issues/12496)
- The `--with` dependencies in `uv run --with` (https://github.com/astral-sh/uv/issues/12505)

Closes #12441.


---

_Label `bug` added by @charliermarsh on 2025-03-27 01:59_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-27 02:08_

---

_Review requested from @konstin by @charliermarsh on 2025-03-27 02:08_

---

_Marked ready for review by @charliermarsh on 2025-03-27 02:08_

---

_Review requested from @Gankra by @charliermarsh on 2025-03-27 02:22_

---

_Comment by @gesslerpd on 2025-03-27 04:02_

Leaving quick comment, unsure if `uv tree` uses these codepaths but I was unable to use `UV_BUILD_CONSTRAINT` to avoid the setuptools issue earlier this week for that command as well.

---

_Comment by @charliermarsh on 2025-03-27 12:57_

@gesslerpd -- Did it work if you added the build constraints to `pyproject.toml`? I don't think the `uv tree` (and `uv sync`, etc.) CLIs respect `UV_BUILD_CONSTRAINT`.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:1330 on 2025-03-27 19:38_

Why do we need to invalidate the lockfile, would the resolution change with different build dependencies? (And if so, do we need to invalidate the cache of previous build attempts?)

---

_@konstin approved on 2025-03-27 19:48_

---

_@charliermarsh reviewed on 2025-03-27 21:11_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:1330 on 2025-03-27 21:11_

It's only because we write the build constraints to the lockfile, and I think it's nice to ensure that we encode them.

---

_Merged by @charliermarsh on 2025-03-27 21:11_

---

_Closed by @charliermarsh on 2025-03-27 21:11_

---

_Branch deleted on 2025-03-27 21:11_

---
