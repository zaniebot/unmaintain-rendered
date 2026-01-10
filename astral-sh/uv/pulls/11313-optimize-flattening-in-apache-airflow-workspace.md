```yaml
number: 11313
title: Optimize flattening in apache airflow workspace
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/top-level-marker
created_at: 2025-02-07T11:30:46Z
updated_at: 2025-02-07T22:08:42Z
url: https://github.com/astral-sh/uv/pull/11313
synced_at: 2026-01-10T11:10:35Z
```

# Optimize flattening in apache airflow workspace

---

_Pull request opened by @konstin on 2025-02-07 11:30_

## Motivation

No-op `uv lock` in apache airflow (891c67f210ab7c877d1f00ea6ea3d3cdbb0e96ef) is slow, which makes `uv run` slow, too.

Reference project:

```
$ hyperfine "uv run python -c \"print('hi')\""
Benchmark 1: uv run python -c "print('hi')"
Time (mean ± σ):      16.3 ms ±   1.5 ms    [User: 9.8 ms, System: 6.4 ms]
Range (min … max):    13.0 ms …  20.0 ms    186 runs
```

Apache airflow before:

```
$ hyperfine "uv run python -c \"print('hi')\""
Benchmark 1: uv run python -c "print('hi')"
Time (mean ± σ):     161.0 ms ±   5.2 ms    [User: 135.3 ms, System: 24.1 ms]
Range (min … max):   155.0 ms … 176.3 ms    18 runs
```

## Optimization

`FlatRequiresDist::from_requirements` is taking 50% of main thread runtime.

Before:

![image](https://github.com/user-attachments/assets/10ea76eb-d1e9-477c-b400-39e653eb8f3a)

After both commits:

![image](https://github.com/user-attachments/assets/5c578ff6-f80b-46bb-9b5f-8be8435c3d85)

Apache airflow after the first commit:

```
$ hyperfine "uv-profiling run python -c \"print('hi')\""
Benchmark 1: uv-profiling run python -c "print('hi')"
  Time (mean ± σ):     122.3 ms ±   5.4 ms    [User: 96.1 ms, System: 24.7 ms]
  Range (min … max):   114.0 ms … 133.2 ms    23 runs
```

Apache airflow after the second commit:

```
$ hyperfine "uv-profiling run python -c \"print('hi')\""
Benchmark 1: uv-profiling run python -c "print('hi')"
  Time (mean ± σ):     108.5 ms ±   3.4 ms    [User: 83.2 ms, System: 24.2 ms]
  Range (min … max):   103.6 ms … 119.9 ms    28 runs
```

---

_Label `performance` added by @konstin on 2025-02-07 11:30_

---

_Review requested from @ibraheemdev by @konstin on 2025-02-07 11:30_

---

_@konstin reviewed on 2025-02-07 11:32_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/tree.rs`:1006 on 2025-02-07 11:32_

If we didn't go through the DNF for the general top level marker case this could be a `&'static ExtraName`

---

_@charliermarsh approved on 2025-02-07 13:45_

---

_Merged by @charliermarsh on 2025-02-07 22:08_

---

_Closed by @charliermarsh on 2025-02-07 22:08_

---

_Branch deleted on 2025-02-07 22:08_

---
