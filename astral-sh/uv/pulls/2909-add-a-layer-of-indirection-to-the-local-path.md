```yaml
number: 2909
title: Add a layer of indirection to the local path-based wheel cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/manifest
created_at: 2024-04-08T19:14:21Z
updated_at: 2024-04-08T19:33:00Z
url: https://github.com/astral-sh/uv/pull/2909
synced_at: 2026-01-10T14:43:31Z
```

# Add a layer of indirection to the local path-based wheel cache

---

_Pull request opened by @charliermarsh on 2024-04-08 19:14_

## Summary

Right now, the path-based wheel cache just looks at the symlink to the archives directory, checks the timestamp on it, and continues with that symlink as long as the timestamp is up-to-date.

The HTTP-based wheel meanwhile, uses an intermediary `.http` file, which includes the HTTP caching information. The `.http` file's payload is just a path pointing to an entry in the archives directory.

This PR modifies the path-based codepaths to use a similar cache file, which stores a timestamp along with a path to the archives directory. The main advantage here is that we can add other data to this cache file (namely, hashes in the future).

## Test Plan

Beyond existing tests, I also verified that this doesn't require a version bump:

```
git checkout main 
cargo run pip install ~/Downloads/zeal-0.0.1-py3-none-any.whl --cache-dir baz --reinstall
git checkout charlie/manifest
cargo run pip install ~/Downloads/zeal-0.0.1-py3-none-any.whl --cache-dir baz --reinstall
cargo run pip install ~/Downloads/zeal-0.0.1-py3-none-any.whl --cache-dir baz --reinstall --refresh
```


---

_Label `internal` added by @charliermarsh on 2024-04-08 19:15_

---

_Marked ready for review by @charliermarsh on 2024-04-08 19:17_

---

_Merged by @charliermarsh on 2024-04-08 19:32_

---

_Closed by @charliermarsh on 2024-04-08 19:32_

---

_Branch deleted on 2024-04-08 19:33_

---
