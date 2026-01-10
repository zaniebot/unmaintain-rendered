```yaml
number: 2326
title: Update astral-sh/setup-uv action to v7.1.6 - autoclosed
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-7.x
created_at: 2026-01-05T01:02:48Z
updated_at: 2026-01-05T01:10:18Z
url: https://github.com/astral-sh/ty/pull/2326
synced_at: 2026-01-10T02:34:11Z
```

# Update astral-sh/setup-uv action to v7.1.6 - autoclosed

---

_Pull request opened by @renovate on 2026-01-05 01:02_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | patch | `v7.1.4` → `v7.1.6` |

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v7.1.6`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.6): 🌈 add OS version to cache key to prevent binary incompatibility

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.5...v7.1.6)

##### Changes

This release will invalidate your cache existing keys!

The os version e.g. `ubuntu-22.04` is now part of the cache key. This prevents failing builds when a cache got populated with wheels built with different tools (e.g. glibc) than are present on the runner where the cache got restored.

##### 🐛 Bug fixes

- feat: add OS version to cache key to prevent binary incompatibility [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;716](https://redirect.github.com/astral-sh/setup-uv/issues/716))

##### 🧰 Maintenance

- chore: update known checksums for 0.9.17 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;714](https://redirect.github.com/astral-sh/setup-uv/issues/714))

##### ⬆️ Dependency updates

- Bump actions/checkout from 5.0.0 to 6.0.1 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;712](https://redirect.github.com/astral-sh/setup-uv/issues/712))
- Bump actions/setup-node from 6.0.0 to 6.1.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;715](https://redirect.github.com/astral-sh/setup-uv/issues/715))

### [`v7.1.5`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.5): 🌈 allow setting `cache-local-path` without `enable-cache: true`

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.4...v7.1.5)

##### Changes

[#&#8203;612](https://redirect.github.com/astral-sh/setup-uv/pull/612) fixed a faulty behavior where this action set `UV_CACHE_DIR` even though `enable-cache` was `false`. It also fixed the cases were the cache dir is already configured in a settings file like `pyproject.toml` or `UV_CACHE_DIR` was already set.  Here the action shouldn't overwrite or set `UV_CACHE_DIR`.

These fixes introduced an unwanted behavior: You can still set `cache-local-path` but this action didn't do anything. This release fixes that.

You can now use `cache-local-path` to automatically set `UV_CACHE_DIR` even when `enable-cache` is `false` (or gets set to false by default e.g. on self-hosted runners)

```yaml
- name: This is now possible
  uses: astral-sh/setup-uv@v7
  with:
    enable-cache: false
    cache-local-path: "/path/to/cache"
```

##### 🐛 Bug fixes

- allow cache-local-path w/o enable-cache [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;707](https://redirect.github.com/astral-sh/setup-uv/issues/707))

##### 🧰 Maintenance

- set biome files.maxSize to 2MiB [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;708](https://redirect.github.com/astral-sh/setup-uv/issues/708))
- chore: update known checksums for 0.9.16 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;706](https://redirect.github.com/astral-sh/setup-uv/issues/706))
- chore: update known checksums for 0.9.15 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;704](https://redirect.github.com/astral-sh/setup-uv/issues/704))
- chore: use `npm ci --ignore-scripts` everywhere [@&#8203;woodruffw](https://redirect.github.com/woodruffw) ([#&#8203;699](https://redirect.github.com/astral-sh/setup-uv/issues/699))
- chore: update known checksums for 0.9.14 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;700](https://redirect.github.com/astral-sh/setup-uv/issues/700))
- chore: update known checksums for 0.9.13 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;694](https://redirect.github.com/astral-sh/setup-uv/issues/694))
- chore: update known checksums for 0.9.12 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;693](https://redirect.github.com/astral-sh/setup-uv/issues/693))
- chore: update known checksums for 0.9.11 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;688](https://redirect.github.com/astral-sh/setup-uv/issues/688))

##### ⬆️ Dependency updates

- Bump peter-evans/create-pull-request from 7.0.8 to 7.0.9 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;695](https://redirect.github.com/astral-sh/setup-uv/issues/695))
- bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;709](https://redirect.github.com/astral-sh/setup-uv/issues/709))
- Bump github/codeql-action from 4.30.9 to 4.31.6 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;698](https://redirect.github.com/astral-sh/setup-uv/issues/698))
- Bump zizmorcore/zizmor-action from 0.2.0 to 0.3.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;696](https://redirect.github.com/astral-sh/setup-uv/issues/696))
- Bump eifinger/actionlint-action from 1.9.2 to 1.9.3 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;690](https://redirect.github.com/astral-sh/setup-uv/issues/690))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi42OS4xIiwidXBkYXRlZEluVmVyIjoiNDIuNjkuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-05 01:02_

---

_Merged by @AlexWaygood on 2026-01-05 01:09_

---

_Closed by @AlexWaygood on 2026-01-05 01:09_

---

_Branch deleted on 2026-01-05 01:09_

---

_Renamed from "Update astral-sh/setup-uv action to v7.1.6" to "Update astral-sh/setup-uv action to v7.1.6 - autoclosed" by @renovate[bot] on 2026-01-05 01:10_

---
