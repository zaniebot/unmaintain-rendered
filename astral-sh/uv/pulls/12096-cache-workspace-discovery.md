```yaml
number: 12096
title: " Cache workspace discovery"
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/cache-workspace-discovery
created_at: 2025-03-10T14:43:19Z
updated_at: 2025-03-11T15:43:41Z
url: https://github.com/astral-sh/uv/pull/12096
synced_at: 2026-01-10T11:10:39Z
```

#  Cache workspace discovery

---

_Pull request opened by @konstin on 2025-03-10 14:43_

Reduce the overhead of `uv run` in large workspaces. Instead of re-discovering the entire workspace each time we resolve the metadata of a member, we can the discovered set of workspace members. Care needs to be taken to not cache the discovery for `uv init`, `uv add` and `uv remove`, which change the definitions of workspace members.

Below is apache airflow e3fe06382df4b19f2c0de40ce7c0bdc726754c74 `uv run python` with a minimal payload. With this change, we avoid a ~350ms overhead of each `uv run` invocation.

```
$ hyperfine --warmup 2 \
    "uv run --no-dev python -c \"print('hi')\"" \
    "uv-profiling run --no-dev python -c \"print('hi')\""
Benchmark 1: uv run --no-dev python -c "print('hi')"
  Time (mean ± σ):     492.6 ms ±   7.0 ms    [User: 393.2 ms, System: 97.1 ms]
  Range (min … max):   482.3 ms … 501.5 ms    10 runs
 
Benchmark 2: uv-profiling run --no-dev python -c "print('hi')"
  Time (mean ± σ):     129.7 ms ±   2.5 ms    [User: 105.4 ms, System: 23.2 ms]
  Range (min … max):   126.0 ms … 136.1 ms    22 runs
 
Summary
  uv-profiling run --no-dev python -c "print('hi')" ran
    3.80 ± 0.09 times faster than uv run --no-dev python -c "print('hi')"
```

The profile after those change below. We still spend a large chunk in toml parsing (both `uv.lock` and `pyproject.toml`), but it's not excessive anymore.

![image](https://github.com/user-attachments/assets/6fe78510-7e25-48ee-8a6d-220ee98ad120)


---

_Label `performance` added by @konstin on 2025-03-10 14:43_

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:687 on 2025-03-10 14:44_

We could avoid the clone here with a nested hashmap but I prefer the simplicity and extensibility (with future discovery options) here.

---

_@konstin reviewed on 2025-03-10 14:44_

---

_@charliermarsh approved on 2025-03-10 19:47_

---

_Comment by @konstin on 2025-03-10 21:03_

CC @potiuk this is hopefully relevant to you too, I've been checking if there are performance gotchas when adopting uv for large workspaces.

---

_Merged by @konstin on 2025-03-10 21:03_

---

_Closed by @konstin on 2025-03-10 21:03_

---

_Branch deleted on 2025-03-10 21:03_

---

_Comment by @potiuk on 2025-03-11 15:43_

> CC @potiuk this is hopefully relevant to you too, I've been checking if there are performance gotchas when adopting uv for large workspaces.

Nice! I did notice a bit of overhead in our workspace and haven't got a time to follow up to do some checking... But you seem to be (as usual) solving the problems before your users manage to even realise them :)

Good job :)

---
