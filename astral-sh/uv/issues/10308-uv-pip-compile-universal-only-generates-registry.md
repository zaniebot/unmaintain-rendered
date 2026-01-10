```yaml
number: 10308
title: "`uv pip compile --universal` only generates registry hashes for a single distribution"
type: issue
state: open
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-01-06T00:48:25Z
updated_at: 2025-01-06T00:48:28Z
url: https://github.com/astral-sh/uv/issues/10308
synced_at: 2026-01-10T04:36:21Z
```

# `uv pip compile --universal` only generates registry hashes for a single distribution

---

_Issue opened by @charliermarsh on 2025-01-06 00:48_

When a registry distribution lacks hashes, we attempt to generate them. However, we only generate such a hash for the currently-selected wheel. So we're only generating hash for one distribution, rather than for each distribution for a given package-version. The original motivation here was to include hashes for `--find-links` entries. But to do this _right_, we need to hash all the distributions for package-version. I don't know if this is actually worth doing, so I'm somewhat inclined to stop generating hashes here entirely and say that we need the registry to provide them.

---

_Label `bug` added by @charliermarsh on 2025-01-06 00:48_

---
