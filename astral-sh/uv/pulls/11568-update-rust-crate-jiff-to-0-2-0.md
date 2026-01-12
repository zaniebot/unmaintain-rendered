```yaml
number: 11568
title: Update Rust crate jiff to 0.2.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/jiff-0.x
created_at: 2025-02-17T01:51:55Z
updated_at: 2025-02-17T02:49:22Z
url: https://github.com/astral-sh/uv/pull/11568
synced_at: 2026-01-12T16:09:53Z
```

# Update Rust crate jiff to 0.2.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [jiff](https://redirect.github.com/BurntSushi/jiff) | workspace.dependencies | minor | `0.1.14` -> `0.2.0` |

---

### Release Notes

<details>
<summary>BurntSushi/jiff (jiff)</summary>

### [`v0.2.1`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#021-2025-02-16)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.0...0.2.1)

\==================
This release includes a massive number of optimizations that significantly
improves performance in some cases. If you had a workload whose performance
with Jiff was underwhelming, please give it a try. I welcome questions via
\[Discussions on GitHub].

This release also provides a new API, `Timestamp::constant`, for constructing
`Timestamp` values in a `const` context.

Enhancements:

-   [#&#8203;235](https://redirect.github.com/BurntSushi/jiff/discussions/235),
    [#&#8203;255](https://redirect.github.com/BurntSushi/jiff/discussions/255),
    [#&#8203;266](https://redirect.github.com/BurntSushi/jiff/pull/266):
    A massive number of performance improvements. Jiff should now generally be
    faster than `chrono` and `time`.
-   [#&#8203;263](https://redirect.github.com/BurntSushi/jiff/issues/263):
    Add `Timestamp::constant` for constructing timestamps in `const` context.

### [`v0.2.0`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#020-2025-02-10)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.1.29...0.2.0)

\==================
This is a new semver incompatible release of Jiff. It contains several breaking
changes. I expect most users of Jiff to be able to upgrade without any changes.
The fundamental API organization of Jiff has not changed.

Some of the highlights of this release include reducing footguns and better
ecosystem integration.

For reducing footguns, APIs on `Span` will no longer implicitly assume that
days are always 24 hours long. And `Span` no longer implements `PartialEq` or
`Eq` (instead favoring `span.fieldwise()` to create a value that supports naive
fieldwise comparison). Moreover, when using `TimeZone::system()` (perhaps via
`Zoned::now()`), if the system time zone could not be detected, then a special
`Etc/Unknown` time zone will be used instead. This avoids erroring, but also
surfaces itself to make it clearer that something has (perhaps) gone wrong.

As for ecosystem integration, this release coincides with the publication
of the \[`jiff-icu`], \[`jiff-sqlx`] and \[`jiff-diesel`] crates. `jiff-icu`
integrates with the \[ICU4X project], and is now the recommended way to use
Jiff to work with non-Gregorian calendars or to localize datetimes for end
users. `jiff-sqlx` and `jiff-diesel` provide wrapper types that implement the
necessary traits to make it ergonomic to store and retrieve Jiff values in a
database using \[SQLx] or \[Diesel], respectively.

Unless something unexpected happens, my plan is for the next breaking change
release to be Jiff 1.0 in about 6 months. Once Jiff 1.0 is out, I plan to
commit to it indefinitely.

**BREAKING CHANGES**:

This is an exhaustive list of breaking changes. Changes with the bolded
**RUNTIME** prefix are changes that will not be caught by the Rust compiler.
That is, they are changes in runtime behavior.

-   [#&#8203;28](https://redirect.github.com/BurntSushi/jiff/issues/28):
    The deprecated `intz` routines on `Zoned`, `Timestamp`, `civil::DateTime` and
    `civil::Date` have been removed. You can use `in_tz` instead. This change was
    made because many found the name `intz` to be unclear.
-   [#&#8203;32](https://redirect.github.com/BurntSushi/jiff/issues/32):
    The `PartialEq` and `Eq` trait implementations on `Span` have been removed.
    Ideally these shouldn't have been used, but if you do need them, please use
    `Span::fieldwise` to create a `SpanFieldwise`, which does have the `PartialEq`
    and `Eq` traits implemented. These were removed on `Span` itself because they
    made it very easy to commit subtle bugs.
-   [#&#8203;36](https://redirect.github.com/BurntSushi/jiff/issues/36):
    Turn panics during `Timestamp::saturing_add` into errors. Callers adding
    spans that are known to contain units of hours or smaller are guaranteed that
    this will not return an error.
-   **RUNTIME** [#&#8203;48](https://redirect.github.com/BurntSushi/jiff/issues/48):
    On `Span` APIs, days are no longer silently assumed to always be 24 hours when
    a relative datetime is not provided. Instead, to perform operations on units
    of days or bigger, callers must either provide a relative date or opt into
    invariant 24-hour days with `SpanRelativeTo::days_are_24_hours`. Shortcuts have
    been added to the span builders. For example, `SpanTotal::days_are_24_hours`.
-   **RUNTIME** [#&#8203;147](https://redirect.github.com/BurntSushi/jiff/issues/147):
    Change the behavior of the deprecated `%V` conversion specifier in
    `jiff::fmt::strtime` from formatting an IANA time zone identifier to formatting
    an ISO 8601 week number. To format an IANA time zone identifier, use `%Q` or
    `%:Q` (which were introduced in `jiff 0.1`).
-   **RUNTIME** [#&#8203;212](https://redirect.github.com/BurntSushi/jiff/issues/212):
    When parsing into a `Zoned` with a civil time corresponding to a gap, we treat
    all offsets as invalid and return an error. Previously, we would accept the
    offset as given. This brings us into line with Temporal's behavior. For
    example, previously Jiff accepted `2006-04-02T02:30-05[America/Indiana/Vevay]`
    but will now return an error. This is desirable for cases where a datetime in
    the future is serialized before a change in the daylight saving time rules.
    For more examples, see `jiff::fmt::temporal::DateTimeParser::offset_conflict`
    for details on how to change Jiff's default behavior. This behavior change also
    applies to `tz::OffsetConflict::PreferOffset`.
-   **RUNTIME** [#&#8203;213](https://redirect.github.com/BurntSushi/jiff/issues/213):
    Tweak the semantics of `tz::TimeZoneDatabase` so that it only initializes one
    internal tzdb instead of trying to find as many as possible. It is unlikely
    that you'll be impacted by this change, but it's meant to make the semantics
    be a bit more sensible. (In `jiff 0.1`, it was in theory possible for one tz
    lookup to succeed in the system zoneinfo and then another tz lookup to fail
    in zoneinfo but succeed automatically via the bundled copy. But this seems
    confusing and possibly not desirable. Hence the change.)
-   [#&#8203;218](https://redirect.github.com/BurntSushi/jiff/issues/218):
    In order to make naming a little more consistent between `Zoned`
    and `civil::Date`, the `civil::Date::to_iso_week_date` and
    `civil::ISOWeekDate::to_date` APIs were renamed to `civil::Date::iso_week_date`
    and `civil::ISOWeekDate::date`.
-   [#&#8203;220](https://redirect.github.com/BurntSushi/jiff/issues/220):
    Remove `Span::to_duration` for converting a `Span` to a `std::time::Duration`
    and rename `Span::to_jiff_duration` to `Span::to_duration`. This prioritizes
    `SignedDuration` as the "primary" non-calendar duration type in Jiff. And makes
    it more consistent with APIs like `Zoned::duration_since`. For non-calendar
    spans, the `TryFrom<Span> for std::time::Duration` still exists. For calendar
    durations, use `Span::to_duration` and then convert the `SignedDuration` to
    `std::time::Duration`. Additionally, `Timestamp::from_jiff_duration` and
    `Timestamp::as_jiff_duration` were renamed to `Timestamp::from_duration` and
    `Timestamp::as_duration`, respectively. The old deprecated routines on the
    unsigned `std::time::Duration` have been removed.
-   [#&#8203;221](https://redirect.github.com/BurntSushi/jiff/issues/221):
    Change the type of the value yielded by the `jiff::tz::TimeZoneNameIter`
    iterator from `String` to `jiff::tz::TimeZoneName`. This opaque type is more
    API evolution friendly. To access the string, either use `TimeZoneName`'s
    `Display` trait implementation, or its `as_str` method.
-   [#&#8203;222](https://redirect.github.com/BurntSushi/jiff/issues/222):
    Split `TimeZone::to_offset` into two methods. One that just returns the
    offset, and another, `TimeZone::to_offset_info`, which includes the offset,
    DST status and time zone abbreviation. The extra info is rarely needed and
    is sometimes more costly to compute. Also, make the lifetime of the time
    zone abbreviation returned by `TimeZoneTransition::abbreviation` tied to
    the transition instead of the time zone (for future API flexibility, likely
    in core-only environments). This change was overall motivated by wanting to
    do less work in the common case (where we only need the offset), and for
    reducing the size of a `TimeZone` considerably in core-only environments.
    Callers previously using `TimeZone::to_offset` to get DST status and time zone
    abbreviation should now use `TimeZone::to_offset_info`.
-   **RUNTIME** [#&#8203;230](https://redirect.github.com/BurntSushi/jiff/issues/230):
    When `TimeZone::system()` cannot find a system configured time zone, `jiff
    0.1` would automatically fall back to `TimeZone::UTC` (with a WARN-level log
    message). In `jiff 0.2`, the fall back is now to `TimeZone::unknown()`, which
    has a special `Etc/Unknown` identifier (as specified by Unicode and reserved by
    the IANA time zone database). The fallback otherwise still behaves as if it
    were `TimeZone::UTC`. This helps surface error conditions related to finding
    the system time zone without causing unrecoverable failure.

Enhancements:

-   [#&#8203;136](https://redirect.github.com/BurntSushi/jiff/issues/136):
    When the special `SpanRelativeTo::days_are_24_hours()` marker is used, weeks
    will also be treated as invariant. That is, seven 24-hour days. In all cases,
    working with years and months still requires a relative date.
-   [#&#8203;228](https://redirect.github.com/BurntSushi/jiff/issues/228):
    It is now possible to forcefully use a bundled copy of the IANA time zone
    database without relying on disabling crate features. This can be done by
    enabling the `tzdb-bundle-always` crate feature and explicitly creating a
    `jiff::tz::TimeZoneDatabase::bundled()` database. Once in hand, you must use
    APIs like `TimeZoneDatabase::get` to create a `TimeZone` and avoid any APIs
    that implicitly use the global time zone database (like `Timestamp::in_tz` or
    even `Zoned::from_str`).
-   [#&#8203;238](https://redirect.github.com/BurntSushi/jiff/pull/238):
    Add integration with the ICU4X project via the \[`jiff-icu`] crate. `jiff-icu`
    provides traits for easily converting between datetime types defined in Jiff
    and datetime types defined in ICU4X.
-   [#&#8203;240](https://redirect.github.com/BurntSushi/jiff/pull/240):
    Add integration with the SQLx project via the \[`jiff-sqlx`] crate. `jiff-sqlx`
    provides wrapper types that implement the necessary traits in SQLx for
    reasonably ergonomic integration. This includes PostgreSQL and SQLite support,
    but not MySQL support. (It's not clear if it's possible at present to provide
    MySQL supprot fro SQLx for datetime types outside of SQLx itself.)
-   [#&#8203;241](https://redirect.github.com/BurntSushi/jiff/pull/241):
    Add integration with the Diesel project via the \[`jiff-diesel`] crate.
    `jiff-diesel` provides wrapper types that implement the necessary traits in
    Diesel for reasonably ergonomic integration. This includes MySQL, PostgreSQL
    and SQLite support.

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNjcuMSIsInVwZGF0ZWRJblZlciI6IjM5LjE2Ny4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-02-17 01:51_

---

_Merged by @charliermarsh on 2025-02-17 02:49_

---

_Closed by @charliermarsh on 2025-02-17 02:49_

---

_Branch deleted on 2025-02-17 02:49_

---
