---
number: 10903
title: "Providing credentials via env vars for indexes doesn't work for build requirements"
type: issue
state: closed
author: xixixao
labels:
  - needs-mre
assignees: []
created_at: 2025-01-23T15:46:52Z
updated_at: 2025-01-23T18:29:26Z
url: https://github.com/astral-sh/uv/issues/10903
synced_at: 2026-01-10T01:24:58Z
---

# Providing credentials via env vars for indexes doesn't work for build requirements

---

_Issue opened by @xixixao on 2025-01-23 15:46_

### Summary

Repro:
1. Use workspaces
2. Use private index
3. Try to install a workspace package

While putting credentials in `~/.netrc` works, providing credentials via 

```
export UV_INDEX_FOO_USERNAME=foo_user
export UV_INDEX_FOO_PASSWORD=foo_token
```

does not work, resulting in this error (from a Dockerfile build):

```
INFO[0046] Running: [/bin/sh -c uv sync --frozen --all-packages] 
  × Failed to build `foo-workspace-member @
  │ file:///app/foo-workspace-member`
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `hatchling`
  ╰─▶ Because hatchling was not found in the package registry and you require
      hatchling, we can conclude that your requirements are unsatisfiable.
      hint: An index URL
      (http://ourprivateindex.net)
      could not be queried due to a lack of valid authentication credentials
      (401 Unauthorized).
error building image: error building stage: failed to execute command: waiting for process to exit: exit status 1
```

Note that the environment variable credentials **do work** for `uv sync`, so `uv sync --frozen --no-install-workspace` succeeds. It's only the build config and trying to install the workspace package that breaks this:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Probably unrelated, but seemed like a related issue: #7052

If required I can put together a repro repo when I have some time (although you'll need a package registry).

### Platform

Ubuntu amd64

### Version

uv 0.5.21

### Python version

Python 3.11

---

_Label `bug` added by @xixixao on 2025-01-23 15:46_

---

_Comment by @charliermarsh on 2025-01-23 15:58_

Do you mind putting together a reproduction? You can use our "fake" private index: https://pypi-proxy.fly.dev/basic-auth/simple. You can grep for the credentials in the codebase (it's intentionally public, for testing auth).

---

_Label `bug` removed by @charliermarsh on 2025-01-23 16:43_

---

_Label `needs-mre` added by @charliermarsh on 2025-01-23 16:43_

---

_Comment by @xixixao on 2025-01-23 18:29_

False alarm, this was caused by two misunderstandings:

1. There are no good instructions for using docker with workspaces. I didn't realize the first sync should be `uv sync --no-install-workspace --all-packages --frozen` - without `all-packages`, nothing was being installed, so auth was not needed. It would be great to add this to https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers
2.  I was relying on `RUN export` in the Dockerfile, which doesn't persist across the different docker commands. Instead setting the env var in the same command as sync works: `RUN UV_INDEX_INTERNAL_USERNAME=foo uv sync`.

Btw the credentials were not straightforward to grep (they're public+heron atm.).

---

_Closed by @xixixao on 2025-01-23 18:29_

---
