---
number: 5964
title: "chore(deps): Update compatible (dev)"
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: master
head: renovate/compatible-(dev)
created_at: 2025-04-01T03:31:32Z
updated_at: 2025-04-07T23:15:14Z
url: https://github.com/clap-rs/clap/pull/5964
synced_at: 2026-01-07T13:12:20-06:00
---

# chore(deps): Update compatible (dev)

---

_Pull request opened by @renovate on 2025-04-01 03:31_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [color-print](https://gitlab.com/dajoha/color-print) | dev-dependencies | patch | `0.3.6` -> `0.3.7` |
| [divan](https://redirect.github.com/nvzqz/divan) | dev-dependencies | patch | `0.1.15` -> `0.1.18` |
| [jiff](https://redirect.github.com/BurntSushi/jiff) | dev-dependencies | patch | `0.2.3` -> `0.2.6` |
| [rustversion](https://redirect.github.com/dtolnay/rustversion) | dev-dependencies | patch | `1.0.19` -> `1.0.20` |
| [trybuild](https://redirect.github.com/dtolnay/trybuild) | dev-dependencies | patch | `1.0.103` -> `1.0.104` |

---

### Release Notes

<details>
<summary>nvzqz/divan (divan)</summary>

### [`v0.1.18`](https://redirect.github.com/nvzqz/divan/blob/HEAD/CHANGELOG.md#0118---2025-04-05)

[Compare Source](https://redirect.github.com/nvzqz/divan/compare/v0.1.17...v0.1.18)

##### Added

-   Support for [`cargo-nextest`](https://nexte.st) running benchmarks as tests.

-   `prelude` module for simplifying imports of \[`#[bench]`]\[bench_attr],
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

### [`v0.2.6`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#026-TBD)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.5...0.2.6)

\===========
TODO

Enhancements:

-   [#&#8203;315](https://redirect.github.com/BurntSushi/jiff/issues/315):
    Add support for automatically finding the tzdb on Illumos.

Bug fixes:

-   [#&#8203;305](https://redirect.github.com/BurntSushi/jiff/issues/305):
    Fixed `Zoned` rounding on days with DST time zone transitions.
-   [#&#8203;309](https://redirect.github.com/BurntSushi/jiff/issues/309):
    Fixed bug where `TimeZone::preceding` could omit historical time zone
    transitions for time zones that have eliminated DST in the present.
-   [#&#8203;312](https://redirect.github.com/BurntSushi/jiff/issues/312):
    Fixed `nth_weekday_in_month`, where it would sometimes incorrectly return an
    error.

### [`v0.2.5`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#025-2025-03-22)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.4...0.2.5)

\==================
This release updates Jiff's bundled copy of the \[IANA Time Zone Database] to
`2025b`. See the [`2025b` release announcement][2025b release announcement] for more details.

Enhancements:

-   [#&#8203;300](https://redirect.github.com/BurntSushi/jiff/pull/300):
    Update `jiff-tzdb` to `2025b`.

[`2025b` release announcement]: https://lists.iana.org/hyperkitty/list/tz-announce@iana.org/thread/6JVHNHLB6I2WAYTQ75L6KEPEQHFXAJK3/

### [`v0.2.4`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#024-2025-03-10)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.3...0.2.4)

\==================
This is another small release that fixes a problem where Jiff could break
builds if they relied on inference for integer comparisons. Specifically, Jiff
uses internal trait impls to make comparing its internal ranged integers
more convenient. But Rust the language has no concept of "internal" trait
impls, and thus this can impact type inference. If code was written in a way
that relies on a singular trait impl that is available, then adding Jiff to
the project can cause it to break.

This isn't arguably Jiff's fault per se, but since these trait impls were just
about internal convenience and not essential to Jiff's design, we adopt a
pragmatic approach and just remove them.

Bug fixes:

-   [#&#8203;293](https://redirect.github.com/BurntSushi/jiff/issues/293):
    Remove internal trait impls that can cause breaks due to inference failures.

</details>

<details>
<summary>dtolnay/rustversion (rustversion)</summary>

### [`v1.0.20`](https://redirect.github.com/dtolnay/rustversion/releases/tag/1.0.20)

[Compare Source](https://redirect.github.com/dtolnay/rustversion/compare/1.0.19...1.0.20)

-   Documentation improvements

</details>

<details>
<summary>dtolnay/trybuild (trybuild)</summary>

### [`v1.0.104`](https://redirect.github.com/dtolnay/trybuild/releases/tag/1.0.104)

[Compare Source](https://redirect.github.com/dtolnay/trybuild/compare/1.0.103...1.0.104)

-   Documentation improvements

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yMDcuMSIsInVwZGF0ZWRJblZlciI6IjM5LjIyNy4zIiwidGFyZ2V0QnJhbmNoIjoibWFzdGVyIiwibGFiZWxzIjpbXX0=-->


---
