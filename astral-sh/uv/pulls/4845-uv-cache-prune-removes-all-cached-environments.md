```yaml
number: 4845
title: "`uv cache prune` removes all cached environments"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: prune-cached-env
created_at: 2024-07-06T10:42:38Z
updated_at: 2024-07-06T19:37:16Z
url: https://github.com/astral-sh/uv/pull/4845
synced_at: 2026-01-12T16:06:29Z
```

# `uv cache prune` removes all cached environments

---

_@blueraft_

## Summary

Resolves #4802

## Test Plan
- `cargo test`
```sh
❯ cargo run -- cache prune -v
Pruning cache at: /Users/ahmedilyas/Library/Caches/uv
No unused entries found
❯ cargo run -- tool run cowsay
warning: `uv tool run` is experimental and may change without warning.
Resolved 1 package in 182ms
Installed 1 package in 20ms
 + cowsay==6.1
usage: Cowsay [-h] [-c CHARACTER] -t TEXT [-v]
Cowsay: error: the following arguments are required: -t/--text
❯ cargo run -- cache prune -v
Pruning cache at: /Users/ahmedilyas/Library/Caches/uv
    0.793440s DEBUG uv_cache Removing dangling cache entry: /Users/ahmedilyas/Library/Caches/uv/environments-v1/095cd7c4c298a0d8
Removed 41 files (143.5KiB)
```

---

_@blueraft reviewed on 2024-07-06 10:51_

---

_Review comment by @blueraft on `crates/uv/tests/cache_prune.rs`:123 on 2024-07-06 10:51_

Should I add this filter to the `with_filtered_counts()` fn?

---

_@charliermarsh reviewed on 2024-07-06 19:06_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/lib.rs`:396 on 2024-07-06 19:06_

I think this can be a bit simpler -- I don't think we need to check if `references.contains` for `CacheBucket::Environments` because these are never symlinked anywhere, so we can just remove the whole CacheBucket::Environments` directory.

---

_@charliermarsh reviewed on 2024-07-06 19:06_

---

_Review comment by @charliermarsh on `crates/uv/tests/cache_prune.rs`:123 on 2024-07-06 19:06_

Yeah, I think we should add it there.

---

_@charliermarsh approved on 2024-07-06 19:28_

Thanks!

---

_Label `enhancement` added by @charliermarsh on 2024-07-06 19:28_

---

_Label `preview` added by @charliermarsh on 2024-07-06 19:28_

---

_Merged by @charliermarsh on 2024-07-06 19:37_

---

_Closed by @charliermarsh on 2024-07-06 19:37_

---
