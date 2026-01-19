```yaml
number: 17602
title: Update Rust crate wmi to 0.18.0
type: pull_request
state: open
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/wmi-0.x
created_at: 2026-01-19T01:35:44Z
updated_at: 2026-01-19T05:31:16Z
url: https://github.com/astral-sh/uv/pull/17602
synced_at: 2026-01-19T06:21:36Z
```

# Update Rust crate wmi to 0.18.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [wmi](https://redirect.github.com/ohadravid/wmi-rs) | workspace.dependencies | minor | `0.16.0` → `0.18.0` |

---

### Release Notes

<details>
<summary>ohadravid/wmi-rs (wmi)</summary>

### [`v0.18.0`](https://redirect.github.com/ohadravid/wmi-rs/releases/tag/v0.18.0)

[Compare Source](https://redirect.github.com/ohadravid/wmi-rs/compare/v0.17.3...v0.18.0)

#### What's Changed

- Remove `COMLibrary` and let `WMIConnection` initialize COM if needed by [@&#8203;ohadravid](https://redirect.github.com/ohadravid) in [#&#8203;137](https://redirect.github.com/ohadravid/wmi-rs/pull/137)
  You can now call `WMIConnection::new()` and let the crate handle the initialization internally.
  Note: COM will NOT be uninitialized when the connection is dropped (similar to <=0.17 versions, which didn't uninitialize COM on drop since [#&#8203;53](https://redirect.github.com/ohadravid/wmi-rs/issues/53)).
  If this is not what you want, then you must initialize COM yourself **before** creating the connection. See the docs for more.
- Update the crate to Rust 2024 edition

**Full Changelog**: <https://github.com/ohadravid/wmi-rs/compare/v0.17.3...v0.18.0>

### [`v0.17.3`](https://redirect.github.com/ohadravid/wmi-rs/releases/tag/v0.17.3)

[Compare Source](https://redirect.github.com/ohadravid/wmi-rs/compare/v0.17.2...v0.17.3)

#### What's Changed

- chore(deps): update criterion requirement from 0.5 to 0.6 by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;128](https://redirect.github.com/ohadravid/wmi-rs/pull/128)
- Update CI images by [@&#8203;ohadravid](https://redirect.github.com/ohadravid) in [#&#8203;135](https://redirect.github.com/ohadravid/wmi-rs/pull/135)
- chore(deps): update windows requirement from 0.61 to 0.62 by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;134](https://redirect.github.com/ohadravid/wmi-rs/pull/134)

**Full Changelog**: <https://github.com/ohadravid/wmi-rs/compare/v0.17.2...v0.17.3>

### [`v0.17.2`](https://redirect.github.com/ohadravid/wmi-rs/releases/tag/v0.17.2)

[Compare Source](https://redirect.github.com/ohadravid/wmi-rs/compare/v0.17.1...v0.17.2)

#### What's Changed

- feat(remote\_connection): added with\_credentials() by [@&#8203;hatch15](https://redirect.github.com/hatch15) in [#&#8203;127](https://redirect.github.com/ohadravid/wmi-rs/pull/127)

#### New Contributors

- [@&#8203;hatch15](https://redirect.github.com/hatch15) made their first contribution in [#&#8203;127](https://redirect.github.com/ohadravid/wmi-rs/pull/127)

**Full Changelog**: <https://github.com/ohadravid/wmi-rs/compare/v0.17.1...v0.17.2>

### [`v0.17.1`](https://redirect.github.com/ohadravid/wmi-rs/releases/tag/v0.17.1)

[Compare Source](https://redirect.github.com/ohadravid/wmi-rs/compare/v0.17.0...v0.17.1)

#### What's Changed

- Support arrays of IUnknown pointers by [@&#8203;samin-cf](https://redirect.github.com/samin-cf) in [#&#8203;125](https://redirect.github.com/ohadravid/wmi-rs/pull/125) and [@&#8203;ohadravid](https://redirect.github.com/ohadravid) in [#&#8203;126](https://redirect.github.com/ohadravid/wmi-rs/pull/126)

**Full Changelog**: <https://github.com/ohadravid/wmi-rs/compare/v0.17.0...v0.17.1>

### [`v0.17.0`](https://redirect.github.com/ohadravid/wmi-rs/releases/tag/v0.17.0)

[Compare Source](https://redirect.github.com/ohadravid/wmi-rs/compare/v0.16.0...v0.17.0)

#### What's Changed

- Added support for Option by [@&#8203;vpopescu](https://redirect.github.com/vpopescu) in [#&#8203;122](https://redirect.github.com/ohadravid/wmi-rs/pull/122)

#### Breaking Changes

- Fixed conversions from Rust types to WMI types (so, only when used for method calling or using put\_property), which were incorrect in a few cases (notably, u32s and u16s were not converted correctly), and added some missing conversions, in [#&#8203;124](https://redirect.github.com/ohadravid/wmi-rs/pull/124)
- `SafeArrayAccessor::new` now accepts a `NonNull<SAFEARRAY>` instead of a reference, in [#&#8203;124](https://redirect.github.com/ohadravid/wmi-rs/pull/124)

#### New Contributors

- [@&#8203;vpopescu](https://redirect.github.com/vpopescu) made their first contribution in [#&#8203;122](https://redirect.github.com/ohadravid/wmi-rs/pull/122)

**Full Changelog**: <https://github.com/ohadravid/wmi-rs/compare/v0.16.0...v0.17.0>

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

_Label `internal` added by @renovate[bot] on 2026-01-19 01:35_

---

_Comment by @zanieb on 2026-01-19 05:31_

See https://github.com/astral-sh/uv/pull/17602/commits/b52e4fc02358f9e02700d8cf24230a7862a9faaa?diff=unified&w=1

---
