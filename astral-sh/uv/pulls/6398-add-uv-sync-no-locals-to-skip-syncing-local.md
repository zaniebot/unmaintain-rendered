```yaml
number: 6398
title: "Add `uv sync --no-locals` to skip syncing local dependencies"
type: pull_request
state: closed
author: charliermarsh
labels:
  - enhancement
  - cache
assignees: []
base: main
head: charlie/locals
created_at: 2024-08-22T01:44:00Z
updated_at: 2024-08-23T20:41:52Z
url: https://github.com/astral-sh/uv/pull/6398
synced_at: 2026-01-10T13:09:51Z
```

# Add `uv sync --no-locals` to skip syncing local dependencies

---

_Pull request opened by @charliermarsh on 2024-08-22 01:44_

## Summary

This PR adds a flag (name TBD) that skips the installation of any local dependencies, allowing users to prime a Docker layer by copying over _just_ the lockfile.

See: https://github.com/astral-sh/uv/issues/4028


---

_Marked ready for review by @charliermarsh on 2024-08-22 01:49_

---

_Comment by @charliermarsh on 2024-08-22 01:51_

I sort of hate the name, and I don't think we've resolved the issue of "flag for only omitting the current project".

---

_Label `enhancement` added by @charliermarsh on 2024-08-22 01:51_

---

_Label `cache` added by @charliermarsh on 2024-08-22 01:51_

---

_Comment by @zanieb on 2024-08-22 03:53_

Hoping a good name comes to me overnight...

---

_Review comment by @konstin on `crates/distribution-types/src/resolution.rs`:75 on 2024-08-22 10:19_

nit: Should this take `self` instead of `&self`?

---

_Review comment by @konstin on `docs/guides/integration/docker.md`:185 on 2024-08-22 10:22_

```suggestion
FROM python:3.12-slim
```

no need to lock in a debian version if we don't use any debian-specific features.

---

_@konstin approved on 2024-08-22 10:25_

---

_@zanieb reviewed on 2024-08-23 18:21_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolution.rs`:75 on 2024-08-23 18:21_

Done

---

_@zanieb reviewed on 2024-08-23 18:21_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:185 on 2024-08-23 18:21_

Done

---

_Comment by @zanieb on 2024-08-23 20:41_

We could still support `--no-install-locals`, but I did https://github.com/astral-sh/uv/pull/6538 / https://github.com/astral-sh/uv/pull/6539 / https://github.com/astral-sh/uv/pull/6540 for now using this implementation



---

_Closed by @zanieb on 2024-08-23 20:41_

---
