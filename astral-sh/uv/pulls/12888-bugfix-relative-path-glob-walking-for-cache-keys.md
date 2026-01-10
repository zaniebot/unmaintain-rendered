```yaml
number: 12888
title: Bugfix relative path glob walking for cache-keys
type: pull_request
state: open
author: stonier
labels: []
assignees: []
base: main
head: stonier/relatve_path_walking
created_at: 2025-04-15T00:04:46Z
updated_at: 2025-05-23T16:28:14Z
url: https://github.com/astral-sh/uv/pull/12888
synced_at: 2026-01-10T11:10:40Z
```

# Bugfix relative path glob walking for cache-keys

---

_Pull request opened by @stonier on 2025-04-15 00:04_

## Description

Being able to specify relative paths for `cache-keys` is useful, but it is currently broken. This PR fixes that.

## Background

### Use Case

Building a python project from maturin on a cargo workspace (i.e. it's not just the local package to trigger rebuilds from). e.g.

```
[tool.uv]
# Rebuild package when any rust files change
cache-keys = [
    { file = "pyproject.toml" },
    { file = "Cargo.toml" },
    { file = ".cargo/config.toml" },
    { file = "**/*.rs" },
    { file = "../sv2_behaviors/**/*.rs" },
    { file = "../sv2_collisions/**/*.rs" },
    ...
]
```

### Why it's broken

The previous tool, `globwalker`, cannot handle relative paths - it just silently passed over them. It's a long standing issue (5+ years) that's not likely to be fixed given that it's in maintenance mode.

* https://github.com/Gilnaa/globwalk/issues/28.

## Solution

Plain old `glob` can handle relative paths. It is obsessed with working from `cwd` though, so this PR provides some assistance to redirect the relative paths to that rather than the directory that the toml file is in though.

## Test Plan

I have a unit test in place to check for the relative path lookups.

----

First time contributing here, please do point out where and how I might better integrate this contribution.

---

_Review requested from @BurntSushi by @konstin on 2025-04-15 09:09_

---

_Review requested from @charliermarsh by @konstin on 2025-04-15 09:09_

---

_Comment by @BurntSushi on 2025-04-15 12:28_

I _think_ the technique here is sound, but I'm not certain. @charliermarsh How good is our test coverage here? IIRC, another concern here was perf. This PR I think is changing it to a world where every glob results in its own distinct directory traversal, which seems like it might be an issue. It might be better to just use `globset` with `walkdir`.

---

_Comment by @charliermarsh on 2025-04-15 12:44_

Our test coverage here is not particularly good.

---
