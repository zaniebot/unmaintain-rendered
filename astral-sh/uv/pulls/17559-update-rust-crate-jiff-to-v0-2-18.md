```yaml
number: 17559
title: Update Rust crate jiff to v0.2.18
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/jiff-0.x-lockfile
created_at: 2026-01-19T01:29:10Z
updated_at: 2026-01-19T14:50:16Z
url: https://github.com/astral-sh/uv/pull/17559
synced_at: 2026-01-19T15:25:20Z
```

# Update Rust crate jiff to v0.2.18

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [jiff](https://redirect.github.com/BurntSushi/jiff) | workspace.dependencies | patch | `0.2.16` → `0.2.18` |

---

### Release Notes

<details>
<summary>BurntSushi/jiff (jiff)</summary>

### [`v0.2.18`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0218-2026-01-05)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.17...0.2.18)

\===================
This release ships a sizeable refactor to the RFC 2822, RFC 9110, RC
3339, RFC 9557, ISO 8601 and friendly format printers. Specifically,
they are now all monomorphic internally (instead of being generic over
`jiff::fmt::Write`) and write to uninitialized buffers. This improves
runtime performance (sometimes dramatically so), and to a more modest
degree, decreases binary size and improves compile times.

This release also includes a bug fix where `DateTime::MIN.to_zoned(..)`
could panic.

Enhancements:

- [#&#8203;460](https://redirect.github.com/BurntSushi/jiff/pull/460):
  Improve runtime performance and binary size of RFC 2822 printer.
- [#&#8203;461](https://redirect.github.com/BurntSushi/jiff/pull/461):
  Tweak behavior of printing min/max offsets in RFC 2822 and Temporal printers.
- [#&#8203;462](https://redirect.github.com/BurntSushi/jiff/pull/462):
  Export fallible constructors for `jiff::SignedDuration`.
- [#&#8203;465](https://redirect.github.com/BurntSushi/jiff/pull/465):
  Improve runtime performance and binary size of the "friendly" duration printer.
- [#&#8203;468](https://redirect.github.com/BurntSushi/jiff/pull/468):
  Improve runtime performance and binary size of the Temporal ISO 8601 duration
  printer.
- [#&#8203;470](https://redirect.github.com/BurntSushi/jiff/pull/470):
  Improve runtime performance and binary size of the Temporal ISO 8601 datetime
  printer.
- [#&#8203;474](https://redirect.github.com/BurntSushi/jiff/pull/474):
  Improve runtime performance and binary size of Jiff's `strftime`
  implementation.
- [#&#8203;477](https://redirect.github.com/BurntSushi/jiff/pull/477):
  Fix a bug where time zone lookups for `civil::DateTime::MIN` could panic.

### [`v0.2.17`](https://redirect.github.com/BurntSushi/jiff/blob/HEAD/CHANGELOG.md#0217-2025-12-24)

[Compare Source](https://redirect.github.com/BurntSushi/jiff/compare/0.2.16...0.2.17)

\===================
This release contains binary size improvements to Jiff, more succinct error
messages and some new minor APIs.

While Jiff 1.0 is overdue, I've been doing a lot of experimenting with
improving Jiff's binary size and compile times. In particular, I want to spend
time doing this before Jiff 1.0 so that we don't box ourselves into a corner.
(For example, some binary size improvements may require minor API breaking
changes.)

In this release, Jiff has switched to structured error handling internally
in an effort to provide error predicates and also hopefully improve binary
sizes and compile times. Overall this didn't have as big of an impact on
binary sizes or compile times as I was hoping. I did take this opportunity to
make Jiff's error messages a bit more succinct. In many cases, this involved
de-duplicating some aspects of error messages and omitting user provided input
in the messages. If you feel like there is a significant decrease in error
message quality that isn't easily amended by callers providing additional
context themselves, please open an issue.

This release also updates Jiff's bundled copy of the \[IANA Time Zone Database]
to `2025c`. See the [`2025c` release announcement] for more details.

Enhancements:

- [#&#8203;412](https://redirect.github.com/BurntSushi/jiff/issues/412):
  Add `Display`, `FromStr`, `Serialize` and `Deserialize` trait implementations
  for `jiff::civil::ISOWeekDate`. These all use the ISO 8601 week date format.
- [#&#8203;418](https://redirect.github.com/BurntSushi/jiff/issues/418):
  Add some basic predicates to `jiff::Error` for basic error introspection.
- [#&#8203;453](https://redirect.github.com/BurntSushi/jiff/pull/453),
  [#&#8203;454](https://redirect.github.com/BurntSushi/jiff/pull/454):
  Switch to structured error handling internally.
- [#&#8203;456](https://redirect.github.com/BurntSushi/jiff/pull/456),
  [#&#8203;457](https://redirect.github.com/BurntSushi/jiff/pull/457),
  [#&#8203;458](https://redirect.github.com/BurntSushi/jiff/pull/458):
  Various improvements to binary size.

[`2025c` release announcement]: https://lists.iana.org/hyperkitty/list/tz-announce@iana.org/thread/TAGXKYLMAQRZRFTERQ33CEKOW7KRJVAK/

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Between 12:00 AM and 03:59 AM, only on Monday ( * 0-3 * * 1 ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi43NC41IiwidXBkYXRlZEluVmVyIjoiNDIuNzQuNSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-19 01:29_

---

_@zanieb approved on 2026-01-19 14:50_

---

_Merged by @zanieb on 2026-01-19 14:50_

---

_Closed by @zanieb on 2026-01-19 14:50_

---

_Branch deleted on 2026-01-19 14:50_

---
