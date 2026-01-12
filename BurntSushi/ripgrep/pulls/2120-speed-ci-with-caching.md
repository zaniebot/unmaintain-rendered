```yaml
number: 2120
title: Speed CI with caching
type: pull_request
state: closed
author: AustinWise
labels: []
assignees: []
base: master
head: austin/CacheCI2
created_at: 2022-01-09T05:16:46Z
updated_at: 2022-02-06T22:49:23Z
url: https://github.com/BurntSushi/ripgrep/pull/2120
synced_at: 2026-01-12T18:23:14Z
```

# Speed CI with caching

---

_@AustinWise_

Cache both Cargo dependencies and build output using the [example settings for actions/cache](https://github.com/actions/cache/blob/main/examples.md#rust---cargo).

This is similar to #719 which cached builds in Travis and Appveyor.

# Cache speedup

This speeds up *most* builds:

|CI Leg|[Before](https://github.com/AustinWise/ripgrep/actions/runs/1673024260)|[After](https://github.com/AustinWise/ripgrep/actions/runs/1673043494)|
|--|--|--|
|pinned|3m 11s|1m 54s|
|stable|3m 24s|1m 15s|
|beta|2m 51s|1m 30s|
|nightly|2m 42s|1m 39s|
|nightly-musl|4m 56s|3m 9s|
|nightly-32|4m 29s|2m 35s|
|nightly-mips|8m 29s|**9m 22s**|
|macos|4m 23s|2m 3s|
|win-msvc|5m 32s|2m 17s|
|win-gnu|5m 52s|3m 1s|

The only build configuration that regressed between these two runs was the
slowest: `nightly-mips`. The "run tests" step was the slow part. I did another
[uncached run](https://github.com/AustinWise/ripgrep/runs/4751425936?check_suite_focus=true)
that took the same amount of time on "run tests" as the slow cached one, so
maybe this is noise?

# Cache size

There is a [10GB limit](https://github.com/actions/cache#cache-limits) for the
cache:

> A repository can have up to 10GB of caches. Once the 10GB limit is reached, older caches will be evicted based on when the cache was last accessed. Caches that are not accessed within the last week will also be evicted.

The size of the cached artifacts current vary from 256MB to 410MB. Assuming a
worst case of 500MB per leg, this would allow for caching of 20 different
builds. This is greater than the current 11 builds in the CI matrix.

# Cache invalidation

The cache key will be invalidated whenever the Cargo.lock changes.

# Soundness

Rust has suffered from
[miscompilation during incremental builds](https://blog.rust-lang.org/2021/05/10/Rust-1.52.1.html)
in the past. So that is something to consider when deciding if the speedup is
worth the potential for spurious errors or lack of errors during CI runs.

---

_Comment by @AustinWise on 2022-01-09 05:23_

Note the the nightly-arm build is failing. I assume it not because of my change, as it is failing even with an empty cache.

---

_Comment by @AustinWise on 2022-02-06 22:48_

I'm not sure now that this is best way to do the caching. Rustup uses a slightly different approach. See here:

https://github.com/rust-lang/rustup/blob/master/.github/workflows/linux-builds-on-pr.yaml

The main differences are:
* Separate caches for `.cargo` and `target` directories
* Cleans the `.cargo` cache to be smaller

I'm going to close this PR until I have more confidence about the right approach.

---

_Closed by @AustinWise on 2022-02-06 22:49_

---
