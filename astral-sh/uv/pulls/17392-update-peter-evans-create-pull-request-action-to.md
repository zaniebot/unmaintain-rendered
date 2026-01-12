```yaml
number: 17392
title: Update peter-evans/create-pull-request action to v8
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/peter-evans-create-pull-request-8.x
created_at: 2026-01-09T21:32:06Z
updated_at: 2026-01-12T09:52:12Z
url: https://github.com/astral-sh/uv/pull/17392
synced_at: 2026-01-12T16:12:45Z
```

# Update peter-evans/create-pull-request action to v8

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [peter-evans/create-pull-request](https://redirect.github.com/peter-evans/create-pull-request) | action | major | `v7.0.8` → `v8.0.0` |

---

### Release Notes

<details>
<summary>peter-evans/create-pull-request (peter-evans/create-pull-request)</summary>

### [`v8.0.0`](https://redirect.github.com/peter-evans/create-pull-request/releases/tag/v8.0.0): Create Pull Request v8.0.0

[Compare Source](https://redirect.github.com/peter-evans/create-pull-request/compare/v7.0.11...v8.0.0)

##### What's new in v8

- Requires [Actions Runner v2.327.1](https://redirect.github.com/actions/runner/releases/tag/v2.327.1) or later if you are using a self-hosted runner for Node 24 support.

##### What's Changed

- chore: Update checkout action version to v6 by [@&#8203;yonas](https://redirect.github.com/yonas) in [#&#8203;4258](https://redirect.github.com/peter-evans/create-pull-request/pull/4258)
- Update actions/checkout references to [@&#8203;v6](https://redirect.github.com/v6) in docs by [@&#8203;Copilot](https://redirect.github.com/Copilot) in [#&#8203;4259](https://redirect.github.com/peter-evans/create-pull-request/pull/4259)
- feat: v8 by [@&#8203;peter-evans](https://redirect.github.com/peter-evans) in [#&#8203;4260](https://redirect.github.com/peter-evans/create-pull-request/pull/4260)

##### New Contributors

- [@&#8203;yonas](https://redirect.github.com/yonas) made their first contribution in [#&#8203;4258](https://redirect.github.com/peter-evans/create-pull-request/pull/4258)
- [@&#8203;Copilot](https://redirect.github.com/Copilot) made their first contribution in [#&#8203;4259](https://redirect.github.com/peter-evans/create-pull-request/pull/4259)

**Full Changelog**: <https://github.com/peter-evans/create-pull-request/compare/v7.0.11...v8.0.0>

### [`v7.0.11`](https://redirect.github.com/peter-evans/create-pull-request/releases/tag/v7.0.11): Create Pull Request v7.0.11

[Compare Source](https://redirect.github.com/peter-evans/create-pull-request/compare/v7.0.10...v7.0.11)

##### What's Changed

- fix: restrict remote prune to self-hosted runners by [@&#8203;peter-evans](https://redirect.github.com/peter-evans) in [#&#8203;4250](https://redirect.github.com/peter-evans/create-pull-request/pull/4250)

**Full Changelog**: <https://github.com/peter-evans/create-pull-request/compare/v7.0.10...v7.0.11>

### [`v7.0.10`](https://redirect.github.com/peter-evans/create-pull-request/releases/tag/v7.0.10): Create Pull Request v7.0.10

[Compare Source](https://redirect.github.com/peter-evans/create-pull-request/compare/v7.0.9...v7.0.10)

⚙️ Fixes an issue where updating a pull request failed when targeting a forked repository with the same owner as its parent.

##### What's Changed

- build(deps): bump the github-actions group with 2 updates by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;4235](https://redirect.github.com/peter-evans/create-pull-request/pull/4235)
- build(deps-dev): bump prettier from 3.6.2 to 3.7.3 in the npm group by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;4240](https://redirect.github.com/peter-evans/create-pull-request/pull/4240)
- fix: provider list pulls fallback for multi fork same owner by [@&#8203;peter-evans](https://redirect.github.com/peter-evans) in [#&#8203;4245](https://redirect.github.com/peter-evans/create-pull-request/pull/4245)

##### New Contributors

- [@&#8203;obnyis](https://redirect.github.com/obnyis) made their first contribution in [#&#8203;4064](https://redirect.github.com/peter-evans/create-pull-request/pull/4064)

**Full Changelog**: <https://github.com/peter-evans/create-pull-request/compare/v7.0.9...v7.0.10>

### [`v7.0.9`](https://redirect.github.com/peter-evans/create-pull-request/releases/tag/v7.0.9): Create Pull Request v7.0.9

[Compare Source](https://redirect.github.com/peter-evans/create-pull-request/compare/v7.0.8...v7.0.9)

⚙️ Fixes an [incompatibility](https://redirect.github.com/peter-evans/create-pull-request/issues/4228) with the recently released `actions/checkout@v6`.

#### What's Changed

- \~70 dependency updates by [@&#8203;dependabot](https://redirect.github.com/dependabot)
- docs: fix workaround description about `ready_for_review` by [@&#8203;ybiquitous](https://redirect.github.com/ybiquitous) in [#&#8203;3939](https://redirect.github.com/peter-evans/create-pull-request/pull/3939)
- Docs: `add-paths` default behavior by [@&#8203;joeflack4](https://redirect.github.com/joeflack4) in [#&#8203;3928](https://redirect.github.com/peter-evans/create-pull-request/pull/3928)
- docs: update to create-github-app-token v2 by [@&#8203;Goooler](https://redirect.github.com/Goooler) in [#&#8203;4063](https://redirect.github.com/peter-evans/create-pull-request/pull/4063)
- Fix compatibility with actions/checkout\@&#8203;v6 by [@&#8203;ericsciple](https://redirect.github.com/ericsciple) in [#&#8203;4230](https://redirect.github.com/peter-evans/create-pull-request/pull/4230)

#### New Contributors

- [@&#8203;joeflack4](https://redirect.github.com/joeflack4) made their first contribution in [#&#8203;3928](https://redirect.github.com/peter-evans/create-pull-request/pull/3928)
- [@&#8203;Goooler](https://redirect.github.com/Goooler) made their first contribution in [#&#8203;4063](https://redirect.github.com/peter-evans/create-pull-request/pull/4063)
- [@&#8203;ericsciple](https://redirect.github.com/ericsciple) made their first contribution in [#&#8203;4230](https://redirect.github.com/peter-evans/create-pull-request/pull/4230)

**Full Changelog**: <https://github.com/peter-evans/create-pull-request/compare/v7.0.8...v7.0.9>

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

_Label `internal` added by @renovate[bot] on 2026-01-09 21:32_

---

_Merged by @konstin on 2026-01-12 09:52_

---

_Closed by @konstin on 2026-01-12 09:52_

---

_Branch deleted on 2026-01-12 09:52_

---
