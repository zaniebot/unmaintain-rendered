```yaml
number: 14999
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
created_at: 2024-12-16T01:13:37Z
updated_at: 2024-12-16T01:25:37Z
url: https://github.com/astral-sh/ruff/pull/14999
synced_at: 2026-01-12T15:55:49Z
```

# Update NPM Development dependencies

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [@types/react-dom](https://redirect.github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/react-dom) ([source](https://redirect.github.com/DefinitelyTyped/DefinitelyTyped/tree/HEAD/types/react-dom)) | [`19.0.1` -> `19.0.2`](https://renovatebot.com/diffs/npm/@types%2freact-dom/19.0.1/19.0.2) | [![age](https://developer.mend.io/api/mc/badges/age/npm/@types%2freact-dom/19.0.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/@types%2freact-dom/19.0.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/@types%2freact-dom/19.0.1/19.0.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/@types%2freact-dom/19.0.1/19.0.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [@typescript-eslint/eslint-plugin](https://typescript-eslint.io/packages/eslint-plugin) ([source](https://redirect.github.com/typescript-eslint/typescript-eslint/tree/HEAD/packages/eslint-plugin)) | [`8.17.0` -> `8.18.0`](https://renovatebot.com/diffs/npm/@typescript-eslint%2feslint-plugin/8.17.0/8.18.0) | [![age](https://developer.mend.io/api/mc/badges/age/npm/@typescript-eslint%2feslint-plugin/8.18.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/@typescript-eslint%2feslint-plugin/8.18.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/@typescript-eslint%2feslint-plugin/8.17.0/8.18.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/@typescript-eslint%2feslint-plugin/8.17.0/8.18.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [@typescript-eslint/parser](https://typescript-eslint.io/packages/parser) ([source](https://redirect.github.com/typescript-eslint/typescript-eslint/tree/HEAD/packages/parser)) | [`8.17.0` -> `8.18.0`](https://renovatebot.com/diffs/npm/@typescript-eslint%2fparser/8.17.0/8.18.0) | [![age](https://developer.mend.io/api/mc/badges/age/npm/@typescript-eslint%2fparser/8.18.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/@typescript-eslint%2fparser/8.18.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/@typescript-eslint%2fparser/8.17.0/8.18.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/@typescript-eslint%2fparser/8.17.0/8.18.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [wrangler](https://redirect.github.com/cloudflare/workers-sdk) ([source](https://redirect.github.com/cloudflare/workers-sdk/tree/HEAD/packages/wrangler)) | [`3.93.0` -> `3.95.0`](https://renovatebot.com/diffs/npm/wrangler/3.93.0/3.95.0) | [![age](https://developer.mend.io/api/mc/badges/age/npm/wrangler/3.95.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/wrangler/3.95.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/wrangler/3.93.0/3.95.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/wrangler/3.93.0/3.95.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>typescript-eslint/typescript-eslint (@&#8203;typescript-eslint/eslint-plugin)</summary>

### [`v8.18.0`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/eslint-plugin/CHANGELOG.md#8180-2024-12-09)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.17.0...v8.18.0)

##### 🚀 Features

-   **eslint-plugin:** \[switch-exhaustiveness-check] add support for "no default" comment ([#&#8203;10218](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10218))
-   **eslint-plugin:** \[no-deprecated] report on super call of deprecated constructor ([#&#8203;10397](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10397))

##### 🩹 Fixes

-   **eslint-plugin:** \[use-unknown-in-catch-callback-variable] only flag function literals ([#&#8203;10436](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10436))
-   **eslint-plugin:** \[no-base-to-string] handle more robustly when multiple `toString()` declarations are present for a type ([#&#8203;10432](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10432))
-   **eslint-plugin:** \[no-deprecated] check if a JSX attribute is deprecated ([#&#8203;10374](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10374))
-   typescript peer dependency ([#&#8203;10373](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10373))

##### ❤️  Thank You

-   Kim Sang Du [@&#8203;developer-bandi](https://redirect.github.com/developer-bandi)
-   Kirk Waiblinger [@&#8203;kirkwaiblinger](https://redirect.github.com/kirkwaiblinger)
-   mdm317
-   rtritto

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

</details>

<details>
<summary>typescript-eslint/typescript-eslint (@&#8203;typescript-eslint/parser)</summary>

### [`v8.18.0`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/parser/CHANGELOG.md#8180-2024-12-09)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.17.0...v8.18.0)

##### 🩹 Fixes

-   typescript peer dependency ([#&#8203;10373](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10373))

##### ❤️  Thank You

-   rtritto

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

</details>

<details>
<summary>cloudflare/workers-sdk (wrangler)</summary>

### [`v3.95.0`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#3950)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/wrangler@3.94.0...wrangler@3.95.0)

##### Minor Changes

-   [#&#8203;7382](https://redirect.github.com/cloudflare/workers-sdk/pull/7382) [`e0b98fd`](https://redirect.github.com/cloudflare/workers-sdk/commit/e0b98fdb6eefffff16fc0624517cd9e5fce93c98) Thanks [@&#8203;jonesphillip](https://redirect.github.com/jonesphillip)! - Added r2 bucket cors command to Wrangler including list, set, delete

### [`v3.94.0`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#3940)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/wrangler@3.93.0...wrangler@3.94.0)

##### Minor Changes

-   [#&#8203;7229](https://redirect.github.com/cloudflare/workers-sdk/pull/7229) [`669d7ad`](https://redirect.github.com/cloudflare/workers-sdk/commit/669d7ad1e44c07cf74202c4d0fc244a9c50dec81) Thanks [@&#8203;gabivlj](https://redirect.github.com/gabivlj)! - Introduce a new cloudchamber command `wrangler cloudchamber apply`, which will be used by customers to deploy container-apps

##### Patch Changes

-   [#&#8203;7002](https://redirect.github.com/cloudflare/workers-sdk/pull/7002) [`d2447c6`](https://redirect.github.com/cloudflare/workers-sdk/commit/d2447c6c1ebcdebf0829519c3bc52bc2d30a4294) Thanks [@&#8203;GregBrimble](https://redirect.github.com/GregBrimble)! - fix: More helpful error messages when validating compatibility date

-   [#&#8203;7493](https://redirect.github.com/cloudflare/workers-sdk/pull/7493) [`4c140bc`](https://redirect.github.com/cloudflare/workers-sdk/commit/4c140bcb2b75a3dcf12240d66c22619af10503df) Thanks [@&#8203;emily-shen](https://redirect.github.com/emily-shen)! - fix: remove non-json output in json mode commands

    Fixes regressions in 3.93.0 where unwanted text (wrangler banner, telemetry notice) was printing in commands that should only output valid json.

-   Updated dependencies \[[`5449fe5`](https://redirect.github.com/cloudflare/workers-sdk/commit/5449fe54b15cf7c6dd12c385b0c8d2883c641b80)]:
    -   [@&#8203;cloudflare/workers-shared](https://redirect.github.com/cloudflare/workers-shared)[@&#8203;0](https://redirect.github.com/0).11.0
    -   miniflare@3.20241205.0

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://redirect.github.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS41OC4xIiwidXBkYXRlZEluVmVyIjoiMzkuNTguMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-12-16 01:13_

---

_Merged by @charliermarsh on 2024-12-16 01:25_

---

_Closed by @charliermarsh on 2024-12-16 01:25_

---

_Branch deleted on 2024-12-16 01:25_

---
