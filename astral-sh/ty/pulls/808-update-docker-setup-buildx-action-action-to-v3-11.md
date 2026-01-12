```yaml
number: 808
title: Update docker/setup-buildx-action action to v3.11.1
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: main
head: renovate/docker-setup-buildx-action-3.x
created_at: 2025-07-11T07:30:56Z
updated_at: 2025-07-11T07:44:35Z
url: https://github.com/astral-sh/ty/pull/808
synced_at: 2026-01-12T15:54:27Z
```

# Update docker/setup-buildx-action action to v3.11.1

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [docker/setup-buildx-action](https://redirect.github.com/docker/setup-buildx-action) | action | minor | `v3` -> `v3.11.1` |

---

### Release Notes

<details>
<summary>docker/setup-buildx-action (docker/setup-buildx-action)</summary>

### [`v3.11.1`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.11.1)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.11.0...v3.11.1)

- Fix `keep-state` not being respected by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/429](https://redirect.github.com/docker/setup-buildx-action/pull/429)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.11.0...v3.11.1

### [`v3.11.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.11.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.10.0...v3.11.0)

- Keep BuildKit state support by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/427](https://redirect.github.com/docker/setup-buildx-action/pull/427)
- Remove aliases created when installing by default by [@&#8203;hashhar](https://redirect.github.com/hashhar) in [https://github.com/docker/setup-buildx-action/pull/139](https://redirect.github.com/docker/setup-buildx-action/pull/139)
- Bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.56.0 to 0.62.1 in [https://github.com/docker/setup-buildx-action/pull/422](https://redirect.github.com/docker/setup-buildx-action/pull/422) [https://github.com/docker/setup-buildx-action/pull/425](https://redirect.github.com/docker/setup-buildx-action/pull/425)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.10.0...v3.11.0

### [`v3.10.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.10.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.9.0...v3.10.0)

- Bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.54.0 to 0.56.0 in [https://github.com/docker/setup-buildx-action/pull/408](https://redirect.github.com/docker/setup-buildx-action/pull/408)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.9.0...v3.10.0

### [`v3.9.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.9.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.8.0...v3.9.0)

- Bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.48.0 to 0.54.0 in [https://github.com/docker/setup-buildx-action/pull/402](https://redirect.github.com/docker/setup-buildx-action/pull/402) [https://github.com/docker/setup-buildx-action/pull/404](https://redirect.github.com/docker/setup-buildx-action/pull/404)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.8.0...v3.9.0

### [`v3.8.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.8.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.7.1...v3.8.0)

- Make cloud prefix optional to download buildx if driver is cloud by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/390](https://redirect.github.com/docker/setup-buildx-action/pull/390)
- Bump [@&#8203;actions/core](https://redirect.github.com/actions/core) from 1.10.1 to 1.11.1 in [https://github.com/docker/setup-buildx-action/pull/370](https://redirect.github.com/docker/setup-buildx-action/pull/370)
- Bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.39.0 to 0.48.0 in [https://github.com/docker/setup-buildx-action/pull/389](https://redirect.github.com/docker/setup-buildx-action/pull/389)
- Bump cross-spawn from 7.0.3 to 7.0.6 in [https://github.com/docker/setup-buildx-action/pull/382](https://redirect.github.com/docker/setup-buildx-action/pull/382)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.7.1...v3.8.0

### [`v3.7.1`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.7.1)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.7.0...v3.7.1)

- Switch back to `uuid` package by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/369](https://redirect.github.com/docker/setup-buildx-action/pull/369)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.7.0...v3.7.1

### [`v3.7.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.7.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.6.1...v3.7.0)

- Always set `buildkitd-flags` if opt-in by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/363](https://redirect.github.com/docker/setup-buildx-action/pull/363)
- Remove `uuid` package and switch to `crypto` by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/366](https://redirect.github.com/docker/setup-buildx-action/pull/366)
- Bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.35.0 to 0.39.0 in [https://github.com/docker/setup-buildx-action/pull/362](https://redirect.github.com/docker/setup-buildx-action/pull/362)
- Bump path-to-regexp from 6.2.2 to 6.3.0 in [https://github.com/docker/setup-buildx-action/pull/354](https://redirect.github.com/docker/setup-buildx-action/pull/354)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.6.1...v3.7.0

### [`v3.6.1`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.6.1)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.6.0...v3.6.1)

- Check for malformed docker context by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/347](https://redirect.github.com/docker/setup-buildx-action/pull/347)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.6.0...v3.6.1

### [`v3.6.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.6.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.5.0...v3.6.0)

- Create temp docker context if default one has TLS data loaded before creating a container builder by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/341](https://redirect.github.com/docker/setup-buildx-action/pull/341)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.5.0...v3.6.0

### [`v3.5.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.5.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.4.0...v3.5.0)

- Bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.31.0 to 0.35.0 in [https://github.com/docker/setup-buildx-action/pull/340](https://redirect.github.com/docker/setup-buildx-action/pull/340) [https://github.com/docker/setup-buildx-action/pull/344](https://redirect.github.com/docker/setup-buildx-action/pull/344) [https://github.com/docker/setup-buildx-action/pull/345](https://redirect.github.com/docker/setup-buildx-action/pull/345)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.4.0...v3.5.0

### [`v3.4.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.4.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.3.0...v3.4.0)

- Throw error message instead of exit code by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/315](https://redirect.github.com/docker/setup-buildx-action/pull/315)
- Bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.20.0 to 0.31.0 in [https://github.com/docker/setup-buildx-action/pull/321](https://redirect.github.com/docker/setup-buildx-action/pull/321) [https://github.com/docker/setup-buildx-action/pull/338](https://redirect.github.com/docker/setup-buildx-action/pull/338)
- Bump braces from 3.0.2 to 3.0.3 in [https://github.com/docker/setup-buildx-action/pull/329](https://redirect.github.com/docker/setup-buildx-action/pull/329)
- Bump undici from 5.28.3 to 5.28.4 in [https://github.com/docker/setup-buildx-action/pull/312](https://redirect.github.com/docker/setup-buildx-action/pull/312)
- Bump uuid from 9.0.1 to 10.0.0 in [https://github.com/docker/setup-buildx-action/pull/326](https://redirect.github.com/docker/setup-buildx-action/pull/326)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.3.0...v3.4.0

### [`v3.3.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.3.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.2.0...v3.3.0)

- Bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.19.0 to 0.20.0 by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/307](https://redirect.github.com/docker/setup-buildx-action/pull/307)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.2.0...v3.3.0

### [`v3.2.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.2.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.1.0...v3.2.0)

- Rename and align config inputs by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/303](https://redirect.github.com/docker/setup-buildx-action/pull/303)
  - `config` to `buildkitd-config`
  - `config-inline` to `buildkitd-config-inline`
- Bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.17.0 to 0.19.0 in [https://github.com/docker/setup-buildx-action/pull/302](https://redirect.github.com/docker/setup-buildx-action/pull/302) [https://github.com/docker/setup-buildx-action/pull/306](https://redirect.github.com/docker/setup-buildx-action/pull/306)

> \[!NOTE]
> `config` and `config-inline` input names are deprecated and will be removed in next major release.

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.1.0...v3.2.0

### [`v3.1.0`](https://redirect.github.com/docker/setup-buildx-action/releases/tag/v3.1.0)

[Compare Source](https://redirect.github.com/docker/setup-buildx-action/compare/v3.0.0...v3.1.0)

- `cache-binary` input to enable/disable caching binary to GHA cache backend by [@&#8203;crazy-max](https://redirect.github.com/crazy-max) in [https://github.com/docker/setup-buildx-action/pull/300](https://redirect.github.com/docker/setup-buildx-action/pull/300)
- build(deps): bump [@&#8203;babel/traverse](https://redirect.github.com/babel/traverse) from 7.17.3 to 7.23.2 in [https://github.com/docker/setup-buildx-action/pull/282](https://redirect.github.com/docker/setup-buildx-action/pull/282)
- build(deps): bump [@&#8203;docker/actions-toolkit](https://redirect.github.com/docker/actions-toolkit) from 0.12.0 to 0.17.0 in [https://github.com/docker/setup-buildx-action/pull/281](https://redirect.github.com/docker/setup-buildx-action/pull/281) [https://github.com/docker/setup-buildx-action/pull/284](https://redirect.github.com/docker/setup-buildx-action/pull/284) [https://github.com/docker/setup-buildx-action/pull/299](https://redirect.github.com/docker/setup-buildx-action/pull/299)
- build(deps): bump uuid from 9.0.0 to 9.0.1 in [https://github.com/docker/setup-buildx-action/pull/271](https://redirect.github.com/docker/setup-buildx-action/pull/271)
- build(deps): bump undici from 5.26.3 to 5.28.3 in [https://github.com/docker/setup-buildx-action/pull/297](https://redirect.github.com/docker/setup-buildx-action/pull/297)

**Full Changelog**: https://github.com/docker/setup-buildx-action/compare/v3.0.0...v3.1.0

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ty).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4yMy4yIiwidXBkYXRlZEluVmVyIjoiNDEuMjMuMiIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Merged by @MichaReiser on 2025-07-11 07:44_

---

_Closed by @MichaReiser on 2025-07-11 07:44_

---

_Branch deleted on 2025-07-11 07:44_

---
