```yaml
number: 10972
title: Update Rust crate jiff to v0.1.27
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/jiff-0.x-lockfile
created_at: 2025-01-27T01:57:32Z
updated_at: 2025-01-27T02:21:03Z
url: https://github.com/astral-sh/uv/pull/10972
synced_at: 2026-01-12T16:09:36Z
```

# Update Rust crate jiff to v0.1.27

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [jiff](https://redirect.github.com/BurntSushi/jiff) | workspace.dependencies | patch | `0.1.24` -> `0.1.27` |

---

### Release Notes

<details>
<summary>BurntSushi/jiff (jiff)</summary>

### [`v0.1.27`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0127-2025-01-25)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.1.26...0.1.27)

\===================
This is a small release with a bug fix for precision loss in some cases when
doing arithmetic on `Timestamp` or `Zoned`.

Bug fixes:

-   [#&#8203;223](https://redirect.github.com/BurntSushi/jiff/issues/223):
    Fix the check for fractional seconds before taking the fast path.

### [`v0.1.26`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0126-2025-01-23)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.1.25...0.1.26)

\===================
This is a small release with another deprecation and a new API for doing
prefix parsing via `strptime`. There's also a bug fix for a corner case
when dealing with daylight saving time gaps with the `Zoned::with` API.

Deprecations:

-   [#&#8203;210](https://redirect.github.com/BurntSushi/jiff/pull/210):
    Deprecate `ISOWeekDate::to_date` in favor of `ISOWeekDate::date`.

Enhancements:

-   [#&#8203;209](https://redirect.github.com/BurntSushi/jiff/issues/209):
    Add `fmt::strtime::BrokenDownTime::parse_prefix` for parsing only a prefix.

Bug fixes:

-   [#&#8203;211](https://redirect.github.com/BurntSushi/jiff/issues/211):
    Fix unintuitive behavior of `Zoned::with` when time falls in a DST gap.

### [`v0.1.25`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0125-2025-01-21)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.1.24...0.1.25)

\===================
This release contains a number of deprecations in preparation for a `jiff 0.2`
release. The deprecations are meant to facilitate a smoother transition. The
deprecations, when possible, come with new APIs that will permit users to write
forward compatible code that will work in both `jiff 0.1` and `jiff 0.2`.

This release also includes a handful of new conversion specifiers in Jiff's
`strftime` and `strptime` APIs. This improves compatibility with the analogous
implementation with GNU libc.

Deprecations:

-   [#&#8203;28](https://redirect.github.com/BurntSushi/jiff/issues/28):
    The `intz` methods on `Zoned`, `Timestamp`, `civil::DateTime` and `civil::Date`
    have been deprecated in favor of `in_tz`.
-   [#&#8203;32](https://redirect.github.com/BurntSushi/jiff/issues/32):
    The `Eq` and `PartialEq` trait implementations on `Span` have been deprecated
    in favor of using the new `SpanFieldwise` type.
-   [#&#8203;48](https://redirect.github.com/BurntSushi/jiff/issues/48):
    Silently assuming days are always 24 hours in some `Span` APIs has now been
    deprecated. This will become an error in `jiff 0.2`. To continue assuming
    days are 24 hours without a relative reference date, you can use the new
    `SpanRelativeTo::days_are_24_hours` API. In `jiff 0.1`, you'll seen a
    WARN-level log message emitted if you're code will be broken by `jiff 0.2`.
-   [#&#8203;147](https://redirect.github.com/BurntSushi/jiff/issues/147):
    Both `%V` and `%:V` have been deprecated in favor of `%Q` and `%:Q`. In
    `jiff 0.2`, `%V` will correspond to the ISO 8601 week number and `%:V` will
    result in an error. This change was made to improve compatibility with other
    `strtime` implementations. `%V` and `%:V` continue to correspond to IANA
    time zone identifiers in `jiff 0.1`, but using them for parsing or formatting
    will result in a WARN-level deprecation message.

Enhancements:

-   [#&#8203;147](https://redirect.github.com/BurntSushi/jiff/issues/147):
    Adds a number of new conversion specifiers to Jiff's `strftime` and
    `strptime` APIs. Specifically, `%C`, `%G`, `%g`, `%j`, `%k`, `%l`, `%n`, `%R`,
    `%s`, `%t`, `%U`, `%u`, `%W`, `%w`. Their behavior should match the
    corresponding specifiers in GNU libc.

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xMjUuMSIsInVwZGF0ZWRJblZlciI6IjM5LjEyNS4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-01-27 01:57_

---

_Merged by @charliermarsh on 2025-01-27 02:21_

---

_Closed by @charliermarsh on 2025-01-27 02:21_

---

_Branch deleted on 2025-01-27 02:21_

---
