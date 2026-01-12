```yaml
number: 14059
title: Update actions/checkout action to v4.2.2
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/actions-checkout-4.x
created_at: 2025-06-16T01:44:52Z
updated_at: 2025-06-16T02:27:04Z
url: https://github.com/astral-sh/uv/pull/14059
synced_at: 2026-01-12T16:11:00Z
```

# Update actions/checkout action to v4.2.2

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [actions/checkout](https://redirect.github.com/actions/checkout) | action | minor | `v4` -> `v4.2.2` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>actions/checkout (actions/checkout)</summary>

### [`v4.2.2`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v422)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.2.1...v4.2.2)

-   `url-helper.ts` now leverages well-known environment variables by [@&#8203;jww3](https://redirect.github.com/jww3) in [https://github.com/actions/checkout/pull/1941](https://redirect.github.com/actions/checkout/pull/1941)
-   Expand unit test coverage for `isGhes` by [@&#8203;jww3](https://redirect.github.com/jww3) in [https://github.com/actions/checkout/pull/1946](https://redirect.github.com/actions/checkout/pull/1946)

### [`v4.2.1`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v421)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.2.0...v4.2.1)

-   Check out other refs/\* by commit if provided, fall back to ref by [@&#8203;orhantoy](https://redirect.github.com/orhantoy) in [https://github.com/actions/checkout/pull/1924](https://redirect.github.com/actions/checkout/pull/1924)

### [`v4.2.0`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v420)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.1.7...v4.2.0)

-   Add Ref and Commit outputs by [@&#8203;lucacome](https://redirect.github.com/lucacome) in [https://github.com/actions/checkout/pull/1180](https://redirect.github.com/actions/checkout/pull/1180)
-   Dependency updates by [@&#8203;dependabot-](https://redirect.github.com/dependabot-) [https://github.com/actions/checkout/pull/1777](https://redirect.github.com/actions/checkout/pull/1777), [https://github.com/actions/checkout/pull/1872](https://redirect.github.com/actions/checkout/pull/1872)

### [`v4.1.7`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v417)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.1.6...v4.1.7)

-   Bump the minor-npm-dependencies group across 1 directory with 4 updates by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/checkout/pull/1739](https://redirect.github.com/actions/checkout/pull/1739)
-   Bump actions/checkout from 3 to 4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/checkout/pull/1697](https://redirect.github.com/actions/checkout/pull/1697)
-   Check out other refs/\* by commit by [@&#8203;orhantoy](https://redirect.github.com/orhantoy) in [https://github.com/actions/checkout/pull/1774](https://redirect.github.com/actions/checkout/pull/1774)
-   Pin actions/checkout's own workflows to a known, good, stable version. by [@&#8203;jww3](https://redirect.github.com/jww3) in [https://github.com/actions/checkout/pull/1776](https://redirect.github.com/actions/checkout/pull/1776)

### [`v4.1.6`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v416)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.1.5...v4.1.6)

-   Check platform to set archive extension appropriately by [@&#8203;cory-miller](https://redirect.github.com/cory-miller) in [https://github.com/actions/checkout/pull/1732](https://redirect.github.com/actions/checkout/pull/1732)

### [`v4.1.5`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v415)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.1.4...v4.1.5)

-   Update NPM dependencies by [@&#8203;cory-miller](https://redirect.github.com/cory-miller) in [https://github.com/actions/checkout/pull/1703](https://redirect.github.com/actions/checkout/pull/1703)
-   Bump github/codeql-action from 2 to 3 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/checkout/pull/1694](https://redirect.github.com/actions/checkout/pull/1694)
-   Bump actions/setup-node from 1 to 4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/checkout/pull/1696](https://redirect.github.com/actions/checkout/pull/1696)
-   Bump actions/upload-artifact from 2 to 4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/checkout/pull/1695](https://redirect.github.com/actions/checkout/pull/1695)
-   README: Suggest `user.email` to be `41898282+github-actions[bot]@&#8203;users.noreply.github.com` by [@&#8203;cory-miller](https://redirect.github.com/cory-miller) in [https://github.com/actions/checkout/pull/1707](https://redirect.github.com/actions/checkout/pull/1707)

### [`v4.1.4`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v414)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.1.3...v4.1.4)

-   Disable `extensions.worktreeConfig` when disabling `sparse-checkout` by [@&#8203;jww3](https://redirect.github.com/jww3) in [https://github.com/actions/checkout/pull/1692](https://redirect.github.com/actions/checkout/pull/1692)
-   Add dependabot config by [@&#8203;cory-miller](https://redirect.github.com/cory-miller) in [https://github.com/actions/checkout/pull/1688](https://redirect.github.com/actions/checkout/pull/1688)
-   Bump the minor-actions-dependencies group with 2 updates by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/checkout/pull/1693](https://redirect.github.com/actions/checkout/pull/1693)
-   Bump word-wrap from 1.2.3 to 1.2.5 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [https://github.com/actions/checkout/pull/1643](https://redirect.github.com/actions/checkout/pull/1643)

### [`v4.1.3`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v413)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.1.2...v4.1.3)

-   Check git version before attempting to disable `sparse-checkout` by [@&#8203;jww3](https://redirect.github.com/jww3) in [https://github.com/actions/checkout/pull/1656](https://redirect.github.com/actions/checkout/pull/1656)
-   Add SSH user parameter by [@&#8203;cory-miller](https://redirect.github.com/cory-miller) in [https://github.com/actions/checkout/pull/1685](https://redirect.github.com/actions/checkout/pull/1685)
-   Update `actions/checkout` version in `update-main-version.yml` by [@&#8203;jww3](https://redirect.github.com/jww3) in [https://github.com/actions/checkout/pull/1650](https://redirect.github.com/actions/checkout/pull/1650)

### [`v4.1.2`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v412)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.1.1...v4.1.2)

-   Fix: Disable sparse checkout whenever `sparse-checkout` option is not present [@&#8203;dscho](https://redirect.github.com/dscho) in [https://github.com/actions/checkout/pull/1598](https://redirect.github.com/actions/checkout/pull/1598)

### [`v4.1.1`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v411)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4.1.0...v4.1.1)

-   Correct link to GitHub Docs by [@&#8203;peterbe](https://redirect.github.com/peterbe) in [https://github.com/actions/checkout/pull/1511](https://redirect.github.com/actions/checkout/pull/1511)
-   Link to release page from what's new section by [@&#8203;cory-miller](https://redirect.github.com/cory-miller) in [https://github.com/actions/checkout/pull/1514](https://redirect.github.com/actions/checkout/pull/1514)

### [`v4.1.0`](https://redirect.github.com/actions/checkout/blob/HEAD/CHANGELOG.md#v410)

[Compare Source](https://redirect.github.com/actions/checkout/compare/v4...v4.1.0)

-   [Add support for partial checkout filters](https://redirect.github.com/actions/checkout/pull/1396)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC41MC4wIiwidXBkYXRlZEluVmVyIjoiNDAuNTAuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-06-16 01:44_

---

_Merged by @charliermarsh on 2025-06-16 02:27_

---

_Closed by @charliermarsh on 2025-06-16 02:27_

---

_Branch deleted on 2025-06-16 02:27_

---
