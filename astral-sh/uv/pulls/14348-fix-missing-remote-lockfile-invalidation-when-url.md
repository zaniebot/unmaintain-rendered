```yaml
number: 14348
title: Fix missing remote lockfile invalidation when URL has no path
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
base: main
head: zb/lock-check-slash
created_at: 2025-06-29T15:22:28Z
updated_at: 2025-06-29T17:46:00Z
url: https://github.com/astral-sh/uv/pull/14348
synced_at: 2026-01-10T06:53:01Z
```

# Fix missing remote lockfile invalidation when URL has no path

---

_Pull request opened by @zanieb on 2025-06-29 15:22_

Fixes https://github.com/astral-sh/uv/issues/14344#issuecomment-3016763833

`Url::pop_if_empty`, used at 

https://github.com/astral-sh/uv/blob/734b228edf53984686dbb48db57aff8505695473/crates/uv-distribution-types/src/index_url.rs#L265-L269

does not remove a `/` if there's no path, e.g., `https://example.com/` (vs `https://example.com/foo/`)

However, `UrlString::without_trailing_slash` always trims the trailing slash

https://github.com/astral-sh/uv/blob/f892b8564fc3dc2e9c6db4453d8d8860c4a8ba65/crates/uv-distribution-types/src/file.rs#L175-L177

since these are inconsistent, we consider URLs inequal and `uv lock --check` regresses with

```
Ignoring existing lockfile due to missing remote index: anyio 4.9.0 from https://localhost:8999
```

---

_Label `bug` added by @zanieb on 2025-06-29 15:22_

---

_@charliermarsh approved on 2025-06-29 15:23_

I think we should fix `without_trailing_slash` to match `pop_if_empty`, but this is also fine for now.

---

_Comment by @zanieb on 2025-06-29 17:46_

Preferring https://github.com/astral-sh/uv/pull/14349

---

_Closed by @zanieb on 2025-06-29 17:46_

---
