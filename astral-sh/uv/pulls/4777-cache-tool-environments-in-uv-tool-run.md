```yaml
number: 4777
title: "Cache tool environments in `uv tool run`"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/cache-tool-run
created_at: 2024-07-03T15:24:18Z
updated_at: 2024-07-03T17:16:35Z
url: https://github.com/astral-sh/uv/pull/4777
synced_at: 2026-01-12T16:06:27Z
```

# Cache tool environments in `uv tool run`

---

_@charliermarsh_

## Summary

The basic strategy:

- When the user does `uv tool run`, we resolve the `from` and `with` requirements (always).
- After resolving, we generate a hash of the requirements. For now, I'm just converting to a lockfile and hashing _that_, but that's an implementation detail.
- Once we have a hash, we _also_ hash the interpreter.
- We then store environments in `${CACHE_DIR}/${INTERPRETER_HASH}/${RESOLUTION_HASH}`.

Some consequences:

- We cache based on the interpreter, so if you request a different Python, we'll create a new environment (even if they're compatible). This has the nice side-effect of ensuring that we don't use environments for interpreters that were later deleted.
- We cache the `from` and `with` together. In practice, we may want to cache them separately, then layer them? But this is also an implementation detail that we could change later.
- Because we use the lockfile as the cache key, we will invalidate the cache when the format changes. That seems ok, but we could improve it in the future by generating a stable hash from a lockfile that's independent of the schema.

There are a lot of TODOs, but it'd be nice to get alignment on the strategy before I spend time knocking them out.

Closes https://github.com/astral-sh/uv/issues/4752.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-03 15:50_

---

_Comment by @charliermarsh on 2024-07-03 17:16_

Gonna open a new PR when it's ready to avoid notification spam.

---

_Closed by @charliermarsh on 2024-07-03 17:16_

---
