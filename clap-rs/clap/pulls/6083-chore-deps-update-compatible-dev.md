```yaml
number: 6083
title: "chore(deps): Update compatible (dev)"
type: pull_request
state: open
author: renovate
labels: []
assignees: []
base: master
head: renovate/compatible-(dev)
created_at: 2025-08-01T02:27:14Z
updated_at: 2025-12-11T22:47:19Z
url: https://github.com/clap-rs/clap/pull/6083
synced_at: 2026-01-12T16:14:17Z
```

# chore(deps): Update compatible (dev)

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [divan](https://redirect.github.com/nvzqz/divan) | dev-dependencies | patch | `0.1.15` -> `0.1.21` |
| [jiff](https://redirect.github.com/BurntSushi/jiff) | dev-dependencies | patch | `0.2.15` -> `0.2.16` |
| [rustversion](https://redirect.github.com/dtolnay/rustversion) | dev-dependencies | patch | `1.0.21` -> `1.0.22` |
| [semver](https://redirect.github.com/dtolnay/semver) | dev-dependencies | patch | `1.0.26` -> `1.0.27` |
| [snapbox](https://redirect.github.com/assert-rs/snapbox) | dev-dependencies | patch | `0.6.21` -> `0.6.23` |
| [trybuild](https://redirect.github.com/dtolnay/trybuild) | dev-dependencies | patch | `1.0.106` -> `1.0.114` |
| [trycmd](https://redirect.github.com/assert-rs/snapbox) | dev-dependencies | patch | `0.15.9` -> `0.15.11` |

---

### Release Notes

<details>
<summary>nvzqz/divan (divan)</summary>

### [`v0.1.21`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0121---2025-04-09)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.20...v0.1.21)

##### Fixed

- `Divan::skip_exact` behaved incorrectly in `v0.1.19`.

##### Changed

- Improved handling of internal code around filters and those responsible for
  sacking the people who have just been sacked have been sacked.

### [`v0.1.20`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0120---2025-04-09)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.19...v0.1.20)

##### Fixed

- `Divan::skip_regex` accidentally dropped
  [`regex_lite::Regex`](https://docs.rs/regex-lite/latest/regex_lite/struct.Regex.html)
  and behaved incorrectly in `v0.1.19`.

### [`v0.1.19`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0119---2025-04-09)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.18...v0.1.19)

##### Fixed

- \[`cargo-nextest`] no longer skips benchmarks with argument parameters (\[[#&#8203;75](https://redirect.github.com/nvzqz/divan/issues/75)]).

##### Changed

- Organized positive and negative filters into a split buffer.

### [`v0.1.18`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0118---2025-04-05)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.17...v0.1.18)

##### Added

- Support for \[`cargo-nextest`] running benchmarks as tests.

- \[`prelude`] module for simplifying imports of \[`#[bench]`]\[bench\_attr],
  \[`#[bench_group]`]\[bench\_group\_attr], \[`black_box`], \[`black_box_drop`],
  \[`AllocProfiler`], \[`Bencher`], and \[`Divan`].

- Support `wasi` and `emscripten` targets.

### [`v0.1.17`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0117---2024-12-04)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.16...v0.1.17)

##### Changed

- Set \[MSRV] to 1.80 for \[`LazyLock`] and new `size_of` prelude import.

- Reduced thread pool memory usage by many kilobytes by using rendezvous
  channels instead of array-based channels.

### [`v0.1.16`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0116---2024-11-25)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.15...v0.1.16)

##### Added

- Thread pool for reusing threads across multi-threaded benchmarks. The result
  is that when running Divan benchmarks under a sampling profiler, the
  profiler's output will be cleaner and easier to understand. (\[[#&#8203;37](https://redirect.github.com/nvzqz/divan/issues/37)])

- Track the maximum number of allocations during a benchmark.

##### Changed

- Make private `Arg::get` trait method not take `self`, so that text editors
  don't recommend using it. (\[[#&#8203;59](https://redirect.github.com/nvzqz/divan/issues/59)])

- Cache `BenchOptions` using `LazyLock` instead of `OnceLock`, saving space and
  simplifying the implementation.

</details>

<details>
<summary>BurntSushi/jiff (jiff)</summary>

### [`v0.2.16`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0216-2025-11-07)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.15...0.2.16)

\===================
This release contains a number of enhancements and bug fixes that have accrued
over the last few months. Most are small polishes. A couple of the bug fixes
apply to panics that could occur when parsing invalid `TZ` strings or invalid
`strptime` format strings.

Also, parsing into a `Span` should now be much faster (for both the ISO 8601
and "friendly" duration formats).

Enhancements:

- [#&#8203;298](https://redirect.github.com/BurntSushi/jiff/issues/298):
  Add Serde helpers for (de)serializing `std::time::Duration` values.
- [#&#8203;396](https://redirect.github.com/BurntSushi/jiff/issues/396):
  Add `Sub` and `Add` trait implementations for `Zoned` (in addition to the
  already existing trait implementations for `&Zoned`).
- [#&#8203;397](https://redirect.github.com/BurntSushi/jiff/pull/397):
  Add `BrokenDownTime::set_meridiem` and ensure it overrides the hour when
  formatting.
- [#&#8203;409](https://redirect.github.com/BurntSushi/jiff/pull/409):
  Switch dependency on `serde` to `serde_core`. This should help speed up
  compilation times in some cases.
- [#&#8203;430](https://redirect.github.com/BurntSushi/jiff/pull/430):
  Add new `Zoned::series` API, making it consistent with the same API on other
  datetime types.
- [#&#8203;432](https://redirect.github.com/BurntSushi/jiff/pull/432):
  When `lenient` mode is enabled for `strftime`, Jiff will no longer error when
  the formatting string contains invalid UTF-8.
- [#&#8203;432](https://redirect.github.com/BurntSushi/jiff/pull/432):
  Formatting of `%y` and `%g` no longer fails based on the specific year value.
- [#&#8203;432](https://redirect.github.com/BurntSushi/jiff/pull/432):
  Parsing of `%s` is now a bit more consistent with other fields. Moreover,
  `BrokenDownTime::{to_timestamp,to_zoned}` will now prefer timestamps parsed
  with `%s` over any other fields that have been parsed.
- [#&#8203;433](https://redirect.github.com/BurntSushi/jiff/pull/433):
  Allow parsing just a `%s` into a `Zoned` via the `Etc/Unknown` time zone.

Bug fixes:

- [#&#8203;386](https://redirect.github.com/BurntSushi/jiff/issues/386):
  Fix a bug where `2087-12-31T23:00:00Z` in the `Africa/Casablanca` time zone
  could not be round-tripped (because its offset was calculated incorrectly as
  a result of not handling "permanent DST" POSIX time zones).
- [#&#8203;407](https://redirect.github.com/BurntSushi/jiff/issues/407):
  Fix a panic that occurred when parsing an empty string as a POSIX time zone.
- [#&#8203;410](https://redirect.github.com/BurntSushi/jiff/issues/410):
  Fix a panic that could occur when parsing `%:` via `strptime` APIs.
- [#&#8203;414](https://redirect.github.com/BurntSushi/jiff/pull/414):
  Update some parts of the documentation to indicate that `TimeZone::unknown()`
  is a fallback for `TimeZone::system()` (instead of the `jiff 0.1` behavior of
  using `TimeZone::UTC`).
- [#&#8203;423](https://redirect.github.com/BurntSushi/jiff/issues/423):
  Fix a panicking bug when reading malformed TZif data.
- [#&#8203;426](https://redirect.github.com/BurntSushi/jiff/issues/426):
  Fix a panicking bug when parsing century (`%C`) via `strptime`.
- [#&#8203;445](https://redirect.github.com/BurntSushi/jiff/pull/445):
  Fixed bugs with parsing durations like `-9223372036854775808s`
  and `-PT9223372036854775808S`.

Performance:

- [#&#8203;445](https://redirect.github.com/BurntSushi/jiff/pull/445):
  Parsing into `Span` or `SignedDuration` is now a fair bit faster in some cases.

</details>

<details>
<summary>dtolnay/rustversion (rustversion)</summary>

### [`v1.0.22`](https://redirect.github.com/dtolnay/rustversion/releases/tag/1.0.22)

[Compare Source](https://redirect.github.com/dtolnay/rustversion/compare/1.0.21...1.0.22)

- Turn off clippy incompatible\_msrv in rustversion-conditional code ([#&#8203;63](https://redirect.github.com/dtolnay/rustversion/issues/63))

</details>

<details>
<summary>dtolnay/semver (semver)</summary>

### [`v1.0.27`](https://redirect.github.com/dtolnay/semver/releases/tag/1.0.27)

[Compare Source](https://redirect.github.com/dtolnay/semver/compare/1.0.26...1.0.27)

- Switch serde dependency to serde\_core ([#&#8203;333](https://redirect.github.com/dtolnay/semver/issues/333))

</details>

<details>
<summary>assert-rs/snapbox (snapbox)</summary>

### [`v0.6.23`](https://redirect.github.com/assert-rs/snapbox/compare/snapbox-v0.6.22...snapbox-v0.6.23)

[Compare Source](https://redirect.github.com/assert-rs/snapbox/compare/snapbox-v0.6.22...snapbox-v0.6.23)

### [`v0.6.22`](https://redirect.github.com/assert-rs/snapbox/compare/snapbox-v0.6.21...snapbox-v0.6.22)

[Compare Source](https://redirect.github.com/assert-rs/snapbox/compare/snapbox-v0.6.21...snapbox-v0.6.22)

</details>

<details>
<summary>dtolnay/trybuild (trybuild)</summary>

### [`v1.0.114`](https://redirect.github.com/dtolnay/trybuild/releases/tag/1.0.114)

[Compare Source](https://redirect.github.com/dtolnay/trybuild/compare/1.0.113...1.0.114)

- Normalize indentation of rustc suggestion lines ([#&#8203;319](https://redirect.github.com/dtolnay/trybuild/issues/319))

### [`v1.0.113`](https://redirect.github.com/dtolnay/trybuild/releases/tag/1.0.113)

[Compare Source](https://redirect.github.com/dtolnay/trybuild/compare/1.0.112...1.0.113)

- Update `target-triple` dependency to v1

### [`v1.0.112`](https://redirect.github.com/dtolnay/trybuild/releases/tag/1.0.112)

[Compare Source](https://redirect.github.com/dtolnay/trybuild/compare/1.0.111...1.0.112)

- Normalize indentation of consteval notes ([#&#8203;318](https://redirect.github.com/dtolnay/trybuild/issues/318))

### [`v1.0.111`](https://redirect.github.com/dtolnay/trybuild/releases/tag/1.0.111)

[Compare Source](https://redirect.github.com/dtolnay/trybuild/compare/1.0.110...1.0.111)

- Normalize dependency crate's version in filepaths ([#&#8203;316](https://redirect.github.com/dtolnay/trybuild/issues/316))

### [`v1.0.110`](https://redirect.github.com/dtolnay/trybuild/releases/tag/1.0.110)

[Compare Source](https://redirect.github.com/dtolnay/trybuild/compare/1.0.109...1.0.110)

- Fix permissions\_set\_readonly\_false warning ([#&#8203;312](https://redirect.github.com/dtolnay/trybuild/issues/312))

### [`v1.0.109`](https://redirect.github.com/dtolnay/trybuild/releases/tag/1.0.109)

[Compare Source](https://redirect.github.com/dtolnay/trybuild/compare/1.0.108...1.0.109)

- Support read-only Cargo.lock files ([#&#8203;310](https://redirect.github.com/dtolnay/trybuild/issues/310), thanks [@&#8203;MingweiSamuel](https://redirect.github.com/MingweiSamuel))

### [`v1.0.108`](https://redirect.github.com/dtolnay/trybuild/releases/tag/1.0.108)

[Compare Source](https://redirect.github.com/dtolnay/trybuild/compare/1.0.107...1.0.108)

- More right-aligned line number fixes ([#&#8203;309](https://redirect.github.com/dtolnay/trybuild/issues/309))

### [`v1.0.107`](https://redirect.github.com/dtolnay/trybuild/releases/tag/1.0.107)

[Compare Source](https://redirect.github.com/dtolnay/trybuild/compare/1.0.106...1.0.107)

- Recognize right aligned line numbers ([#&#8203;308](https://redirect.github.com/dtolnay/trybuild/issues/308), [rust-lang/rust#144609](https://redirect.github.com/rust-lang/rust/pull/144609))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS40Ni4zIiwidXBkYXRlZEluVmVyIjoiNDIuNDIuMiIsInRhcmdldEJyYW5jaCI6Im1hc3RlciIsImxhYmVscyI6W119-->


---
