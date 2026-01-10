```yaml
number: 10505
title: Update Rust crate libz-ng-sys to <1.1.22
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/libz-ng-sys-1.x
created_at: 2025-01-11T16:09:32Z
updated_at: 2025-01-11T17:30:16Z
url: https://github.com/astral-sh/uv/pull/10505
synced_at: 2026-01-10T11:44:53Z
```

# Update Rust crate libz-ng-sys to <1.1.22

---

_Pull request opened by @renovate on 2025-01-11 16:09_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libz-ng-sys](https://redirect.github.com/rust-lang/libz-sys) | dependencies | patch | `<1.1.20` -> `<1.1.22` |

---

### Release Notes

<details>
<summary>rust-lang/libz-sys (libz-ng-sys)</summary>

### [`v1.1.21`](https://redirect.github.com/rust-lang/libz-sys/releases/tag/1.1.21)

[Compare Source](https://redirect.github.com/rust-lang/libz-sys/compare/1.1.20...1.1.21)

#### What's Changed

-   Fix cargo:warning syntax in build.rs by [@&#8203;kornelski](https://redirect.github.com/kornelski) in [https://github.com/rust-lang/libz-sys/pull/209](https://redirect.github.com/rust-lang/libz-sys/pull/209)
-   Bump actions/upload-artifact from 4.3.6 to 4.4.0 in the github-actions group by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/rust-lang/libz-sys/pull/210](https://redirect.github.com/rust-lang/libz-sys/pull/210)
-   Update zlib-ng library to 2.2.1 by [@&#8203;JakubOnderka](https://redirect.github.com/JakubOnderka) in [https://github.com/rust-lang/libz-sys/pull/211](https://redirect.github.com/rust-lang/libz-sys/pull/211)
-   Treat `\` in `cargo package -l` output as `/` by [@&#8203;EliahKagan](https://redirect.github.com/EliahKagan) in [https://github.com/rust-lang/libz-sys/pull/213](https://redirect.github.com/rust-lang/libz-sys/pull/213)
-   Fix race condition scheduling `$tempdir` deletion by [@&#8203;EliahKagan](https://redirect.github.com/EliahKagan) in [https://github.com/rust-lang/libz-sys/pull/216](https://redirect.github.com/rust-lang/libz-sys/pull/216)
-   Also `grep -v` out `.cargo_vcs_info.json` in `cargo-zng` by [@&#8203;EliahKagan](https://redirect.github.com/EliahKagan) in [https://github.com/rust-lang/libz-sys/pull/217](https://redirect.github.com/rust-lang/libz-sys/pull/217)
-   Update zlib-ng library to 2.2.2 by [@&#8203;EliahKagan](https://redirect.github.com/EliahKagan) in [https://github.com/rust-lang/libz-sys/pull/219](https://redirect.github.com/rust-lang/libz-sys/pull/219)
-   Recognize `RISCV_WITH_RVV` env var on RISC-V to set `WITH_RVV` cmake var by [@&#8203;EliahKagan](https://redirect.github.com/EliahKagan) in [https://github.com/rust-lang/libz-sys/pull/218](https://redirect.github.com/rust-lang/libz-sys/pull/218)
-   Bump actions/checkout from 4.1.7 to 4.2.0 in the github-actions group by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/rust-lang/libz-sys/pull/220](https://redirect.github.com/rust-lang/libz-sys/pull/220)
-   ohos: Link shared lib on OpenHarmony by [@&#8203;jschwe](https://redirect.github.com/jschwe) in [https://github.com/rust-lang/libz-sys/pull/221](https://redirect.github.com/rust-lang/libz-sys/pull/221)
-   Bump the github-actions group with 2 updates by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/rust-lang/libz-sys/pull/222](https://redirect.github.com/rust-lang/libz-sys/pull/222)
-   bump the ZNG manifest version by [@&#8203;Byron](https://redirect.github.com/Byron) in [https://github.com/rust-lang/libz-sys/pull/224](https://redirect.github.com/rust-lang/libz-sys/pull/224)
-   Bump actions/checkout from 4.2.1 to 4.2.2 in the github-actions group by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/rust-lang/libz-sys/pull/226](https://redirect.github.com/rust-lang/libz-sys/pull/226)
-   Disable neon for apple targets by [@&#8203;Jake-Shadle](https://redirect.github.com/Jake-Shadle) in [https://github.com/rust-lang/libz-sys/pull/231](https://redirect.github.com/rust-lang/libz-sys/pull/231)
-   bump `zlib-ng` to version `2.2.3` by [@&#8203;folkertdev](https://redirect.github.com/folkertdev) in [https://github.com/rust-lang/libz-sys/pull/232](https://redirect.github.com/rust-lang/libz-sys/pull/232)
-   bump `zng` to 1.1.21 by [@&#8203;folkertdev](https://redirect.github.com/folkertdev) in [https://github.com/rust-lang/libz-sys/pull/233](https://redirect.github.com/rust-lang/libz-sys/pull/233)

#### New Contributors

-   [@&#8203;jschwe](https://redirect.github.com/jschwe) made their first contribution in [https://github.com/rust-lang/libz-sys/pull/221](https://redirect.github.com/rust-lang/libz-sys/pull/221)

**Full Changelog**: https://github.com/rust-lang/libz-sys/compare/1.1.20...1.1.21

### [`v1.1.20`](https://redirect.github.com/rust-lang/libz-sys/releases/tag/1.1.20)

[Compare Source](https://redirect.github.com/rust-lang/libz-sys/compare/1.1.16...1.1.20)

#### What's Changed

-   Fix unknown-cfg warning in libz-ng by [@&#8203;NobodyXu](https://redirect.github.com/NobodyXu) in [https://github.com/rust-lang/libz-sys/pull/208](https://redirect.github.com/rust-lang/libz-sys/pull/208)
-   Add minimal-versions CI by [@&#8203;NobodyXu](https://redirect.github.com/NobodyXu) in [https://github.com/rust-lang/libz-sys/pull/207](https://redirect.github.com/rust-lang/libz-sys/pull/207)

#### New Contributors

-   [@&#8203;NobodyXu](https://redirect.github.com/NobodyXu) made their first contribution in [https://github.com/rust-lang/libz-sys/pull/208](https://redirect.github.com/rust-lang/libz-sys/pull/208)

**Full Changelog**: https://github.com/rust-lang/libz-sys/compare/1.1.19...1.1.20

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS45Mi4wIiwidXBkYXRlZEluVmVyIjoiMzkuOTIuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-01-11 16:09_

---

_Closed by @charliermarsh on 2025-01-11 17:27_

---

_Comment by @renovate[bot] on 2025-01-11 17:30_

### Renovate Ignore Notification

Because you closed this PR without merging, Renovate will ignore this update (`<1.1.22`). You will get a PR once a newer version is released. To ignore this dependency forever, add it to the `ignoreDeps` array of your Renovate config.

If you accidentally closed this PR, or if you changed your mind: rename this PR to get a fresh replacement PR.

---

_Branch deleted on 2025-01-11 17:30_

---
