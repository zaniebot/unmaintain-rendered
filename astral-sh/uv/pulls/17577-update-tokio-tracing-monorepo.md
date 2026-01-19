```yaml
number: 17577
title: Update tokio-tracing monorepo
type: pull_request
state: open
author: renovate
labels:
  - internal
  - "build:skip-release"
assignees: []
base: main
head: renovate/tokio-tracing-monorepo
created_at: 2026-01-19T01:32:00Z
updated_at: 2026-01-19T19:14:38Z
url: https://github.com/astral-sh/uv/pull/17577
synced_at: 2026-01-19T19:29:44Z
```

# Update tokio-tracing monorepo

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [tracing](https://tokio.rs) ([source](https://redirect.github.com/tokio-rs/tracing)) | workspace.dependencies | patch | `0.1.41` → `0.1.44` |
| [tracing-subscriber](https://tokio.rs) ([source](https://redirect.github.com/tokio-rs/tracing)) | workspace.dependencies | patch | `0.3.20` → `0.3.22` |

---

### Release Notes

<details>
<summary>tokio-rs/tracing (tracing)</summary>

### [`v0.1.44`](https://redirect.github.com/tokio-rs/tracing/releases/tag/tracing-0.1.44): tracing 0.1.44

[Compare Source](https://redirect.github.com/tokio-rs/tracing/compare/tracing-0.1.43...tracing-0.1.44)

##### Fixed

- Fix `record_all` panic ([#&#8203;3432])

##### Changed

- `tracing-core`: updated to 0.1.36 ([#&#8203;3440])

[#&#8203;3432]: https://redirect.github.com/tokio-rs/tracing/pull/3432

[#&#8203;3440]: https://redirect.github.com/tokio-rs/tracing/pull/3440

### [`v0.1.43`](https://redirect.github.com/tokio-rs/tracing/releases/tag/tracing-0.1.43): tracing 0.1.43

[Compare Source](https://redirect.github.com/tokio-rs/tracing/compare/tracing-0.1.42...tracing-0.1.43)

##### Important

The previous release [0.1.42] was yanked because [#&#8203;3382] was a breaking change.
See further details in [#&#8203;3424]. This release contains all the changes from that
version, plus a revert for the problematic part of the breaking PR.

##### Fixed

- Revert "make `valueset` macro sanitary" ([#&#8203;3425])

[#&#8203;3382]: https://redirect.github.com/tokio-rs/tracing/pull/3382

[#&#8203;3424]: https://redirect.github.com/tokio-rs/tracing/pull/3424

[#&#8203;3425]: https://redirect.github.com/tokio-rs/tracing/pull/3425

[0.1.42]: https://redirect.github.com/tokio-rs/tracing/releases/tag/tracing-0.1.42

### [`v0.1.42`](https://redirect.github.com/tokio-rs/tracing/releases/tag/tracing-0.1.42): tracing 0.1.42

[Compare Source](https://redirect.github.com/tokio-rs/tracing/compare/tracing-0.1.41...tracing-0.1.42)

##### Important

The [`Span::record_all`] method has been removed from the documented API. It
was always unsuable via the documented API as it requried a `ValueSet` which
has no publically documented constructors. The method remains, but should not
be used outside of `tracing` macros.

##### Added

- **attributes**: Support constant expressions as instrument field names ([#&#8203;3158])
- Add `record_all!` macro for recording multiple values in one call ([#&#8203;3227])
- **core**: Improve code generation at trace points significantly ([#&#8203;3398])

##### Changed

- `tracing-core`: updated to 0.1.35 ([#&#8203;3414])
- `tracing-attributes`: updated to 0.1.31 ([#&#8203;3417])

##### Fixed

- Fix "name / parent" variant of `event!` ([#&#8203;2983])
- Remove 'r#' prefix from raw identifiers in field names ([#&#8203;3130])
- Fix perf regression when `release_max_level_*` not set ([#&#8203;3373])
- Use imported instead of fully qualified path ([#&#8203;3374])
- Make `valueset` macro sanitary ([#&#8203;3382])

##### Documented

- **core**: Add missing `dyn` keyword in `Visit` documentation code sample ([#&#8203;3387])

[#&#8203;2983]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;2983

[#&#8203;3130]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3130

[#&#8203;3158]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3158

[#&#8203;3227]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3227

[#&#8203;3373]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3373

[#&#8203;3374]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3374

[#&#8203;3382]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3382

[#&#8203;3387]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3387

[#&#8203;3398]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3398

[#&#8203;3414]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3414

[#&#8203;3417]: https://redirect.github.com/tokio-rs/tracing/pull/#&#8203;3417

[`Span::record_all`]: https://docs.rs/tracing/0.1.41/tracing/struct.Span.html#method.record_all

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Between 12:00 AM and 03:59 AM, only on Monday ( * 0-3 * * 1 ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://redirect.github.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi43NC41IiwidXBkYXRlZEluVmVyIjoiNDIuODUuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiYnVpbGQ6c2tpcC1yZWxlYXNlIiwiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-19 01:32_

---

_Label `build:skip-release` added by @renovate[bot] on 2026-01-19 19:14_

---
