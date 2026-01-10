```yaml
number: 10310
title: Update Rust crate jiff to v0.1.21
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/jiff-0.x-lockfile
created_at: 2025-01-06T00:59:50Z
updated_at: 2025-01-06T01:12:58Z
url: https://github.com/astral-sh/uv/pull/10310
synced_at: 2026-01-10T11:44:41Z
```

# Update Rust crate jiff to v0.1.21

---

_Pull request opened by @renovate on 2025-01-06 00:59_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [jiff](https://redirect.github.com/BurntSushi/jiff) | workspace.dependencies | patch | `0.1.16` -> `0.1.21` |

---

### Release Notes

<details>
<summary>BurntSushi/jiff (jiff)</summary>

### [`v0.1.21`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0121-2025-01-04)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.1.20...0.1.21)

\===================
This release includes a new API for setting the unit designator label in a
friendly formatted duration for zero-length durations.

Enhancements:

-   [#&#8203;192](https://redirect.github.com/BurntSushi/jiff/issues/192):
    Add option to the friendly printer for setting the unit when writing a
    zero-length duration.

### [`v0.1.20`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0120-2025-01-03)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.1.19...0.1.20)

\===================
This release inclues a new type, `Pieces`, in the `jiff::fmt::temporal`
sub-module. This exposes the individual components of a parsed Temporal
ISO 8601 datetime string. It allows users of Jiff to circumvent the checks
in the higher level parsing routines that prevent you from shooting yourself
in the foot.

For example, parsing into a `Zoned` will return an error for raw RFC 3339
timestamps like `2025-01-03T22:03-05` because there is no time zone annotation.
Without a time zone, Jiff cannot do time zone aware arithmetic and rounding.
Instead, such a datetime can only be parsed into a `Timestamp`. This lower
level `Pieces` API now permits users of Jiff to parse this string into its
component parts and assemble it into a `Zoned` if they so choose.

Enhancements:

-   [#&#8203;188](https://redirect.github.com/BurntSushi/jiff/issues/188):
    Add `fmt::temporal::Pieces` for granular datetime parsing and formatting.

### [`v0.1.19`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0119-2025-01-02)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.1.18...0.1.19)

\===================
This releases includes a UTF-8 related bug fix and a few enhancements.

Firstly, a `Span`'s default `Display` implementation now writes uppercase
unit designator labels. That means you'll get `P1Y2M3DT4H5M6S` instead
of `P1y2m3dT4h5m6s` by default. You can restore previous behavior via
`jiff::fmt::temporal::SpanPrinter::lowercase`. This change was made to improve
interoperability.

Secondly, `SignedDuration` now supports rounding via `SignedDuration::round`.
Note that it only supports rounding time units (hours or smaller). In order to
round with calendar units, you'll still need to use a `Span`.

Enhancements:

-   [#&#8203;130](https://redirect.github.com/BurntSushi/jiff/issues/130):
    Document value ranges for methods like `year`, `day`, `hour` and so on.
-   [#&#8203;187](https://redirect.github.com/BurntSushi/jiff/issues/187):
    Add a rounding API (for time units only) on `SignedDuration`.
-   [#&#8203;190](https://redirect.github.com/BurntSushi/jiff/issues/190):
    `Span` and `SignedDuration` now use uppercase unit designator labels in their
    default ISO 8601 `Display` implementation.

Bug fixes:

-   [#&#8203;155](https://redirect.github.com/BurntSushi/jiff/issues/155):
    Relax `strftime` format strings from ASCII-only to all of UTF-8.

### [`v0.1.18`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0118-2024-12-31)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.1.17...0.1.18)

\===================
This release includes a few minor enhancements. Namely, the ability to iterate
over time zone transitions (in the future or the past), and some improvements
to failure modes when `Timestamp` and `Span` arithmetic fails.

Enhancements:

-   [#&#8203;144](https://redirect.github.com/BurntSushi/jiff/issues/144):
    Add APIs for iterating over the transitions of a time zone.
-   [#&#8203;145](https://redirect.github.com/BurntSushi/jiff/issues/145):
    Improve docs and error messages around fallible `Timestamp` arithmetic.

### [`v0.1.17`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0117-2024-12-31)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.1.16...0.1.17)

\===================
This release enhances Jiff's support for `no_std` environments by making its
`alloc` feature optional. When `alloc` is disabled, only fixed offset time
zones are supported and error messages are significantly degraded. If you have
core-only use cases for Jiff, I'd love to hear about them on the issue tracker.

Enhancements:

-   [#&#8203;162](https://redirect.github.com/BurntSushi/jiff/issues/162):
    Support platforms that do not have atomics in `std`.
-   [#&#8203;168](https://redirect.github.com/BurntSushi/jiff/issues/168):
    Jiff now supports disabling the `alloc` feature, which enables core-only mode.
-   [#&#8203;169](https://redirect.github.com/BurntSushi/jiff/issues/169):
    Add `TimeZone::to_fixed_offset` for accessing an invariant offset if possible.

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS44NS4wIiwidXBkYXRlZEluVmVyIjoiMzkuODUuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-01-06 00:59_

---

_Merged by @charliermarsh on 2025-01-06 01:12_

---

_Closed by @charliermarsh on 2025-01-06 01:12_

---

_Branch deleted on 2025-01-06 01:12_

---
