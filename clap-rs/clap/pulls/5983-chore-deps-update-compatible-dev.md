```yaml
number: 5983
title: "chore(deps): Update compatible (dev)"
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: master
head: renovate/compatible-(dev)
created_at: 2025-05-01T02:51:39Z
updated_at: 2025-05-01T22:28:00Z
url: https://github.com/clap-rs/clap/pull/5983
synced_at: 2026-01-12T16:14:17Z
```

# chore(deps): Update compatible (dev)

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [divan](https://redirect.github.com/nvzqz/divan) | dev-dependencies | patch | `0.1.15` -> `0.1.21` |
| [jiff](https://redirect.github.com/BurntSushi/jiff) | dev-dependencies | patch | `0.2.6` -> `0.2.11` |

---

### Release Notes

<details>
<summary>nvzqz/divan (divan)</summary>

### [`v0.1.21`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0121---2025-04-09)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.20...v0.1.21)

##### Fixed

-   `Divan::skip_exact` behaved incorrectly in `v0.1.19`.

##### Changed

-   Improved handling of internal code around filters and those responsible for
    sacking the people who have just been sacked have been sacked.

### [`v0.1.20`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0120---2025-04-09)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.19...v0.1.20)

##### Fixed

-   `Divan::skip_regex` accidentally dropped
    [`regex_lite::Regex`](https://docs.rs/regex-lite/latest/regex_lite/struct.Regex.html)
    and behaved incorrectly in `v0.1.19`.

### [`v0.1.19`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0119---2025-04-09)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.18...v0.1.19)

##### Fixed

-   \[`cargo-nextest`] no longer skips benchmarks with argument parameters (\[[#&#8203;75](https://redirect.github.com/nvzqz/divan/issues/75)]).

##### Changed

-   Organized positive and negative filters into a split buffer.

### [`v0.1.18`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0118---2025-04-05)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.17...v0.1.18)

##### Added

-   Support for \[`cargo-nextest`] running benchmarks as tests.

-   \[`prelude`] module for simplifying imports of \[`#[bench]`]\[bench_attr],
    \[`#[bench_group]`]\[bench_group_attr], \[`black_box`], \[`black_box_drop`],
    \[`AllocProfiler`], \[`Bencher`], and \[`Divan`].

-   Support `wasi` and `emscripten` targets.

### [`v0.1.17`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0117---2024-12-04)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.16...v0.1.17)

##### Changed

-   Set \[MSRV] to 1.80 for \[`LazyLock`] and new `size_of` prelude import.

-   Reduced thread pool memory usage by many kilobytes by using rendezvous
    channels instead of array-based channels.

### [`v0.1.16`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0116---2024-11-25)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.15...v0.1.16)

##### Added

-   Thread pool for reusing threads across multi-threaded benchmarks. The result
    is that when running Divan benchmarks under a sampling profiler, the
    profiler's output will be cleaner and easier to understand. (\[[#&#8203;37](https://redirect.github.com/nvzqz/divan/issues/37)])

-   Track the maximum number of allocations during a benchmark.

##### Changed

-   Make private `Arg::get` trait method not take `self`, so that text editors
    don't recommend using it. (\[[#&#8203;59](https://redirect.github.com/nvzqz/divan/issues/59)])

-   Cache `BenchOptions` using `LazyLock` instead of `OnceLock`, saving space and
    simplifying the implementation.

</details>

<details>
<summary>BurntSushi/jiff (jiff)</summary>

### [`v0.2.11`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0211-2025-05-01)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.10...0.2.11)

\===================
This release includes new APIs for customizing Jiff's `strtime` behavior along
with a few minor bug fixes. Jiff's `strtime` formatting API has also been
optimized. It's about twice as fast as it was.

This release also coincides with the publication of `jiff-icu 0.2.0-beta.2`,
which has support for `icu 2.0.0-beta.2`.

Enhancements:

-   [#&#8203;338](https://redirect.github.com/BurntSushi/jiff/pull/338):
    Add support for the `%c`, `%r`, `%X` and `%x` conversion specifiers.
-   [#&#8203;341](https://redirect.github.com/BurntSushi/jiff/issues/341):
    Add support for `%q` in `jiff::fmt::strtime` (prints quarter of year).
-   [#&#8203;342](https://redirect.github.com/BurntSushi/jiff/issues/342):
    Add support for `%::z` and `%:::z` in `jiff::fmt::strtime`.
-   [#&#8203;344](https://redirect.github.com/BurntSushi/jiff/issues/344):
    Add support for `%N` in `jiff::fmt::strtime` (alias for `%9f`).
-   [#&#8203;350](https://redirect.github.com/BurntSushi/jiff/issues/350):
    Add a "lenient" mode for `strtime` formatting APIs that ignores most errors.

Bug fixes:

-   [#&#8203;328](https://redirect.github.com/BurntSushi/jiff/issues/328):
    Document default precision behavior of `Display` impls for datetime types.
-   [#&#8203;340](https://redirect.github.com/BurntSushi/jiff/issues/340):
    Allow whitespace in more places in RFC 2822 parser (improves spec compliance).
-   [#&#8203;346](https://redirect.github.com/BurntSushi/jiff/issues/346):
    `TimeZone::get("UTC")` should now always return `TimeZone::UTC`.

Performance:

-   [#&#8203;338](https://redirect.github.com/BurntSushi/jiff/pull/338):
    Jiff's `strftime` APIs are now approximately twice as fast as they were.
    Performance should be comparable to `chrono` and `time`'s prebuilt APIs.

### [`v0.2.10`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0210-2025-04-21)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.9...0.2.10)

\===================
This release includes a bug fix for parsing `Tuesday` when using `%A` via
Jiff's `strptime` APIs. Specifically, it would recognize `Tueday` instead of
`Tuesday`.

Bug fixes:

-   [#&#8203;333](https://redirect.github.com/BurntSushi/jiff/issues/333):
    Fix typo in `strptime` parsing from `Tueday` to `Tuesday`.

### [`v0.2.9`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#029-2025-04-19)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.8...0.2.9)

\==================
This release includes a bug fix that, in debug mode, could result in datetime
types having different hashes for the same value. This could cause problems,
for example, if you are using datetimes as keys in a hash map. This problem
didn't exist when Jiff was compiled in release mode.

This release also improves the panic message shown when the `js` feature isn't
enabled and the current time is requested on `wasm32-unknown-unknown` targets.

Enhancements:

-   [#&#8203;296](https://redirect.github.com/BurntSushi/jiff/issues/296):
    Provide a better panic message when `Zoned::now()` fails on WASM.

Bug fixes:

-   [#&#8203;330](https://redirect.github.com/BurntSushi/jiff/issues/330):
    Fix bug where `Hash` on datetime types could yield different hash values for
    the same underlying date/time.

### [`v0.2.8`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#028-2025-04-13)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.7...0.2.8)

\==================
This release fixes a bug where the constructors on `SignedDuration`
for floating point durations could panic (in debug mode) or produce
incorrect results (in release mode). This bug only impacts users of
the `try_from_secs_{f32,f64}` and `from_secs_{f32,f64}` methods on
`SignedDuration`.

Enhancements:

-   [#&#8203;326](https://redirect.github.com/BurntSushi/jiff/pull/326):
    Add an alternate `Debug` impl for `SignedDuration` that only shows its second
    and nanosecond components (while using only one component when the other is
    zero).

Bug fixes:

-   [#&#8203;324](https://redirect.github.com/BurntSushi/jiff/issues/324):
    Fix a bug that could produce a panic or incorrect results in
    `SignedDuration::(try_)?from_secs_{f32,f64}`.

### [`v0.2.7`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#027-2025-04-13)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.6...0.2.7)

\==================
This release includes a bug fix that changes how an empty but set `TZ`
environment variable is interpreted (as indistinguishable from `TZ=UTC`).
This also includes a new enabled by default create feature, `perf-inline`,
which allows toggling Jiff's use of `inline(always)`. This may help improve
compile times or decrease binary size.

Enhancements:

-   [#&#8203;320](https://redirect.github.com/BurntSushi/jiff/pull/320):
    Remove some internal uses of generics to mildly improve compile times.
-   [#&#8203;321](https://redirect.github.com/BurntSushi/jiff/pull/321):
    Add `perf-inline` crate feature for controlling `inline(always)` annotations.

Bug fixes:

-   [#&#8203;311](https://redirect.github.com/BurntSushi/jiff/issues/311):
    Make `TZ=` indistinguishable from `TZ=UTC`.

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 5am on the first day of the month" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR is behind base branch, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://redirect.github.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/clap-rs/clap).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yNjQuMCIsInVwZGF0ZWRJblZlciI6IjM5LjI2NC4wIiwidGFyZ2V0QnJhbmNoIjoibWFzdGVyIiwibGFiZWxzIjpbXX0=-->


---
