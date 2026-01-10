```yaml
number: 522
title: "Parse `SimpleJson` into categorized data in the client"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/client-response-type
created_at: 2023-11-30T20:20:12Z
updated_at: 2023-12-07T17:04:49Z
url: https://github.com/astral-sh/uv/pull/522
synced_at: 2026-01-10T15:44:44Z
```

# Parse `SimpleJson` into categorized data in the client

---

_Pull request opened by @zanieb on 2023-11-30 20:20_

Extends #517 with a suggestion from @konstin to parse the `SimpleJson` into an intermediate type `SimpleMetadata(BTreeMap<Version, VersionFiles>)` before converting to a `VersionMap`. This reduces the number of times we need to parse the response. Additionally, we cache the parsed response now instead of `SimpleJson`.

`VersionFiles` stores two vectors with `WheelFilename`/`SourceDistFilename` and `File` tuples. These can be iterated over together or separately. A new enum `DistFilename` was added to capture the `SourceDistFilename` and `WheelFilename` variants allowing iteration over both vectors.


---

_@zanieb reviewed on 2023-11-30 20:39_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/finder.rs`:150 on 2023-11-30 20:39_

I would recommend viewing [with whitespace ignored](https://github.com/astral-sh/puffin/pull/522/files?diff=unified&w=1)

---

_Review requested from @konstin by @zanieb on 2023-11-30 21:05_

---

_@konstin reviewed on 2023-12-01 11:46_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:394 on 2023-12-01 11:46_

Could we split wheels and source dists?

```rust
pub struct VersionFiles {
    wheels: Vec<(WheelFilename, File)>,
    source_dists: Vec<(SourceDistFilename, File)>,
}

pub struct SimpleMetadata(BTreeMap<Version, VersionFiles>);
```

This will improve deserialization, we don't have to try both parsers, and make the file selection logic clear (go through all wheels to find the best, if none, find any source dist with matching requires python).

---

_@konstin reviewed on 2023-12-01 11:47_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:394 on 2023-12-01 11:47_

My motivation is to put the `SimpleMetadata` into the cache instead of the `SimpleJson`, for both html index and perf reason.

---

_@zanieb reviewed on 2023-12-04 15:11_

---

_Review comment by @zanieb on `crates/puffin-client/src/registry_client.rs`:394 on 2023-12-04 15:11_

Hm I tried this first but it made some other code pretty duplicative. I can look into it again.

---

_Marked ready for review by @zanieb on 2023-12-06 20:29_

---

_Comment by @zanieb on 2023-12-06 20:30_

Konsti mentioned wanting to use this type in the cache as well, impl in https://github.com/astral-sh/puffin/pull/522/commits/46577048aab828e51439ad715bd9acb83f1743d3

---

_@zanieb reviewed on 2023-12-06 20:41_

---

_Review comment by @zanieb on `crates/puffin-client/src/registry_client.rs`:394 on 2023-12-06 20:41_

impl in https://github.com/astral-sh/puffin/pull/522/commits/00e63fcc8f71f4aeeb8aeda318543a3a17c8d29e

---

_Comment by @zanieb on 2023-12-06 21:40_

A very rough benchmark with `resolve-many` shows no improvement 

```
time cargo run --bin puffin-dev --release -- resolve-many --cache-dir cache-docker-no-build --no-build ./pypi_top_8k_flat.txt

main cold cache

    102.47s user 13.76s system 433% cpu 26.840 total

main hot cache

    104.07s user 9.73s system 746% cpu 15.245 total

branch cold cache

    120.44s user 14.73s system 495% cpu 27.292 total

branch hot cache

    124.87s user 10.65s system 793% cpu 17.070 total
```



---

_@konstin approved on 2023-12-06 21:44_

---

_Comment by @konstin on 2023-12-06 21:46_

I think this is the right change independent of benchmarks

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/lib.rs`:16 on 2023-12-07 00:49_

Nit: I'd say `try_from_filename` is a fine name.

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/source_dist.rs`:5 on 2023-12-07 00:51_

I believe this is an optional dependency, so it needs to be added optionally here. Like:

```rust
#[derive(Debug, PartialEq, Eq, Clone)]
#[cfg_attr(feature = "serde", derive(Serialize, Deserialize))]
```

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/finder.rs`:188 on 2023-12-07 00:54_

Is this necessary given that we're using `into_iter`?

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:419 on 2023-12-07 00:56_

Why do we need to index by version? Don't we end up iterating over all of the entires anyway? And `WheelFilename` and `SourceDistFilename` both contain `Version`.

---

_@charliermarsh reviewed on 2023-12-07 01:02_

The change looks nice, though I'm slightly torn on this because I suspect it should actually be slower in the installer, since we now have to iterate over the entire `SimpleJson` to build the `SimpleMetadata`, whereas today in the installer, we reverse-iterate over the list of files and break as soon as we pass the highest-compatible version.

In the worst case (imagine a distribution with a ton of versions, and we're using the latest), that means we're iterating over a huge number of files and versions to parse, allocate, etc. in a way that won't ultimately be necessary. I wish we could produce some benchmarks to decide one way or another.

(We _do_ iterate over all files and versions in the _resolver_ today, but not in the installer.)

---

_@zanieb reviewed on 2023-12-07 04:35_

---

_Review comment by @zanieb on `crates/puffin-client/src/registry_client.rs`:419 on 2023-12-07 04:35_

I defer to @konstin but it does let us do things like..

```rust
        for (version, files) in metadata.into_iter().rev() {
            // If we iterated past the first-compatible version, break.
            if best_version
                .as_ref()
                .is_some_and(|best_version| *best_version != version)
            {
                break;
            }

            // If the version does not satisfy the requirement, continue.
            if !requirement.is_satisfied_by(&version) {
                continue;
            }

            // Find the most-compatible wheel
            for (wheel, file) in files.wheels { ... }

             // Find the most-compatible sdist, if no wheel was found.
            if best_wheel.is_none() {
                for (sdist, file) in files.source_dists { ... }
            }
```

Perhaps for sorting?

---

_@konstin reviewed on 2023-12-07 13:19_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:419 on 2023-12-07 13:19_

We don't strongly need to; I thought it was useful because it gives us a list of versions upfront and makes it possible to split by versions if we want later.

---

_Comment by @charliermarsh on 2023-12-07 14:44_

I will do some basic benchmarking of this in the installer to understand whether the concern I brought up matters at all.

---

_@charliermarsh approved on 2023-12-07 16:47_

I did some benchmarking and couldn't see any noticeable changes so this looks good to me. I'll write up a separate comment outlining my benchmarking approach.

---

_Comment by @charliermarsh on 2023-12-07 16:50_

To benchmark...

- I first built a release build on main: `git co main && cargo build --release && mv ./target/release/puffin ./target/release/main`
- I then built a a release build on this branch with `cargo build --release`.
- I then modified `./scripts/benchmarks/sync.sh` to compare the two release builds:

```sh
#!/usr/bin/env bash

###
# Benchmark the installer against `pip`.
#
# Example usage:
#
#   ./scripts/benchmarks/sync.sh ./scripts/benchmarks/requirements.txt
###

set -euxo pipefail

TARGET=${1}

###
# Installation with a cold cache.
###
hyperfine --runs 50 --warmup 10 \
    --prepare "virtualenv --clear .venv" \
    "./target/release/main pip-sync ${TARGET} --no-cache" \
    --prepare "virtualenv --clear .venv" \
    "./target/release/puffin pip-sync ${TARGET} --no-cache"

###
# Installation with a warm cache, similar to blowing away and re-creating a virtual environment.
###
hyperfine --runs 50 --warmup 10 \
    --prepare "virtualenv --clear .venv" \
    "./target/release/main pip-sync ${TARGET}" \
    --prepare "virtualenv --clear .venv" \
    "./target/release/puffin pip-sync ${TARGET}"

###
# Installation with all dependencies already installed (no-op).
###
hyperfine --runs 50 --warmup 10 \
    --setup "virtualenv --clear .venv && source .venv/bin/activate" \
    "./target/release/main pip-sync ${TARGET}" \
    "./target/release/puffin pip-sync ${TARGET}"
```

- I then ran `./scripts/benchmarks/sync.sh ./scripts/benchmarks/requirements.txt`.

I tweaked `requirements.txt` to include different packages (e.g., `boto3` which has a ton of versions). In general, I didn't see any noticeable effect. The cached install (second benchmark) was _maybe_ a bit faster, and the uncached install (first benchmark) was _maybe_ a bit slower (but if so, within 1%).


---

_Merged by @zanieb on 2023-12-07 17:04_

---

_Closed by @zanieb on 2023-12-07 17:04_

---

_Branch deleted on 2023-12-07 17:04_

---
