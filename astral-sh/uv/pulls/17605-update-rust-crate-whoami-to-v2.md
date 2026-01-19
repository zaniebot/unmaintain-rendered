```yaml
number: 17605
title: Update Rust crate whoami to v2
type: pull_request
state: open
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/whoami-2.x
created_at: 2026-01-19T01:36:17Z
updated_at: 2026-01-19T01:36:17Z
url: https://github.com/astral-sh/uv/pull/17605
synced_at: 2026-01-19T02:24:58Z
```

# Update Rust crate whoami to v2

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [whoami](https://redirect.github.com/ardaku/whoami/releases) ([source](https://redirect.github.com/ardaku/whoami)) | workspace.dependencies | major | `1.6.0` → `2.0.0` |

---

### Release Notes

<details>
<summary>ardaku/whoami (whoami)</summary>

### [`v2.0.2`](https://redirect.github.com/ardaku/whoami/releases/tag/v2.0.2)

[Compare Source](https://redirect.github.com/ardaku/whoami/compare/v2.0.1...v2.0.2)

### Changelog

#### Fixed

- Crate not compiling when `std` feature isn't set ([#&#8203;215](https://redirect.github.com/ardaku/whoami/issues/215))

***

#### What's Changed

- Test no-std in C/I by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;216](https://redirect.github.com/ardaku/whoami/pull/216)
- Prepare v2.0.2 by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;217](https://redirect.github.com/ardaku/whoami/pull/217)

**Full Changelog**: <https://github.com/ardaku/whoami/compare/v2.0.1...v2.0.2>

### [`v2.0.1`](https://redirect.github.com/ardaku/whoami/releases/tag/v2.0.1)

[Compare Source](https://redirect.github.com/ardaku/whoami/compare/v2.0.0...v2.0.1)

### Changelog

#### Fixed

- MSRV testing in CI actually works again
- MSRV mishap - WhoAmI 2.0.0 was requiring an MSRV of 1.81 accidentally, due to using `core::error::Error`, changed so that when the `std` feature is enabled the MSRV is as-advertised - 1.65.  Updated MSRV documentation to reflect that when the `std` feature is disabled it bumps the MSRV to 1.81
- Fixed Daku reporting 64-bit when 32-bit in `cpu_arch()` implementation
- Fixed environment variable typo `LC_MONTEARY` -> `LC_MONETARY`
- Fixed `whoami::Error` having infinite recursion in the `Display` implementation
- Fixed Broken links in the README.md
- Updated wasite minimum version to v1.0.2 to fix MSRV issues on WASI

***

#### What's Changed

- Fix architecture detection bug in Daku OS by [@&#8203;paolobarbolini](https://redirect.github.com/paolobarbolini) in [#&#8203;206](https://redirect.github.com/ardaku/whoami/pull/206)
- Fix typo in `LC_MONETARY` env var name by [@&#8203;paolobarbolini](https://redirect.github.com/paolobarbolini) in [#&#8203;205](https://redirect.github.com/ardaku/whoami/pull/205)
- Fix recursive Display implementation in Error type by [@&#8203;paolobarbolini](https://redirect.github.com/paolobarbolini) in [#&#8203;209](https://redirect.github.com/ardaku/whoami/pull/209)
- Update MSRV docs in README and fix broken links by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;210](https://redirect.github.com/ardaku/whoami/pull/210)
- Downgrade wasi in `Cargo.lock`, fix doc error by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;211](https://redirect.github.com/ardaku/whoami/pull/211)
- Restore MSRV support and CI checks by [@&#8203;paolobarbolini](https://redirect.github.com/paolobarbolini) in [#&#8203;204](https://redirect.github.com/ardaku/whoami/pull/204)
- Prepare v2.0.1 by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;212](https://redirect.github.com/ardaku/whoami/pull/212)

#### New Contributors

- [@&#8203;paolobarbolini](https://redirect.github.com/paolobarbolini) made their first contribution in [#&#8203;206](https://redirect.github.com/ardaku/whoami/pull/206)

**Full Changelog**: <https://github.com/ardaku/whoami/compare/v2.0.0...v2.0.1>

### [`v2.0.0`](https://redirect.github.com/ardaku/whoami/releases/tag/v2.0.0): (yanked)

### Changelog

#### Added

- Added `LanguagePreferences`
- Added `Error`, which implements `Into<std::io::Error>`
- no-std support
- Better docs around hostnames and platform-specific limitations
- `#[must_use]` to applicable functions
- Support checking for `Xfce` desktop environment

#### Changed

- MSRV updated to 1.65, upgraded to 2021 edition
- Rename `Platform::MacOS` to `Platform::Mac`
- Rename `Platform::Nintendo` to `Platform::Nintendo3ds`
- When checking pointer width, if can't be determined return `ErrorKind::Unsupported` instead of `ErrorKind::Other`
- `*::Unknown(_)` variants are now defined at the top of enums
- Updated libredox to v0.1.12
- `desktop_env()` now returns an `Option` - returning `None` when no desktop environment is applicable (such as over SSH)
- Feature set changed to:
  - `force-stub`: Only compile to stub backend
  - `std`: Depend on the standard library (default feature)
  - `wasi-wasite`: Assume Wasite environment variables are defined when compiling to wasi targets (default feature)
  - `wasm-web`: Assume wasm with unknown OS is being compiled for a web page (default feature)
- Rename `arch()` / `Arch` to `cpu_arch()` / `CpuArchitecture`
- Rename `DesktopEnv` to `DesktopEnvironment`
- `Language` is now a struct instead of an enum
- Language fallbacks without the country are appended at the end of language iteration
- `DesktopEnv::WebBrowser` variant now includes browser name and version
- `devicename()` on web target now always returns "Browser"
- `hostname()` on web target now returns the webpage URL instead of "localhost"
- `DesktopEnvironment::is_gtk()` and `DesktopEnvironment::is_kde()` are now `const` and marked with `#[must_use]`
- Update wasite target dependency to 1.0.0
- `distro()` now reports Windows 11 as Windows 11 instead of 10

#### Removed

- Removed `Platform::Xbox`
- Remove all infallible function variants, and remove the `fallible` module, moving those functions to the root module
- Remove deprecated `lang()`
- Remove `langs()` in favor of `lang_prefs()`
- `Language::country()`, `Country`
- Unnecessary `#[non_exhaustive]` attributes

***

#### What's Changed

- WhoAmI 2.0: 2021 edition by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;100](https://redirect.github.com/ardaku/whoami/pull/100)
- Make major 2.0 breaking changes by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;101](https://redirect.github.com/ardaku/whoami/pull/101)
- Deny doc warnings by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;105](https://redirect.github.com/ardaku/whoami/pull/105)
- Test docs in CI by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;107](https://redirect.github.com/ardaku/whoami/pull/107)
- V2: Update redox\_syscall to v0.5.1 by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;112](https://redirect.github.com/ardaku/whoami/pull/112)
- Keep V2 up to date with V1 patches by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;123](https://redirect.github.com/ardaku/whoami/pull/123)
- V2: Make `desktop_env()` return an `Option` by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;125](https://redirect.github.com/ardaku/whoami/pull/125)
- Use std::ffi everywhere instead of std::os::raw ([#&#8203;122](https://redirect.github.com/ardaku/whoami/issues/122)) by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;127](https://redirect.github.com/ardaku/whoami/pull/127)
- CI: Make cross compile clippy a separate action by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;128](https://redirect.github.com/ardaku/whoami/pull/128)
- Update copyright by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;129](https://redirect.github.com/ardaku/whoami/pull/129)
- Update readme with new MSRV policy by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;131](https://redirect.github.com/ardaku/whoami/pull/131)
- Fix clippy duplicate attribute for MacOS by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;134](https://redirect.github.com/ardaku/whoami/pull/134)
- Update wasm-bindgen to v0.2.100 by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;137](https://redirect.github.com/ardaku/whoami/pull/137)
- Update platform dependencies by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;139](https://redirect.github.com/ardaku/whoami/pull/139)
- Check more environment variables for language by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;144](https://redirect.github.com/ardaku/whoami/pull/144)
- Switch Windows `hostname()` to return `PhysicalDnsHostname` by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;147](https://redirect.github.com/ardaku/whoami/pull/147)
- Improved error handling by [@&#8203;robertbacula](https://redirect.github.com/robertbacula) in [#&#8203;136](https://redirect.github.com/ardaku/whoami/pull/136)
- Adjusts `langs()` to match POSIX locale spec by [@&#8203;cjohnson19](https://redirect.github.com/cjohnson19) in [#&#8203;150](https://redirect.github.com/ardaku/whoami/pull/150)
- Bump version to "2.0.0-pre.1" by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;152](https://redirect.github.com/ardaku/whoami/pull/152)
- Fix license link by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;153](https://redirect.github.com/ardaku/whoami/pull/153)
- upsteam v1 -> v2: Add discriminants to `ExtendedNameFormat` enum by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;157](https://redirect.github.com/ardaku/whoami/pull/157)
- Add ability to get language prefs for categories by [@&#8203;cjohnson19](https://redirect.github.com/cjohnson19) in [#&#8203;155](https://redirect.github.com/ardaku/whoami/pull/155)
- Bump version to 2.0.0-pre.2 by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;159](https://redirect.github.com/ardaku/whoami/pull/159)
- Try new templates by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;160](https://redirect.github.com/ardaku/whoami/pull/160)
- More template changes by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;161](https://redirect.github.com/ardaku/whoami/pull/161)
- Fix reading escaped unix pretty names by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;164](https://redirect.github.com/ardaku/whoami/pull/164)
- Remove some TODOs now that dedicated issues exist by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;177](https://redirect.github.com/ardaku/whoami/pull/177)
- Fix `LanguagePrefs` impl of `Display` by [@&#8203;cjohnson19](https://redirect.github.com/cjohnson19) in [#&#8203;179](https://redirect.github.com/ardaku/whoami/pull/179)
- Update crate description by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;181](https://redirect.github.com/ardaku/whoami/pull/181)
- CI: Turn off auto install for rustup by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;182](https://redirect.github.com/ardaku/whoami/pull/182)
- Migrate to workspace and define xtask by [@&#8203;cjohnson19](https://redirect.github.com/cjohnson19) in [#&#8203;180](https://redirect.github.com/ardaku/whoami/pull/180)
- CI: only cross-compile whoami library by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;184](https://redirect.github.com/ardaku/whoami/pull/184)
- Update Release PR template (v1.6.0) by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;183](https://redirect.github.com/ardaku/whoami/pull/183)
- v1 -> v2: Windows linkage fix by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;186](https://redirect.github.com/ardaku/whoami/pull/186)
- Replace redox\_syscall with libredox by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;189](https://redirect.github.com/ardaku/whoami/pull/189)
- Bump libredox from 0.1.9 to 0.1.10 by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;191](https://redirect.github.com/ardaku/whoami/pull/191)
- Change web behavior for whoami 2.0 by [@&#8203;DynoW](https://redirect.github.com/DynoW) in [#&#8203;192](https://redirect.github.com/ardaku/whoami/pull/192)
- docs: document platform-specific hostname character limits by [@&#8203;naoNao89](https://redirect.github.com/naoNao89) in [#&#8203;194](https://redirect.github.com/ardaku/whoami/pull/194)
- Bump libredox from 0.1.10 to 0.1.11 by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;196](https://redirect.github.com/ardaku/whoami/pull/196)
- Finalize Language API for whoami 2.0 by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;198](https://redirect.github.com/ardaku/whoami/pull/198)
- Bump libredox from 0.1.11 to 0.1.12 by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;199](https://redirect.github.com/ardaku/whoami/pull/199)
- Final changes preparing for whoami 2.0 by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;200](https://redirect.github.com/ardaku/whoami/pull/200)
- Add some `must_use`s, organize attributes, remove redundant `non_exhaustive`, fix small copypaste bug (Wasm64) by [@&#8203;BioTomateDE](https://redirect.github.com/BioTomateDE) in [#&#8203;201](https://redirect.github.com/ardaku/whoami/pull/201)
- Release v2.0.0 by [@&#8203;AldaronLau](https://redirect.github.com/AldaronLau) in [#&#8203;163](https://redirect.github.com/ardaku/whoami/pull/163)

#### New Contributors

- [@&#8203;robertbacula](https://redirect.github.com/robertbacula) made their first contribution in [#&#8203;136](https://redirect.github.com/ardaku/whoami/pull/136)
- [@&#8203;cjohnson19](https://redirect.github.com/cjohnson19) made their first contribution in [#&#8203;150](https://redirect.github.com/ardaku/whoami/pull/150)
- [@&#8203;DynoW](https://redirect.github.com/DynoW) made their first contribution in [#&#8203;192](https://redirect.github.com/ardaku/whoami/pull/192)
- [@&#8203;naoNao89](https://redirect.github.com/naoNao89) made their first contribution in [#&#8203;194](https://redirect.github.com/ardaku/whoami/pull/194)
- [@&#8203;BioTomateDE](https://redirect.github.com/BioTomateDE) made their first contribution in [#&#8203;201](https://redirect.github.com/ardaku/whoami/pull/201)

**Full Changelog**: <https://github.com/ardaku/whoami/compare/v1.6.0...v2.0.0>

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

_Label `internal` added by @renovate[bot] on 2026-01-19 01:36_

---
