```yaml
number: 17256
title: Update actions/cache action to v4.2.3
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/actions-cache-4.x
created_at: 2025-04-07T02:04:35Z
updated_at: 2025-04-07T06:50:33Z
url: https://github.com/astral-sh/ruff/pull/17256
synced_at: 2026-01-10T19:40:37Z
```

# Update actions/cache action to v4.2.3

---

_Pull request opened by @renovate on 2025-04-07 02:04_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [actions/cache](https://redirect.github.com/actions/cache) | action | minor | `v4` -> `v4.2.3` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>actions/cache (actions/cache)</summary>

### [`v4.2.3`](https://redirect.github.com/actions/cache/releases/tag/v4.2.3)

[Compare Source](https://redirect.github.com/actions/cache/compare/v4.2.2...v4.2.3)

##### What's Changed

-   Update to use [@&#8203;actions/cache](https://redirect.github.com/actions/cache) 4.0.3 package & prepare for new release by [@&#8203;salmanmkc](https://redirect.github.com/salmanmkc) in [https://github.com/actions/cache/pull/1577](https://redirect.github.com/actions/cache/pull/1577) (SAS tokens for cache entries are now masked in debug logs)

##### New Contributors

-   [@&#8203;salmanmkc](https://redirect.github.com/salmanmkc) made their first contribution in [https://github.com/actions/cache/pull/1577](https://redirect.github.com/actions/cache/pull/1577)

**Full Changelog**: https://github.com/actions/cache/compare/v4.2.2...v4.2.3

### [`v4.2.2`](https://redirect.github.com/actions/cache/releases/tag/v4.2.2)

[Compare Source](https://redirect.github.com/actions/cache/compare/v4.2.1...v4.2.2)

##### What's Changed

> \[!IMPORTANT]
> As a reminder, there were important backend changes to release v4.2.0, see [those release notes](https://redirect.github.com/actions/cache/releases/tag/v4.2.0) and [the announcement](https://redirect.github.com/actions/cache/discussions/1510) for more details.

-   Bump [@&#8203;actions/cache](https://redirect.github.com/actions/cache) to v4.0.2 by [@&#8203;robherley](https://redirect.github.com/robherley) in [https://github.com/actions/cache/pull/1560](https://redirect.github.com/actions/cache/pull/1560)

**Full Changelog**: https://github.com/actions/cache/compare/v4.2.1...v4.2.2

### [`v4.2.1`](https://redirect.github.com/actions/cache/releases/tag/v4.2.1)

[Compare Source](https://redirect.github.com/actions/cache/compare/v4.2.0...v4.2.1)

##### What's Changed

> \[!IMPORTANT]
> As a reminder, there were important backend changes to release v4.2.0, see [those release notes](https://redirect.github.com/actions/cache/releases/tag/v4.2.0) and [the announcement](https://redirect.github.com/actions/cache/discussions/1510) for more details.

-   docs: GitHub is spelled incorrectly in caching-strategies.md by [@&#8203;janco-absa](https://redirect.github.com/janco-absa) in [https://github.com/actions/cache/pull/1526](https://redirect.github.com/actions/cache/pull/1526)
-   docs: Make the "always save prime numbers" example more clear by [@&#8203;Tobbe](https://redirect.github.com/Tobbe) in [https://github.com/actions/cache/pull/1525](https://redirect.github.com/actions/cache/pull/1525)
-   Update force deletion docs due a recent deprecation by [@&#8203;sebbalex](https://redirect.github.com/sebbalex) in [https://github.com/actions/cache/pull/1500](https://redirect.github.com/actions/cache/pull/1500)
-   Bump [@&#8203;actions/cache](https://redirect.github.com/actions/cache) to v4.0.1 by [@&#8203;robherley](https://redirect.github.com/robherley) in [https://github.com/actions/cache/pull/1554](https://redirect.github.com/actions/cache/pull/1554)

##### New Contributors

-   [@&#8203;janco-absa](https://redirect.github.com/janco-absa) made their first contribution in [https://github.com/actions/cache/pull/1526](https://redirect.github.com/actions/cache/pull/1526)
-   [@&#8203;Tobbe](https://redirect.github.com/Tobbe) made their first contribution in [https://github.com/actions/cache/pull/1525](https://redirect.github.com/actions/cache/pull/1525)
-   [@&#8203;sebbalex](https://redirect.github.com/sebbalex) made their first contribution in [https://github.com/actions/cache/pull/1500](https://redirect.github.com/actions/cache/pull/1500)

**Full Changelog**: https://github.com/actions/cache/compare/v4.2.0...v4.2.1

### [`v4.2.0`](https://redirect.github.com/actions/cache/releases/tag/v4.2.0)

[Compare Source](https://redirect.github.com/actions/cache/compare/v4.1.2...v4.2.0)

##### ⚠️ Important Changes

The cache backend service has been rewritten from the ground up for improved performance and reliability. [actions/cache](https://redirect.github.com/actions/cache) now integrates with the new cache service (v2) APIs.

The new service will gradually roll out as of **February 1st, 2025**. The legacy service will also be sunset on the same date. Changes in these release are **fully backward compatible**.

**We are deprecating some versions of this action**. We recommend upgrading to version `v4` or `v3` as soon as possible before **February 1st, 2025.** (Upgrade instructions below).

If you are using pinned SHAs, please use the SHAs of versions `v4.2.0` or `v3.4.0`

If you do not upgrade, all workflow runs using any of the deprecated [actions/cache](https://redirect.github.com/actions/cache) will fail.

Upgrading to the recommended versions will not break your workflows.

Read more about the change & access the migration guide: [reference to the announcement](https://redirect.github.com/actions/cache/discussions/1510).

##### Minor changes

Minor and patch version updates for these dependencies:

-   [@&#8203;actions/core](https://redirect.github.com/actions/core): `1.11.1`
-   [@&#8203;actions/io](https://redirect.github.com/actions/io): `1.1.3`
-   [@&#8203;vercel/ncc](https://redirect.github.com/vercel/ncc): `0.38.3`

**Full Changelog**: https://github.com/actions/cache/compare/v4.1.2...v4.2.0

### [`v4.1.2`](https://redirect.github.com/actions/cache/releases/tag/v4.1.2)

[Compare Source](https://redirect.github.com/actions/cache/compare/v4.1.1...v4.1.2)

##### What's Changed

-   Add Bun example by [@&#8203;idleberg](https://redirect.github.com/idleberg) in [https://github.com/actions/cache/pull/1456](https://redirect.github.com/actions/cache/pull/1456)
-   Revise `isGhes` logic by [@&#8203;jww3](https://redirect.github.com/jww3) in [https://github.com/actions/cache/pull/1474](https://redirect.github.com/actions/cache/pull/1474)
-   Bump braces from 3.0.2 to 3.0.3 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/cache/pull/1475](https://redirect.github.com/actions/cache/pull/1475)
-   Add dependabot.yml to enable automatic dependency upgrades by [@&#8203;Link-](https://redirect.github.com/Link-) in [https://github.com/actions/cache/pull/1476](https://redirect.github.com/actions/cache/pull/1476)
-   Bump actions/checkout from 3 to 4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/cache/pull/1478](https://redirect.github.com/actions/cache/pull/1478)
-   Bump actions/stale from 3 to 9 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/cache/pull/1479](https://redirect.github.com/actions/cache/pull/1479)
-   Bump github/codeql-action from 2 to 3 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/cache/pull/1483](https://redirect.github.com/actions/cache/pull/1483)
-   Bump actions/setup-node from 3 to 4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/cache/pull/1481](https://redirect.github.com/actions/cache/pull/1481)
-   Prepare `4.1.2` release by [@&#8203;Link-](https://redirect.github.com/Link-) in [https://github.com/actions/cache/pull/1477](https://redirect.github.com/actions/cache/pull/1477)

##### New Contributors

-   [@&#8203;idleberg](https://redirect.github.com/idleberg) made their first contribution in [https://github.com/actions/cache/pull/1456](https://redirect.github.com/actions/cache/pull/1456)
-   [@&#8203;jww3](https://redirect.github.com/jww3) made their first contribution in [https://github.com/actions/cache/pull/1474](https://redirect.github.com/actions/cache/pull/1474)
-   [@&#8203;Link-](https://redirect.github.com/Link-) made their first contribution in [https://github.com/actions/cache/pull/1476](https://redirect.github.com/actions/cache/pull/1476)

**Full Changelog**: https://github.com/actions/cache/compare/v4.1.1...v4.1.2

### [`v4.1.1`](https://redirect.github.com/actions/cache/releases/tag/v4.1.1)

[Compare Source](https://redirect.github.com/actions/cache/compare/v4.1.0...v4.1.1)

##### What's Changed

-   Restore original behavior of `cache-hit` output by [@&#8203;joshmgross](https://redirect.github.com/joshmgross) in [https://github.com/actions/cache/pull/1467](https://redirect.github.com/actions/cache/pull/1467)

**Full Changelog**: https://github.com/actions/cache/compare/v4.1.0...v4.1.1

### [`v4.1.0`](https://redirect.github.com/actions/cache/releases/tag/v4.1.0)

[Compare Source](https://redirect.github.com/actions/cache/compare/v4.0.2...v4.1.0)

##### What's Changed

-   Fix cache-hit output when cache missed by [@&#8203;fchimpan](https://redirect.github.com/fchimpan) in [https://github.com/actions/cache/pull/1404](https://redirect.github.com/actions/cache/pull/1404)
-   Deprecate `save-always` input by [@&#8203;joshmgross](https://redirect.github.com/joshmgross) in [https://github.com/actions/cache/pull/1452](https://redirect.github.com/actions/cache/pull/1452)

##### New Contributors

-   [@&#8203;ottlinger](https://redirect.github.com/ottlinger) made their first contribution in [https://github.com/actions/cache/pull/1437](https://redirect.github.com/actions/cache/pull/1437)
-   [@&#8203;Olegt0rr](https://redirect.github.com/Olegt0rr) made their first contribution in [https://github.com/actions/cache/pull/1377](https://redirect.github.com/actions/cache/pull/1377)
-   [@&#8203;fchimpan](https://redirect.github.com/fchimpan) made their first contribution in [https://github.com/actions/cache/pull/1404](https://redirect.github.com/actions/cache/pull/1404)
-   [@&#8203;x612skm](https://redirect.github.com/x612skm) made their first contribution in [https://github.com/actions/cache/pull/1434](https://redirect.github.com/actions/cache/pull/1434)
-   [@&#8203;todgru](https://redirect.github.com/todgru) made their first contribution in [https://github.com/actions/cache/pull/1311](https://redirect.github.com/actions/cache/pull/1311)
-   [@&#8203;Jcambass](https://redirect.github.com/Jcambass) made their first contribution in [https://github.com/actions/cache/pull/1463](https://redirect.github.com/actions/cache/pull/1463)
-   [@&#8203;mackey0225](https://redirect.github.com/mackey0225) made their first contribution in [https://github.com/actions/cache/pull/1462](https://redirect.github.com/actions/cache/pull/1462)
-   [@&#8203;quatquatt](https://redirect.github.com/quatquatt) made their first contribution in [https://github.com/actions/cache/pull/1445](https://redirect.github.com/actions/cache/pull/1445)

**Full Changelog**: https://github.com/actions/cache/compare/v4.0.2...v4.1.0

### [`v4.0.2`](https://redirect.github.com/actions/cache/releases/tag/v4.0.2)

[Compare Source](https://redirect.github.com/actions/cache/compare/v4.0.1...v4.0.2)

#### What's Changed

-   Fix `fail-on-cache-miss` not working by [@&#8203;cdce8p](https://redirect.github.com/cdce8p) in [https://github.com/actions/cache/pull/1327](https://redirect.github.com/actions/cache/pull/1327)

**Full Changelog**: https://github.com/actions/cache/compare/v4.0.1...v4.0.2

### [`v4.0.1`](https://redirect.github.com/actions/cache/releases/tag/v4.0.1)

[Compare Source](https://redirect.github.com/actions/cache/compare/v4.0.0...v4.0.1)

#### What's Changed

-   Update README.md by [@&#8203;yacaovsnc](https://redirect.github.com/yacaovsnc) in [https://github.com/actions/cache/pull/1304](https://redirect.github.com/actions/cache/pull/1304)
-   Update examples by [@&#8203;yacaovsnc](https://redirect.github.com/yacaovsnc) in [https://github.com/actions/cache/pull/1305](https://redirect.github.com/actions/cache/pull/1305)
-   Update actions/cache publish flow by [@&#8203;bethanyj28](https://redirect.github.com/bethanyj28) in [https://github.com/actions/cache/pull/1340](https://redirect.github.com/actions/cache/pull/1340)
-   Update [@&#8203;actions/cache](https://redirect.github.com/actions/cache) by [@&#8203;bethanyj28](https://redirect.github.com/bethanyj28) in [https://github.com/actions/cache/pull/1341](https://redirect.github.com/actions/cache/pull/1341)

#### New Contributors

-   [@&#8203;yacaovsnc](https://redirect.github.com/yacaovsnc) made their first contribution in [https://github.com/actions/cache/pull/1304](https://redirect.github.com/actions/cache/pull/1304)

**Full Changelog**: https://github.com/actions/cache/compare/v4...v4.0.1

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

_Label `internal` added by @renovate[bot] on 2025-04-07 02:04_

---

_Closed by @MichaReiser on 2025-04-07 06:25_

---

_Reopened by @MichaReiser on 2025-04-07 06:25_

---

_Closed by @MichaReiser on 2025-04-07 06:34_

---

_Reopened by @MichaReiser on 2025-04-07 06:34_

---

_Merged by @MichaReiser on 2025-04-07 06:47_

---

_Closed by @MichaReiser on 2025-04-07 06:47_

---

_Branch deleted on 2025-04-07 06:47_

---

_Comment by @github-actions[bot] on 2025-04-07 06:50_

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
