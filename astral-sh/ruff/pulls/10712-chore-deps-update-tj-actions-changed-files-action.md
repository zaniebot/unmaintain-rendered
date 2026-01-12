```yaml
number: 10712
title: "chore(deps): update tj-actions/changed-files action to v44"
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/tj-actions-changed-files-44.x
created_at: 2024-04-01T12:29:07Z
updated_at: 2024-04-01T13:19:29Z
url: https://github.com/astral-sh/ruff/pull/10712
synced_at: 2026-01-12T15:55:33Z
```

# chore(deps): update tj-actions/changed-files action to v44

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [tj-actions/changed-files](https://togithub.com/tj-actions/changed-files) | action | major | `v43` -> `v44` |

---

### Release Notes

<details>
<summary>tj-actions/changed-files (tj-actions/changed-files)</summary>

### [`v44`](https://togithub.com/tj-actions/changed-files/releases/tag/v44)

[Compare Source](https://togithub.com/tj-actions/changed-files/compare/v43...v44)

##### Changes in v44.0.0

##### 🔥🔥 BREAKING CHANGE 🔥🔥

##### Overview

We've made a significant update to how pull requests (PRs) from forked repositories are processed. This improvement not only streamlines the handling of such PRs but also fixes a previously identified issue.

##### Before the Change

Previously, when you created a pull request from a forked repository, any files changed in the target branch after the PR creation would erroneously appear as part of the PR's changed files. This made it difficult to distinguish between the actual changes introduced by the PR and subsequent changes made directly to the target branch.

##### What Has Changed

With this update, a pull request from a fork will now **only** include the files that were explicitly changed in the fork. This ensures that the list of changed files in a PR accurately reflects the contributions from the fork, without being muddled by unrelated changes to the target branch.

***

##### What's Changed

-   Upgraded to v43.0.1 by [@&#8203;tj-actions-bot](https://togithub.com/tj-actions-bot) in [https://github.com/tj-actions/changed-files/pull/2004](https://togithub.com/tj-actions/changed-files/pull/2004)
-   chore(deps): lock file maintenance by [@&#8203;renovate](https://togithub.com/renovate) in [https://github.com/tj-actions/changed-files/pull/2005](https://togithub.com/tj-actions/changed-files/pull/2005)
-   chore(deps): update typescript-eslint monorepo to v7.4.0 by [@&#8203;renovate](https://togithub.com/renovate) in [https://github.com/tj-actions/changed-files/pull/2006](https://togithub.com/tj-actions/changed-files/pull/2006)
-   fix: bug with prs from forks returning incorrect set of changed files by [@&#8203;jackton1](https://togithub.com/jackton1) in [https://github.com/tj-actions/changed-files/pull/2007](https://togithub.com/tj-actions/changed-files/pull/2007)
-   fix: check for setting remote urls for forks by [@&#8203;jackton1](https://togithub.com/jackton1) in [https://github.com/tj-actions/changed-files/pull/2009](https://togithub.com/tj-actions/changed-files/pull/2009)
-   fix: update to add the fork remote by [@&#8203;jackton1](https://togithub.com/jackton1) in [https://github.com/tj-actions/changed-files/pull/2010](https://togithub.com/tj-actions/changed-files/pull/2010)
-   fix: update previous sha for forks by [@&#8203;jackton1](https://togithub.com/jackton1) in [https://github.com/tj-actions/changed-files/pull/2011](https://togithub.com/tj-actions/changed-files/pull/2011)
-   fix: ensure the fork remote doesn't exists before creating it by [@&#8203;jackton1](https://togithub.com/jackton1) in [https://github.com/tj-actions/changed-files/pull/2012](https://togithub.com/tj-actions/changed-files/pull/2012)
-   chore: update description of other_deleted_files output by [@&#8203;tonyejack1](https://togithub.com/tonyejack1) in [https://github.com/tj-actions/changed-files/pull/2008](https://togithub.com/tj-actions/changed-files/pull/2008)
-   Updated README.md by [@&#8203;tj-actions-bot](https://togithub.com/tj-actions-bot) in [https://github.com/tj-actions/changed-files/pull/2013](https://togithub.com/tj-actions/changed-files/pull/2013)
-   remove: unused code by [@&#8203;jackton1](https://togithub.com/jackton1) in [https://github.com/tj-actions/changed-files/pull/2014](https://togithub.com/tj-actions/changed-files/pull/2014)
-   chore: update description of outputs removing asterisks  by [@&#8203;tonyejack1](https://togithub.com/tonyejack1) in [https://github.com/tj-actions/changed-files/pull/2015](https://togithub.com/tj-actions/changed-files/pull/2015)
-   Updated README.md by [@&#8203;tj-actions-bot](https://togithub.com/tj-actions-bot) in [https://github.com/tj-actions/changed-files/pull/2016](https://togithub.com/tj-actions/changed-files/pull/2016)

##### New Contributors

-   [@&#8203;tonyejack1](https://togithub.com/tonyejack1) made their first contribution in [https://github.com/tj-actions/changed-files/pull/2008](https://togithub.com/tj-actions/changed-files/pull/2008)

**Full Changelog**: https://github.com/tj-actions/changed-files/compare/v43.0.1...v44.0.0

***

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Label `internal` added by @renovate[bot] on 2024-04-01 12:29_

---

_Comment by @github-actions[bot] on 2024-04-01 12:52_

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

_Merged by @AlexWaygood on 2024-04-01 13:19_

---

_Closed by @AlexWaygood on 2024-04-01 13:19_

---

_Branch deleted on 2024-04-01 13:19_

---
