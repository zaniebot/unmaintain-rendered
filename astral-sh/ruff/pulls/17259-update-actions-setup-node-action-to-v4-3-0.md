```yaml
number: 17259
title: Update actions/setup-node action to v4.3.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/actions-setup-node-4.x
created_at: 2025-04-07T02:05:13Z
updated_at: 2025-04-07T06:57:59Z
url: https://github.com/astral-sh/ruff/pull/17259
synced_at: 2026-01-12T15:56:01Z
```

# Update actions/setup-node action to v4.3.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [actions/setup-node](https://redirect.github.com/actions/setup-node) | action | minor | `v4` -> `v4.3.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>actions/setup-node (actions/setup-node)</summary>

### [`v4.3.0`](https://redirect.github.com/actions/setup-node/compare/v4.2.0...v4.3.0)

[Compare Source](https://redirect.github.com/actions/setup-node/compare/v4.2.0...v4.3.0)

### [`v4.2.0`](https://redirect.github.com/actions/setup-node/releases/tag/v4.2.0)

[Compare Source](https://redirect.github.com/actions/setup-node/compare/v4.1.0...v4.2.0)

#### What's Changed

-   Enhance workflows and upgrade publish-actions from 0.2.2 to 0.3.0 by [@&#8203;aparnajyothi-y](https://redirect.github.com/aparnajyothi-y) in [https://github.com/actions/setup-node/pull/1174](https://redirect.github.com/actions/setup-node/pull/1174)
-   Add recommended permissions section to readme by [@&#8203;benwells](https://redirect.github.com/benwells) in [https://github.com/actions/setup-node/pull/1193](https://redirect.github.com/actions/setup-node/pull/1193)
-   Configure Dependabot settings by [@&#8203;HarithaVattikuti](https://redirect.github.com/HarithaVattikuti) in [https://github.com/actions/setup-node/pull/1192](https://redirect.github.com/actions/setup-node/pull/1192)
-   Upgrade `@actions/cache` to `^4.0.0` by [@&#8203;priyagupta108](https://redirect.github.com/priyagupta108) in [https://github.com/actions/setup-node/pull/1191](https://redirect.github.com/actions/setup-node/pull/1191)
-   Upgrade pnpm/action-setup from 2 to 4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/setup-node/pull/1194](https://redirect.github.com/actions/setup-node/pull/1194)
-   Upgrade actions/publish-immutable-action from 0.0.3 to 0.0.4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/setup-node/pull/1195](https://redirect.github.com/actions/setup-node/pull/1195)
-   Upgrade semver from 7.6.0 to 7.6.3 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/setup-node/pull/1196](https://redirect.github.com/actions/setup-node/pull/1196)
-   Upgrade [@&#8203;types/jest](https://redirect.github.com/types/jest) from 29.5.12 to 29.5.14 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/setup-node/pull/1201](https://redirect.github.com/actions/setup-node/pull/1201)
-   Upgrade undici from 5.28.4 to 5.28.5 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/setup-node/pull/1205](https://redirect.github.com/actions/setup-node/pull/1205)

#### New Contributors

-   [@&#8203;benwells](https://redirect.github.com/benwells) made their first contribution in [https://github.com/actions/setup-node/pull/1193](https://redirect.github.com/actions/setup-node/pull/1193)

**Full Changelog**: https://github.com/actions/setup-node/compare/v4...v4.2.0

### [`v4.1.0`](https://redirect.github.com/actions/setup-node/releases/tag/v4.1.0)

[Compare Source](https://redirect.github.com/actions/setup-node/compare/v4.0.4...v4.1.0)

#### What's Changed

-   Resolve High Security Alerts by upgrading Dependencies by [@&#8203;aparnajyothi-y](https://redirect.github.com/aparnajyothi-y) in [https://github.com/actions/setup-node/pull/1132](https://redirect.github.com/actions/setup-node/pull/1132)
-   Upgrade IA Publish by [@&#8203;Jcambass](https://redirect.github.com/Jcambass) in [https://github.com/actions/setup-node/pull/1134](https://redirect.github.com/actions/setup-node/pull/1134)
-   Revise `isGhes` logic by [@&#8203;jww3](https://redirect.github.com/jww3) in [https://github.com/actions/setup-node/pull/1148](https://redirect.github.com/actions/setup-node/pull/1148)
-   Add architecture to cache key by [@&#8203;pengx17](https://redirect.github.com/pengx17) in [https://github.com/actions/setup-node/pull/843](https://redirect.github.com/actions/setup-node/pull/843)
    This addresses issues with caching by adding the architecture (arch) to the cache key, ensuring that cache keys are accurate to prevent conflicts.
    Note: This change may break previous cache keys as they will no longer be compatible with the new format.

#### New Contributors

-   [@&#8203;jww3](https://redirect.github.com/jww3) made their first contribution in [https://github.com/actions/setup-node/pull/1148](https://redirect.github.com/actions/setup-node/pull/1148)
-   [@&#8203;pengx17](https://redirect.github.com/pengx17) made their first contribution in [https://github.com/actions/setup-node/pull/843](https://redirect.github.com/actions/setup-node/pull/843)

**Full Changelog**: https://github.com/actions/setup-node/compare/v4...v4.1.0

### [`v4.0.4`](https://redirect.github.com/actions/setup-node/releases/tag/v4.0.4)

[Compare Source](https://redirect.github.com/actions/setup-node/compare/v4.0.3...v4.0.4)

#### What's Changed

-   Add workflow file for publishing releases to immutable action package by [@&#8203;Jcambass](https://redirect.github.com/Jcambass) in [https://github.com/actions/setup-node/pull/1125](https://redirect.github.com/actions/setup-node/pull/1125)
-   Enhance Windows ARM64 Setup and Update micromatch Dependency by [@&#8203;priyagupta108](https://redirect.github.com/priyagupta108) in [https://github.com/actions/setup-node/pull/1126](https://redirect.github.com/actions/setup-node/pull/1126)

##### Documentation changes:

-   Documentation update in the README file by [@&#8203;suyashgaonkar](https://redirect.github.com/suyashgaonkar) in [https://github.com/actions/setup-node/pull/1106](https://redirect.github.com/actions/setup-node/pull/1106)
-   Correct invalid 'lts' version string reference by [@&#8203;fulldecent](https://redirect.github.com/fulldecent) in [https://github.com/actions/setup-node/pull/1124](https://redirect.github.com/actions/setup-node/pull/1124)

#### New Contributors

-   [@&#8203;suyashgaonkar](https://redirect.github.com/suyashgaonkar) made their first contribution in [https://github.com/actions/setup-node/pull/1106](https://redirect.github.com/actions/setup-node/pull/1106)
-   [@&#8203;priyagupta108](https://redirect.github.com/priyagupta108) made their first contribution in [https://github.com/actions/setup-node/pull/1126](https://redirect.github.com/actions/setup-node/pull/1126)
-   [@&#8203;Jcambass](https://redirect.github.com/Jcambass) made their first contribution in [https://github.com/actions/setup-node/pull/1125](https://redirect.github.com/actions/setup-node/pull/1125)
-   [@&#8203;fulldecent](https://redirect.github.com/fulldecent) made their first contribution in [https://github.com/actions/setup-node/pull/1124](https://redirect.github.com/actions/setup-node/pull/1124)

**Full Changelog**: https://github.com/actions/setup-node/compare/v4...v4.0.4

### [`v4.0.3`](https://redirect.github.com/actions/setup-node/releases/tag/v4.0.3)

[Compare Source](https://redirect.github.com/actions/setup-node/compare/v4.0.2...v4.0.3)

##### What's Changed

##### Bug fixes:

-   Fix macos latest check failures by [@&#8203;HarithaVattikuti](https://redirect.github.com/HarithaVattikuti) in [https://github.com/actions/setup-node/pull/1041](https://redirect.github.com/actions/setup-node/pull/1041)

##### Documentation changes:

-   Documentation update to update default Node version to 20 by [@&#8203;bengreeley](https://redirect.github.com/bengreeley) in [https://github.com/actions/setup-node/pull/949](https://redirect.github.com/actions/setup-node/pull/949)

##### Dependency  updates:

-   Bump undici from 5.26.5 to 5.28.3 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/setup-node/pull/965](https://redirect.github.com/actions/setup-node/pull/965)
-   Bump braces from 3.0.2 to 3.0.3 and other dependency updates by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/setup-node/pull/1087](https://redirect.github.com/actions/setup-node/pull/1087)

##### New Contributors

-   [@&#8203;bengreeley](https://redirect.github.com/bengreeley) made their first contribution in [https://github.com/actions/setup-node/pull/949](https://redirect.github.com/actions/setup-node/pull/949)
-   [@&#8203;HarithaVattikuti](https://redirect.github.com/HarithaVattikuti) made their first contribution in [https://github.com/actions/setup-node/pull/1041](https://redirect.github.com/actions/setup-node/pull/1041)

**Full Changelog**: https://github.com/actions/setup-node/compare/v4...v4.0.3

### [`v4.0.2`](https://redirect.github.com/actions/setup-node/releases/tag/v4.0.2)

[Compare Source](https://redirect.github.com/actions/setup-node/compare/v4.0.1...v4.0.2)

##### What's Changed

-   Add support for `volta.extends` by [@&#8203;ThisIsManta](https://redirect.github.com/ThisIsManta) in [https://github.com/actions/setup-node/pull/921](https://redirect.github.com/actions/setup-node/pull/921)
-   Add support for arm64 Windows by [@&#8203;dmitry-shibanov](https://redirect.github.com/dmitry-shibanov) in [https://github.com/actions/setup-node/pull/927](https://redirect.github.com/actions/setup-node/pull/927)

##### New Contributors

-   [@&#8203;ThisIsManta](https://redirect.github.com/ThisIsManta) made their first contribution in [https://github.com/actions/setup-node/pull/921](https://redirect.github.com/actions/setup-node/pull/921)

**Full Changelog**: https://github.com/actions/setup-node/compare/v4.0.1...v4.0.2

### [`v4.0.1`](https://redirect.github.com/actions/setup-node/releases/tag/v4.0.1)

[Compare Source](https://redirect.github.com/actions/setup-node/compare/v4...v4.0.1)

##### What's Changed

-   Ignore engines in Yarn 1 e2e-cache tests by [@&#8203;trivikr](https://redirect.github.com/trivikr) in [https://github.com/actions/setup-node/pull/882](https://redirect.github.com/actions/setup-node/pull/882)
-   Update setup-node references in the README.md file to setup-node@v4 by [@&#8203;jwetzell](https://redirect.github.com/jwetzell) in [https://github.com/actions/setup-node/pull/884](https://redirect.github.com/actions/setup-node/pull/884)
-   Update reusable workflows to use Node.js v20 by [@&#8203;MaksimZhukov](https://redirect.github.com/MaksimZhukov) in [https://github.com/actions/setup-node/pull/889](https://redirect.github.com/actions/setup-node/pull/889)
-   Add fix for cache to resolve slow post action step by [@&#8203;aparnajyothi-y](https://redirect.github.com/aparnajyothi-y) in [https://github.com/actions/setup-node/pull/917](https://redirect.github.com/actions/setup-node/pull/917)
-   Fix README.md by [@&#8203;takayamaki](https://redirect.github.com/takayamaki) in [https://github.com/actions/setup-node/pull/898](https://redirect.github.com/actions/setup-node/pull/898)
-   Add `package.json` to `node-version-file` list of examples. by [@&#8203;TWiStErRob](https://redirect.github.com/TWiStErRob) in [https://github.com/actions/setup-node/pull/879](https://redirect.github.com/actions/setup-node/pull/879)
-   Fix node-version-file interprets entire package.json as a version by [@&#8203;NullVoxPopuli](https://redirect.github.com/NullVoxPopuli) in [https://github.com/actions/setup-node/pull/865](https://redirect.github.com/actions/setup-node/pull/865)

##### New Contributors

-   [@&#8203;trivikr](https://redirect.github.com/trivikr) made their first contribution in [https://github.com/actions/setup-node/pull/882](https://redirect.github.com/actions/setup-node/pull/882)
-   [@&#8203;jwetzell](https://redirect.github.com/jwetzell) made their first contribution in [https://github.com/actions/setup-node/pull/884](https://redirect.github.com/actions/setup-node/pull/884)
-   [@&#8203;aparnajyothi-y](https://redirect.github.com/aparnajyothi-y) made their first contribution in [https://github.com/actions/setup-node/pull/917](https://redirect.github.com/actions/setup-node/pull/917)
-   [@&#8203;takayamaki](https://redirect.github.com/takayamaki) made their first contribution in [https://github.com/actions/setup-node/pull/898](https://redirect.github.com/actions/setup-node/pull/898)
-   [@&#8203;TWiStErRob](https://redirect.github.com/TWiStErRob) made their first contribution in [https://github.com/actions/setup-node/pull/879](https://redirect.github.com/actions/setup-node/pull/879)
-   [@&#8203;NullVoxPopuli](https://redirect.github.com/NullVoxPopuli) made their first contribution in [https://github.com/actions/setup-node/pull/865](https://redirect.github.com/actions/setup-node/pull/865)

**Full Changelog**: https://github.com/actions/setup-node/compare/v4...v4.0.1

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yMjcuMyIsInVwZGF0ZWRJblZlciI6IjM5LjIyNy4zIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-04-07 02:05_

---

_Closed by @MichaReiser on 2025-04-07 06:34_

---

_Reopened by @MichaReiser on 2025-04-07 06:34_

---

_Merged by @MichaReiser on 2025-04-07 06:54_

---

_Closed by @MichaReiser on 2025-04-07 06:54_

---

_Branch deleted on 2025-04-07 06:54_

---

_Comment by @github-actions[bot] on 2025-04-07 06:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
