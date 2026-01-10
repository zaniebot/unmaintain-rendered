```yaml
number: 7000
title: "Clean up \"performance allocators\" and \"performance flate2\" backends"
type: pull_request
state: closed
author: fasterthanlime
labels: []
assignees: []
base: main
head: fewer-deps
created_at: 2024-09-04T09:31:43Z
updated_at: 2024-09-25T15:41:42Z
url: https://github.com/astral-sh/uv/pull/7000
synced_at: 2026-01-10T12:53:37Z
```

# Clean up "performance allocators" and "performance flate2" backends

---

_Pull request opened by @fasterthanlime on 2024-09-04 09:31_

## Summary

@charliermarsh has long suspected local builds could be made faster by disabling things like: tikv-jemalloc/mimalloc, zlibng etc.

I'm going through the cargo dep tree looking at things that can be disabled locally.

## Methodology:

`production` cargo flag enables all the production stuff (good allocators, fast compression libs, etc.)

I measure fresh `cargo check` runs, like so:

```shell
rm -rf /tmp/timings; CARGO_TARGET_DIR=/tmp/timings cargo check -F production-memory-allocator --timings
```

Varying the `-F` to enable/disable the features

## FAQ

Q: Why only check `check`?

A: The benefits will trickle down to other subcommands (including test/nextest etc.) — check/clippy are super common while iterating. We can do larger checks near the end.

Q: Why only check cold builds?

A: Warm builds depend on a lot on which part of the code is touched — I'll optimize typical interactions later on.

---

_Converted to draft by @fasterthanlime on 2024-09-04 09:31_

---

_Comment by @fasterthanlime on 2024-09-04 09:48_

## Round 1: allocator

I introduced the `uv-production-memory-allocator` crate, which conditionally pulls in mimalloc (on Windows) and jemalloc (on other platforms, except OpenBSD). The extra crate works around limitations from cargo.

### Before: 430 units, total time 26.6s

![CleanShot 2024-09-04 at 11 46 59@2x](https://github.com/user-attachments/assets/996b4160-236d-4607-90ec-c47aaf30b65b)

### After: 425 units, total time 21s

![CleanShot 2024-09-04 at 11 48 24@2x](https://github.com/user-attachments/assets/cd568175-f468-45c7-aa4a-e2adedfcb2d1)



---

_Comment by @fasterthanlime on 2024-09-04 09:56_

## Round 2: miette's `fancy-no-backtrace`

Getting rid of these:

```shell
❯ cargo tree -p backtrace-ext
backtrace-ext v0.2.1
└── backtrace v0.3.73
    ├── addr2line v0.22.0
    │   └── gimli v0.29.0
    ├── cfg-if v1.0.0
    ├── libc v0.2.158
    ├── miniz_oxide v0.7.4
    │   └── adler v1.0.2
    ├── object v0.36.4
    │   └── memchr v2.7.4
    └── rustc-demangle v0.1.24
    [build-dependencies]
    └── cc v1.1.15
        ├── jobserver v0.1.32
        │   └── libc v0.2.158
        ├── libc v0.2.158
        └── shlex v1.3.0
```

Which are pulled by this:

```
❯ cargo tree -i backtrace-ext -e features
backtrace-ext v0.2.1
└── backtrace-ext feature "default"
    └── miette v7.2.0
        ├── miette feature "backtrace"
        │   └── miette feature "fancy"
        │       └── uv v0.4.4 (/Users/amos/bearcove/uv/crates/uv)
(cut)
```

### Before: 425 units, total time 21s

(See round 1)

### After: 415 units, total time 20.1s

![CleanShot 2024-09-04 at 11 56 18@2x](https://github.com/user-attachments/assets/45594e46-daa0-45e2-a9c3-149f21beb6c2)


---

_Comment by @zanieb on 2024-09-05 00:31_

Part of https://github.com/astral-sh/uv/issues/5711

---

_Marked ready for review by @fasterthanlime on 2024-09-06 17:19_

---

_Comment by @codspeed-hq[bot] on 2024-09-06 17:34_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/bearcove:fewer-deps)

### Merging #7000 will **not alter performance**

<sub>Comparing <code>bearcove:fewer-deps</code> (e89f275) with <code>main</code> (a541d6c)</sub>



### Summary

`✅ 14` untouched benchmarks






---

_Review requested from @konstin by @zanieb on 2024-09-07 15:34_

---

_Review requested from @BurntSushi by @zanieb on 2024-09-07 15:34_

---

_@charliermarsh reviewed on 2024-09-09 14:24_

---

_Review comment by @charliermarsh on `crates/uv/Cargo.toml`:107 on 2024-09-09 14:24_

I think we should keep `self-update` separate, since re-distributors will likely want to run with `--features production`, but _won't_ want to enable `self-update`. `self-update` is only applicable when you install via our dedicated installers, not via `brew`, etc.

---

_@fasterthanlime reviewed on 2024-09-09 14:54_

---

_Review comment by @fasterthanlime on `crates/uv/Cargo.toml`:107 on 2024-09-09 14:54_

Good point! I just re-separated them.

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:124 on 2024-09-09 14:59_

I think this and the reference on line 82 also need `self-update`, unless I'm misreading.

---

_@charliermarsh reviewed on 2024-09-09 14:59_

---

_@charliermarsh approved on 2024-09-09 15:00_

Makes sense to me! Probably good to get @BurntSushi eyes on it too since it will also affect profiling etc.

---

_Review comment by @fasterthanlime on `.github/workflows/build-binaries.yml`:124 on 2024-09-09 15:19_

Fixed! Also adjusted the comments.

---

_@fasterthanlime reviewed on 2024-09-09 15:19_

---

_@konstin approved on 2024-09-09 15:54_

---

_Comment by @konstin on 2024-09-09 15:55_

Nice!

---

_Comment by @BurntSushi on 2024-09-09 20:51_

The decrease in build times here is considerable and a nice win. Nice work.

In terms of how this is setup (feature configuration and the extra crates), that all makes sense to me.

Charlie predicted my main concern here: the default build is "divorced" from the build we ship to users. We kind of already have this problem with our Cargo profiles: our `release` profile has LTO enabled, but our `profiling` profile does not (and also keeps debug info around). The intent is that one is supposed to use `--profile profiling` locally when iterating on perf improvements, because otherwise, with LTO enabled, the build times are astronomical (multiple minutes even on my beefy workstation). This introduces a potential footgun in our development workflow: we measure the performance of binaries under a different configuration than what we ship to users.

I think this PR probably exacerbates that, because building local binaries for performance improvements will now also require, I believe, `--features production`. And I think this will likely be critical given what is being swapped out here (since things like `zlibng` and jemalloc are used specifically for perf reasons).

But the build improvements here are considerable. However...

> Warm builds depend on a lot on which part of the code is touched — I'll optimize typical interactions later on.

If the improvement here is ~23% on cold builds, do we have a sense of what kind of improvement we get on warm builds? My feeling is that for this class of improvement, the warm builds probably matter a lot more than cold builds. And for the dependencies removed here, I wouldn't expect them to be getting rebuilt all of the time. So I'd be curious if this change improves warm build times to the point of being worth the potential footgun here.

Separately, it's probably worth trying to find a different way of avoiding this footgun so that we can more confidently introduce a divergence between "dev builds" and "profiling builds" and "release builds."

---

_@zanieb reviewed on 2024-09-09 20:55_

---

_Review comment by @zanieb on `crates/uv/Cargo.toml`:107 on 2024-09-09 20:55_

Are we going to need to include a note to redistributors in the changelog for the `--features production` flag? Does that suggest we should reserve this change for a breaking release?

---

_Comment by @fasterthanlime on 2024-09-09 21:15_

> [...] because building local binaries for performance improvements will now also require, I believe, `--features production`.

I must admit I worked on this PR under the assumption that:

  * Most work on uv is focused on correctness and/or adding features (as opposed to performance optimizations)
  * Switching decompression backends / memory allocators is not a threat to correctness (since any deviation in behavior besides performance would be immediately spotted and reported as a bug to the respective maintainers)

Even if y'all decide you do not want to go down that road, there's something to salvage in that PR imho: the whole "shim crate to let cargo pick the dependencies/feature flags we want depending on the target platform" thing (and deduplicating the allocator setup between uv & uv-dev).

> do we have a sense of what kind of improvement we get on warm builds?

We don't! I guess let's measure that before deciding the fate of this PR first?

My experience in speeding up builds is that cargo rebuilds a lot more than it should, a lot more often than it should. And even when it _doesn't_ build something, just having a large dependency graph involves a lot of, well, fetching, hashing, etc. — as you're well aware, uv does that too!

I'll report back with data on incremental builds (including switching between check/clippy/build/running tests — some of which I suspect trashes the target dir and causes cargo to rebuild too much) but am already mentally prepared into salvaging this PR into a "mostly cleanups" one as I may have underestimated just how much of astral's work focuses on performance alone.

---

_Comment by @BurntSushi on 2024-09-09 21:23_

> Most work on uv is focused on correctness and/or adding features (as opposed to performance optimizations)

I think it comes in waves. I haven't done any perf work in a while since my focus has been on the functionality/correctness of the multi-platform resolver. But I have done a lot of perf work in the past and hope to do more in the future.

The cleanups/refactoring makes sense.

And looking at the impact on "warm" builds makes sense too. I know for me at least, I do a lot of work in `uv-resolver`. And I think the `pep508_rs` crate has seen a bit of activity lately because of markers. So those might be useful to test, speaking selfishly.

---

_Renamed from "Remove/disable deps in dev" to "Clean up "performance allocators" and "performance flate2" backends" by @fasterthanlime on 2024-09-23 14:27_

---

_Comment by @fasterthanlime on 2024-09-23 15:12_

This PR is now more about "simplifying the logic to pull in performance allocators/flate2 backends" (it still removes the 'backtrace' feature of miette), and less about removing dependencies by default.

The `performance` feature is now enabled by default, I suppose someone working on correctness only could use `--no-default-features` and get the perf improvements I originally had in mind, but as per the comments here: no defaults are changed, the packaging folks (Linux distros etc.) don't have to be notified, there shouldn't be any breaking change here, just fewer Cargo.toml magicks duplicated across uv and uv-dev.

cc @BurntSushi for a second review

---

_@BurntSushi approved on 2024-09-25 12:55_

LGTM!

---

_Comment by @fasterthanlime on 2024-09-25 13:00_

(I’m out sick + traveling so if someone wants to take over this PR to get it rebased and merged, I would owe them a drink, at the very least!)

---

_Comment by @konstin on 2024-09-25 14:30_

Rebase went through trivially (uv-publish addition), made a new PR due to permissions: #7686

---

_Closed by @konstin on 2024-09-25 15:41_

---
