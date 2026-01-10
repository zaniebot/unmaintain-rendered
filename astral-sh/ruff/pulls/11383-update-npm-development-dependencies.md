```yaml
number: 11383
title: Update NPM Development dependencies
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/npm-development-dependencies
created_at: 2024-05-13T00:23:38Z
updated_at: 2024-05-13T00:30:59Z
url: https://github.com/astral-sh/ruff/pull/11383
synced_at: 2026-01-10T22:05:26Z
```

# Update NPM Development dependencies

---

_Pull request opened by @renovate on 2024-05-13 00:23_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [@types/react](https://togithub.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/react) ([source](https://togithub.com/DefinitelyTyped/DefinitelyTyped/tree/HEAD/types/react)) | [`18.3.1` -> `18.3.2`](https://renovatebot.com/diffs/npm/@types%2freact/18.3.1/18.3.2) | [![age](https://developer.mend.io/api/mc/badges/age/npm/@types%2freact/18.3.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/@types%2freact/18.3.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/@types%2freact/18.3.1/18.3.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/@types%2freact/18.3.1/18.3.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [miniflare](https://togithub.com/cloudflare/workers-sdk/tree/main/packages/miniflare#readme) ([source](https://togithub.com/cloudflare/workers-sdk/tree/HEAD/packages/miniflare)) | [`3.20240419.0` -> `3.20240419.1`](https://renovatebot.com/diffs/npm/miniflare/3.20240419.0/3.20240419.1) | [![age](https://developer.mend.io/api/mc/badges/age/npm/miniflare/3.20240419.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/miniflare/3.20240419.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/miniflare/3.20240419.0/3.20240419.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/miniflare/3.20240419.0/3.20240419.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [wrangler](https://togithub.com/cloudflare/workers-sdk) ([source](https://togithub.com/cloudflare/workers-sdk/tree/HEAD/packages/wrangler)) | [`3.53.1` -> `3.55.0`](https://renovatebot.com/diffs/npm/wrangler/3.53.1/3.55.0) | [![age](https://developer.mend.io/api/mc/badges/age/npm/wrangler/3.55.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/wrangler/3.55.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/wrangler/3.53.1/3.55.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/wrangler/3.53.1/3.55.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>cloudflare/workers-sdk (miniflare)</summary>

### [`v3.20240419.1`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/miniflare/CHANGELOG.md#3202404191)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/miniflare@3.20240419.0...miniflare@3.20240419.1)

##### Minor Changes

-   [#&#8203;5570](https://togithub.com/cloudflare/workers-sdk/pull/5570) [`66bdad0`](https://togithub.com/cloudflare/workers-sdk/commit/66bdad08834b403100d1e4d6cd507978cc50eaba) Thanks [@&#8203;sesteves](https://togithub.com/sesteves)! - feature: support delayed delivery in the miniflare's queue simulator.

    This change updates the miniflare's queue broker to support delayed delivery of messages, both when sending the message from a producer and when retrying the message from a consumer.

##### Patch Changes

-   [#&#8203;5670](https://togithub.com/cloudflare/workers-sdk/pull/5670) [`9b4af8a`](https://togithub.com/cloudflare/workers-sdk/commit/9b4af8a59bc75ed494dd752c0a7007dbacf75e51) Thanks [@&#8203;dario-piotrowicz](https://togithub.com/dario-piotrowicz)! - fix: Allow the magic proxy to proxy objects containing functions

    This was previously prevented but this change removes that restriction.

</details>

<details>
<summary>cloudflare/workers-sdk (wrangler)</summary>

### [`v3.55.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#3550)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@3.53.1...wrangler@3.55.0)

##### Minor Changes

-   [#&#8203;5570](https://togithub.com/cloudflare/workers-sdk/pull/5570) [`66bdad0`](https://togithub.com/cloudflare/workers-sdk/commit/66bdad08834b403100d1e4d6cd507978cc50eaba) Thanks [@&#8203;sesteves](https://togithub.com/sesteves)! - feature: support delayed delivery in the miniflare's queue simulator.

    This change updates the miniflare's queue broker to support delayed delivery of messages, both when sending the message from a producer and when retrying the message from a consumer.

##### Patch Changes

-   [#&#8203;5740](https://togithub.com/cloudflare/workers-sdk/pull/5740) [`97741db`](https://togithub.com/cloudflare/workers-sdk/commit/97741dbf8ff7498bcaa381361d61ad17af10f088) Thanks [@&#8203;WalshyDev](https://togithub.com/WalshyDev)! - chore: log "Version ID" in `wrangler deploy`, `wrangler deployments list`, `wrangler deployments view` and `wrangler rollback` to support migration from the deprecated "Deployment ID". Users should update any parsing to use "Version ID" before "Deployment ID" is removed.

-   [#&#8203;5754](https://togithub.com/cloudflare/workers-sdk/pull/5754) [`f673c66`](https://togithub.com/cloudflare/workers-sdk/commit/f673c66373e2acd8d9cc94d5afa87b07dd3d750c) Thanks [@&#8203;RamIdeas](https://togithub.com/RamIdeas)! - fix: when using custom builds, the `wrangler dev` proxy server was sometimes left in a paused state

    This could be observed as the browser loading indefinitely, after saving a source file (unchanged) when using custom builds. This is now fixed by ensuring the proxy server is unpaused after a short timeout period.

-   Updated dependencies \[[`66bdad0`](https://togithub.com/cloudflare/workers-sdk/commit/66bdad08834b403100d1e4d6cd507978cc50eaba), [`9b4af8a`](https://togithub.com/cloudflare/workers-sdk/commit/9b4af8a59bc75ed494dd752c0a7007dbacf75e51)]:
    -   miniflare@3.20240419.1

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://togithub.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4zNTEuMiIsInVwZGF0ZWRJblZlciI6IjM3LjM1MS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-05-13 00:23_

---

_Merged by @charliermarsh on 2024-05-13 00:30_

---

_Closed by @charliermarsh on 2024-05-13 00:30_

---

_Branch deleted on 2024-05-13 00:30_

---
