```yaml
number: 14571
title: "Update `toml` to v0.9"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
assignees: []
merged: true
base: main
head: ibraheem/toml-0.9
created_at: 2025-07-11T22:58:54Z
updated_at: 2025-07-21T13:18:18Z
url: https://github.com/astral-sh/uv/pull/14571
synced_at: 2026-01-12T16:11:17Z
```

# Update `toml` to v0.9

---

_@ibraheemdev_

## Summary

This should give us some performance and error message improvements.

---

_Label `internal` added by @ibraheemdev on 2025-07-11 22:58_

---

_@ibraheemdev reviewed on 2025-07-11 22:59_

---

_Review comment by @ibraheemdev on `Cargo.toml`:175 on 2025-07-11 22:59_

This also transitively enables the `preserve_order`, which [is apparently faster](https://epage.github.io/blog/2025/07/toml-09/), not really sure why it's not enabled by default.

---

_@ibraheemdev reviewed on 2025-07-11 23:00_

---

_Review comment by @ibraheemdev on `crates/uv-build-frontend/src/lib.rs`:514 on 2025-07-11 23:00_

I tried to use `toml` for the initial parse to avoid the `toml_edit` overhead, but that seemed to regress error messages because `toml::Value` does not retain the original raw spans. Probably not very relevant because we're doing a double parse anyways.

---

_@ibraheemdev reviewed on 2025-07-11 23:01_

---

_Review comment by @ibraheemdev on `crates/uv/tests/it/edit.rs`:10906 on 2025-07-11 23:01_

Fairly sure this is a bug in `toml_edit`, because the actual parsed `DocumentMut` drops the comments. Opened https://github.com/toml-rs/toml/issues/1007.

---

_@ibraheemdev reviewed on 2025-07-12 04:18_

---

_Review comment by @ibraheemdev on `Cargo.toml`:175 on 2025-07-12 04:18_

Apparently it's for build times, but indexmap is already in our tree.

---

_Comment by @ibraheemdev on 2025-07-12 05:36_

With a ~3000 line lockfile, this makes a no-op `uv lock` ~20% faster.
```console
taskset 1 hyperfine "uv lock" "uv-new lock" --warmup 10 --runs 500

Benchmark 1: uv lock
  Time (mean ± σ):       8.2 ms ±   0.3 ms    [User: 6.0 ms, System: 2.2 ms]
  Range (min … max):     8.0 ms …  10.3 ms    500 runs
 
Benchmark 2: uv-new lock
  Time (mean ± σ):       6.9 ms ±   0.1 ms    [User: 4.9 ms, System: 1.9 ms]
  Range (min … max):     6.8 ms …   7.2 ms    500 runs
 
Summary
  uv-new lock ran
    1.20 ± 0.05 times faster than uv lock
```

We could probably add benchmarks for lockfile se/deserialization.

---

_@zanieb reviewed on 2025-07-17 16:20_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:628 on 2025-07-17 16:20_

Alas this looks like a regression

---

_@zanieb reviewed on 2025-07-17 16:21_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10906 on 2025-07-17 16:21_

This is a blocking regression, right?

---

_@zanieb reviewed on 2025-07-17 16:21_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:18425 on 2025-07-17 16:21_

This also looks like a regression.

---

_@ibraheemdev reviewed on 2025-07-17 16:28_

---

_Review comment by @ibraheemdev on `crates/uv/tests/it/edit.rs`:10906 on 2025-07-17 16:28_

I think so, yes. There have been user issues about this before.

---

_@zanieb reviewed on 2025-07-17 16:30_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:628 on 2025-07-17 16:30_

(though a minor one)

---

_Review comment by @charliermarsh on `crates/uv/tests/it/edit.rs`:10906 on 2025-07-21 01:40_

Looks like it was fixed in 0.23.2.

---

_@charliermarsh reviewed on 2025-07-21 01:40_

---

_@charliermarsh approved on 2025-07-21 01:41_

---

_Comment by @charliermarsh on 2025-07-21 01:45_

I defer to @zanieb on whether they want to ship with those error messages + file issues in `toml`, or file issues then wait.

---

_@zanieb reviewed on 2025-07-21 13:17_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:628 on 2025-07-21 13:17_

See https://github.com/toml-rs/toml/issues/1013

---

_@zanieb reviewed on 2025-07-21 13:18_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:18425 on 2025-07-21 13:18_

See https://github.com/toml-rs/toml/issues/1012

---

_Merged by @zanieb on 2025-07-21 13:18_

---

_Closed by @zanieb on 2025-07-21 13:18_

---

_Branch deleted on 2025-07-21 13:18_

---
