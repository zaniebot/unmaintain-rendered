---
number: 15822
title: Call out UV_NO_CACHE=1 in logs
type: issue
state: closed
author: williamsnell
labels:
  - enhancement
assignees: []
created_at: 2025-09-13T00:19:43Z
updated_at: 2025-09-14T01:57:12Z
url: https://github.com/astral-sh/uv/issues/15822
synced_at: 2026-01-07T13:12:19-06:00
---

# Call out UV_NO_CACHE=1 in logs

---

_Issue opened by @williamsnell on 2025-09-13 00:19_

### Summary

When `UV_NO_CACHE=1`, uv's cache is *silently* disabled. Running e.g. `uv sync --verbose` prints helpful messages about *other* caching behaviour:

```bash
2025-09-12T22:56:35.6973502Z DEBUG Searching for a compatible version of setuptools (>=61.0.0)
2025-09-12T22:56:35.6974771Z DEBUG Selecting: setuptools==80.9.0 [compatible] (setuptools-80.9.0-py3-none-any.whl)
2025-09-12T22:56:35.6976727Z DEBUG No cache entry for: https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
```

At no point, however, is the fact that caching is disabled (via `UV_NO_CACHE=1`) explicitly logged. Adding this logging would be extremely helpful, to me at least.

Ideally, running with `UV_NO_CACHE=1` would result in something like:

```bash
DEBUG uv's cache has been disabled by UV_NO_CACHE=1. No files will be stored or restored.
```


### Example

I was using vast.ai to run a self-hosted CI runner, using the setup-uv github action. 

uv's caching was misbehaving, and I did not understand why. It was only with much digging that I found `UV_NO_CACHE=1` had been set by default by Vast's dockerfile. 

Thanks for building an incredible tool!

---

_Label `enhancement` added by @williamsnell on 2025-09-13 00:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-14 00:55_

---

_Referenced in [astral-sh/uv#15828](../../astral-sh/uv/pulls/15828.md) on 2025-09-14 01:06_

---

_Closed by @charliermarsh on 2025-09-14 01:57_

---
