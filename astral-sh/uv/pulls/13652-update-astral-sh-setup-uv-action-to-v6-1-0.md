```yaml
number: 13652
title: Update astral-sh/setup-uv action to v6.1.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-6.x
created_at: 2025-05-26T02:12:17Z
updated_at: 2025-05-26T12:37:39Z
url: https://github.com/astral-sh/uv/pull/13652
synced_at: 2026-01-10T11:10:42Z
```

# Update astral-sh/setup-uv action to v6.1.0

---

_Pull request opened by @renovate on 2025-05-26 02:12_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | minor | `v6.0.1` -> `v6.1.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v6.1.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.1.0): 🌈

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.0.1...v6.1.0)

#### Changes

This release adds the input `server-url` which defaults to `https://github.com`. You can set this to a custom url to control where this action downloads the uv release from. This is useful for users of gitea and comparable solutions.

[@&#8203;sebadevo](https://redirect.github.com/sebadevo) pointed out that we don't invalidate the cache when the `prune-cache` input is changed. This leads to unnessecarily big caches. The input is now used to compute the cache key, properly invalidating the cache when it is changed.

> \[!NOTE]\
> For most users this release will invalidate the cache once.
> You will see the known warning [no-github-actions-cache-found-for-key](https://redirect.github.com/astral-sh/setup-uv?tab=readme-ov-file#why-do-i-see-warnings-like-no-github-actions-cache-found-for-key)
> This is expected and will only appear once.

#### 🐛 Bug fixes

-   Purge cache in cache key [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;423](https://redirect.github.com/astral-sh/setup-uv/issues/423))

#### 🚀 Enhancements

-   feat: support custom github url [@&#8203;Zoupers](https://redirect.github.com/Zoupers) ([#&#8203;414](https://redirect.github.com/astral-sh/setup-uv/issues/414))

#### 🧰 Maintenance

-   chore: update known versions for 0.7.7 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;422](https://redirect.github.com/astral-sh/setup-uv/issues/422))
-   chore: update known versions for 0.7.6 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;415](https://redirect.github.com/astral-sh/setup-uv/issues/415))
-   chore: update known versions for 0.7.5 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;412](https://redirect.github.com/astral-sh/setup-uv/issues/412))
-   chore: update known versions for 0.7.4 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;410](https://redirect.github.com/astral-sh/setup-uv/issues/410))
-   chore: update known versions for 0.7.3 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;405](https://redirect.github.com/astral-sh/setup-uv/issues/405))
-   Fix path to known-checksums.ts [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;404](https://redirect.github.com/astral-sh/setup-uv/issues/404))
-   Fix update-known-versions workflow argument [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;401](https://redirect.github.com/astral-sh/setup-uv/issues/401))
-   Fix update-known-versions workflow [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;400](https://redirect.github.com/astral-sh/setup-uv/issues/400))
-   Create version-manifest.json on uv release [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;399](https://redirect.github.com/astral-sh/setup-uv/issues/399))
-   Run infrastructure workflows on arm runners [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;396](https://redirect.github.com/astral-sh/setup-uv/issues/396))
-   chore: update known checksums for 0.7.2 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;395](https://redirect.github.com/astral-sh/setup-uv/issues/395))
-   chore: update known checksums for 0.7.0 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;390](https://redirect.github.com/astral-sh/setup-uv/issues/390))

#### 📚 Documentation

-   Add section to README explaining if packages are installed by setup-uv [@&#8203;pirate](https://redirect.github.com/pirate) ([#&#8203;398](https://redirect.github.com/astral-sh/setup-uv/issues/398))

#### ⬆️ Dependency updates

-   Bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;424](https://redirect.github.com/astral-sh/setup-uv/issues/424))
-   Bump typescript from 5.8.2 to 5.8.3 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;393](https://redirect.github.com/astral-sh/setup-uv/issues/393))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC4xNi4wIiwidXBkYXRlZEluVmVyIjoiNDAuMTYuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-05-26 02:12_

---

_Merged by @konstin on 2025-05-26 12:37_

---

_Closed by @konstin on 2025-05-26 12:37_

---

_Branch deleted on 2025-05-26 12:37_

---
