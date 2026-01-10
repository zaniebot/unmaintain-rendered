```yaml
number: 605
title: Download, build, and install in a single pipeline phase
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/batch
created_at: 2023-12-10T01:31:10Z
updated_at: 2023-12-11T15:42:31Z
url: https://github.com/astral-sh/uv/pull/605
synced_at: 2026-01-10T15:44:44Z
```

# Download, build, and install in a single pipeline phase

---

_Pull request opened by @charliermarsh on 2023-12-10 01:31_

## Summary

At present, we have two separate phases within the installation pipeline related to populating wheels into the cache. The first phase downloads the distribution, and then builds any source distributions into wheels; the second phase unzips all the built wheels into the cache.

This PR merges those two phases into one, such that we seamlessly download, build, and unzip wheels in one pass. This is more efficient, since we can start unzipping while we build. It also ensures that if the install _fails_ partway through, we don't end up with a bunch of downloaded wheels that we never had a chance to unzip. The code is also much simpler.

The main downside is that the user-facing feedback isn't as granular, since we only have one phase and one progress bar for what was originally three distinct phases.

Closes https://github.com/astral-sh/puffin/issues/571.

## Test Plan

I ran the benchmark script on two separate requirements files, and saw a 7% and 31% speedup respectively:

```text
+ TARGET=./scripts/benchmarks/requirements.txt
+ hyperfine --runs 100 --warmup 10 --prepare 'virtualenv --clear .venv' './target/release/main pip-sync ./scripts/benchmarks/requirements.txt --no-cache' --prepare 'virtualenv --clear .venv' './target/release/puffin pip-sync ./scripts/benchmarks/requirements.txt --no-cache'
Benchmark 1: ./target/release/main pip-sync ./scripts/benchmarks/requirements.txt --no-cache
  Time (mean ± σ):     269.4 ms ±  33.0 ms    [User: 42.4 ms, System: 117.5 ms]
  Range (min … max):   221.7 ms … 446.7 ms    100 runs

Benchmark 2: ./target/release/puffin pip-sync ./scripts/benchmarks/requirements.txt --no-cache
  Time (mean ± σ):     250.6 ms ±  28.3 ms    [User: 41.5 ms, System: 127.4 ms]
  Range (min … max):   207.6 ms … 336.4 ms    100 runs

Summary
  './target/release/puffin pip-sync ./scripts/benchmarks/requirements.txt --no-cache' ran
    1.07 ± 0.18 times faster than './target/release/main pip-sync ./scripts/benchmarks/requirements.txt --no-cache'
```

```text
+ TARGET=./scripts/benchmarks/requirements-large.txt
+ hyperfine --runs 100 --warmup 10 --prepare 'virtualenv --clear .venv' './target/release/main pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache' --prepare 'virtualenv --clear .venv' './target/release/puffin pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache'
Benchmark 1: ./target/release/main pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache
  Time (mean ± σ):      5.053 s ±  0.354 s    [User: 1.413 s, System: 6.710 s]
  Range (min … max):    4.584 s …  6.333 s    100 runs

Benchmark 2: ./target/release/puffin pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache
  Time (mean ± σ):      3.845 s ±  0.225 s    [User: 1.364 s, System: 6.970 s]
  Range (min … max):    3.482 s …  4.715 s    100 runs

Summary
  './target/release/puffin pip-sync ./scripts/benchmarks/requirements-large.txt --no-cache' ran
```

---

_Renamed from "Batch installs" to "Download, build, and install in a single pipeline phase" by @charliermarsh on 2023-12-10 01:31_

---

_Review requested from @konstin by @charliermarsh on 2023-12-10 01:31_

---

_Review comment by @konstin on `crates/puffin-installer/src/downloader.rs`:27 on 2023-12-11 12:30_

We should either add `Dist` to both `Unzip` and `Fetch` or leave both to the caller

---

_Review comment by @konstin on `crates/puffin-installer/src/downloader.rs`:90 on 2023-12-11 12:33_

This is good for now, for later i think we'll want to fine tune to individual limits per task (e.g. 32 downloads or whatever pypi likes in parallel but 2 times the number of cores but builds)

---

_Review comment by @konstin on `crates/puffin-installer/src/downloader.rs`:114 on 2023-12-11 12:34_

Should we out the in flight tracking also around `get_or_build_wheel`?

---

_Review comment by @konstin on `crates/puffin-installer/src/downloader.rs`:31 on 2023-12-11 12:51_

Would it make sense to merge the `Downloader` and the `DistributionDatabase`?

---

_@konstin approved on 2023-12-11 12:51_

Nice speedup!

---

_@charliermarsh reviewed on 2023-12-11 15:15_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:114 on 2023-12-11 15:15_

It's not _critical_ because if two builds happen, the wheels will atomically overwrite (whereas with zips, we'll error due to the directory clash), but yes, I think we should.

---

_@charliermarsh reviewed on 2023-12-11 15:15_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:31 on 2023-12-11 15:15_

Honestly, I think the separation is good.

---

_@charliermarsh reviewed on 2023-12-11 15:17_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:90 on 2023-12-11 15:17_

I don't quite know how to enforce that.

---

_Comment by @charliermarsh on 2023-12-11 15:19_

Added TODOs and resolved the ones that I could.

---

_Review comment by @konstin on `crates/puffin-installer/src/downloader.rs`:90 on 2023-12-11 15:21_

I'd put some tokio semaphores on the build context that hand out permits.

---

_@konstin reviewed on 2023-12-11 15:21_

---

_@charliermarsh reviewed on 2023-12-11 15:22_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/downloader.rs`:90 on 2023-12-11 15:22_

Cool, makes sense :) I added a TODO for now.

---

_Merged by @charliermarsh on 2023-12-11 15:42_

---

_Closed by @charliermarsh on 2023-12-11 15:42_

---

_Branch deleted on 2023-12-11 15:42_

---
