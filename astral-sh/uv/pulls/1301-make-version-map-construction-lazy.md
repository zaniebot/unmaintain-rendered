```yaml
number: 1301
title: make version map construction lazy
type: pull_request
state: merged
author: BurntSushi
labels:
  - performance
assignees: []
merged: true
base: main
head: ag/version-map-lazy
created_at: 2024-02-14T19:53:09Z
updated_at: 2024-02-15T13:10:33Z
url: https://github.com/astral-sh/uv/pull/1301
synced_at: 2026-01-12T16:04:34Z
```

# make version map construction lazy

---

_@BurntSushi_

This PR refactors `VersionMap` to be inherently lazy in its
conversion of `VersionFiles` (part of `SimpleMetadata`) to
`PrioritizedDistribution`. The motivation for doing this arose out of
the zero-copy deserialization work on `SimpleMetadata`. Namely, before
this PR, `VersionMap::from_metadata` would immediately and eagerly
deserialize a full `SimpleMetadata` into memory. This negated many of
the benefits of zero-copy deserialization in the first place.

In this PR, we do a lot less work in `VersionMap::from_metadata`.
Namely, we pre-populate a map with all of the versions eagerly
deserialized, but otherwise only leave a stub for where the
distribution was previously stored. When the distribution for a version
number is requested, we check if the corresponding distribution has
already been created (via a `OnceLock`) and use it if so. Otherwise, we
construct it then and there on demand.

There is some complexity here where we need to deal with
`FlatDistribution` and its merging into `VersionFiles` to create a
`PrioritizedDistribution`. There is also still some filtering logic
(that perhaps @zanieb will fully excise soon) that we retain as well.

In general, this is good for about a 1.3x speed-up:

```
$ hyperfine -w10 --runs 50 "puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" "puffin-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" ; A bart
Benchmark 1: puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     148.0 ms ±   5.2 ms    [User: 354.8 ms, System: 312.1 ms]
  Range (min … max):   140.3 ms … 172.2 ms    50 runs

Benchmark 2: puffin-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     110.6 ms ±   3.4 ms    [User: 138.2 ms, System: 216.1 ms]
  Range (min … max):   104.2 ms … 118.5 ms    50 runs

Summary
  puffin-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null ran
    1.34 ± 0.06 times faster than puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
```

Profiling seems to suggest that `VersionMap` construction is no longer
using a significant chunk of time. This is important because there are
in theory things we can do to make resolution even lazier. For example,
we don't necessarily need to deserialize an entire `VersionFiles`
for each version requested. We could only deserialize the `File`s we
actually need. But this would require more refactoring and it isn't
clear that it's worth doing.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-14 19:53_

---

_Review request for @charliermarsh removed by @BurntSushi on 2024-02-14 19:53_

---

_Review requested from @konstin by @BurntSushi on 2024-02-14 19:53_

---

_Review request for @konstin removed by @BurntSushi on 2024-02-14 19:53_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-14 19:53_

---

_Review requested from @zanieb by @BurntSushi on 2024-02-14 19:53_

---

_Review requested from @konstin by @BurntSushi on 2024-02-14 19:54_

---

_Label `performance` added by @zanieb on 2024-02-14 21:07_

---

_@zanieb reviewed on 2024-02-14 22:00_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/candidate_selector.rs`:193 on 2024-02-14 22:00_

What's this new filtering for?

---

_@zanieb reviewed on 2024-02-14 22:07_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/version_map.rs`:422 on 2024-02-14 22:07_

I'm not entirely sure what our write patterns look like, but is this safe in our asynchronous contexts? i.e. could this be called from an asynchronous context and cause a synchronous block?

---

_@zanieb approved on 2024-02-14 22:08_

Makes sense!

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/version_map.rs`:200 on 2024-02-15 00:42_

What's an example?

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/version_map.rs`:231 on 2024-02-15 00:42_

Why use a named field here but not below?

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/version_map.rs`:251 on 2024-02-15 00:53_

`version` repeated here.

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:193 on 2024-02-15 00:55_

My guess (to confirm my understanding): the map has a key for every _version_, but it's possible that we can't actually find a matching distribution for that version once we've lazily materialized the options. For example, maybe you have a version that was uploaded after `exclude_newer` -- so the version exists in the map, but once you try to materialize it, you find that there aren't any matching distributions, and so this returns `None`.

---

_@charliermarsh approved on 2024-02-15 00:55_

---

_@zanieb reviewed on 2024-02-15 01:47_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/candidate_selector.rs`:193 on 2024-02-15 01:47_

That makes sense to me. This should disappear with my work.

---

_@BurntSushi reviewed on 2024-02-15 12:26_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/candidate_selector.rs`:193 on 2024-02-15 12:26_

Yeah that's right @charliermarsh. This also accounts for cases where a `PrioritizedDistribution` can't be converted into a `ResolvableDist`. (I'm unsure if those happen in practice, but the types permit it.)

---

_@BurntSushi reviewed on 2024-02-15 12:48_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/version_map.rs`:422 on 2024-02-15 12:48_

Good question.

It should be fine. Tokio does provide a [async-ified version of `OnceCell`/`OnceLock`](https://docs.rs/tokio/1.5.0/tokio/sync/struct.OnceCell.html), but that's more for cases where your initialization function itself wants to be `async`. Otherwise, it is [generally okay to use blocking synchronization primitives within async code](https://docs.rs/tokio/1.5.0/tokio/sync/struct.Mutex.html#which-kind-of-mutex-should-you-use). The important caveat here is to make sure you aren't trying to hold locks across await points. But the use of "blocking" synchronization primitives themselves is okay. (tokio's mutex for example, [uses a `std::sync::Mutex` internally](https://github.com/tokio-rs/tokio/blob/b32826bc937a34e4d871c89bb2c3711ed3e20cdc/tokio/src/loom/std/mutex.rs#L3-L6).) We don't need any async stuff here. The lazy construction is just pure data transformation.

More broadly, lots of libraries (including some of my own) uses synchronization primitives like this internally. If this were a problem, I think the `regex` crate would be blowing up Rust servers everywhere haha.

---

_@BurntSushi reviewed on 2024-02-15 12:52_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/version_map.rs`:200 on 2024-02-15 12:52_

I was trying not to write examples here haha because I knew Zanie was going to be touching this code and potentially moving some of the filtering logic out of a `VersionMap`. But I can add one concrete thing here at least: [converting a `PrioritizedDistribution` into a `ResolvableDist`](https://github.com/astral-sh/puffin/blob/a0d12d1a9e92fa1f5d11a973a2d7b79d17e63dc5/crates/distribution-types/src/prioritized_distribution.rs#L157-L179) might fail if there are no compatible wheels or source dists.

At time of writing, it might also fail because of `exclude_newer` or `no_binary`.

---

_@BurntSushi reviewed on 2024-02-15 12:53_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/version_map.rs`:200 on 2024-02-15 12:53_

To be clear, there shouldn't be any change here from the status quo.

---

_@BurntSushi reviewed on 2024-02-15 12:56_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/version_map.rs`:231 on 2024-02-15 12:56_

No huge reason. I suppose it is an odd asymmetry. I'll drop the named field here.

---

_Merged by @BurntSushi on 2024-02-15 13:10_

---

_Closed by @BurntSushi on 2024-02-15 13:10_

---

_Branch deleted on 2024-02-15 13:10_

---
