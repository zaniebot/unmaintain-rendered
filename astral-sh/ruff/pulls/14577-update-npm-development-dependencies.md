```yaml
number: 14577
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
created_at: 2024-11-25T01:36:24Z
updated_at: 2024-11-25T07:54:53Z
url: https://github.com/astral-sh/ruff/pull/14577
synced_at: 2026-01-10T20:50:57Z
```

# Update NPM Development dependencies

---

_Pull request opened by @renovate on 2024-11-25 01:36_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [@typescript-eslint/eslint-plugin](https://typescript-eslint.io/packages/eslint-plugin) ([source](https://redirect.github.com/typescript-eslint/typescript-eslint/tree/HEAD/packages/eslint-plugin)) | [`8.14.0` -> `8.15.0`](https://renovatebot.com/diffs/npm/@typescript-eslint%2feslint-plugin/8.14.0/8.15.0) | [![age](https://developer.mend.io/api/mc/badges/age/npm/@typescript-eslint%2feslint-plugin/8.15.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/@typescript-eslint%2feslint-plugin/8.15.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/@typescript-eslint%2feslint-plugin/8.14.0/8.15.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/@typescript-eslint%2feslint-plugin/8.14.0/8.15.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [@typescript-eslint/parser](https://typescript-eslint.io/packages/parser) ([source](https://redirect.github.com/typescript-eslint/typescript-eslint/tree/HEAD/packages/parser)) | [`8.14.0` -> `8.15.0`](https://renovatebot.com/diffs/npm/@typescript-eslint%2fparser/8.14.0/8.15.0) | [![age](https://developer.mend.io/api/mc/badges/age/npm/@typescript-eslint%2fparser/8.15.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/@typescript-eslint%2fparser/8.15.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/@typescript-eslint%2fparser/8.14.0/8.15.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/@typescript-eslint%2fparser/8.14.0/8.15.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [miniflare](https://redirect.github.com/cloudflare/workers-sdk/tree/main/packages/miniflare#readme) ([source](https://redirect.github.com/cloudflare/workers-sdk/tree/HEAD/packages/miniflare)) | [`3.20241106.0` -> `3.20241106.1`](https://renovatebot.com/diffs/npm/miniflare/3.20241106.0/3.20241106.1) | [![age](https://developer.mend.io/api/mc/badges/age/npm/miniflare/3.20241106.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/miniflare/3.20241106.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/miniflare/3.20241106.0/3.20241106.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/miniflare/3.20241106.0/3.20241106.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [typescript](https://www.typescriptlang.org/) ([source](https://redirect.github.com/microsoft/TypeScript)) | [`5.6.3` -> `5.7.2`](https://renovatebot.com/diffs/npm/typescript/5.6.3/5.7.2) | [![age](https://developer.mend.io/api/mc/badges/age/npm/typescript/5.7.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/typescript/5.7.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/typescript/5.6.3/5.7.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/typescript/5.6.3/5.7.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [wrangler](https://redirect.github.com/cloudflare/workers-sdk) ([source](https://redirect.github.com/cloudflare/workers-sdk/tree/HEAD/packages/wrangler)) | [`3.87.0` -> `3.90.0`](https://renovatebot.com/diffs/npm/wrangler/3.87.0/3.90.0) | [![age](https://developer.mend.io/api/mc/badges/age/npm/wrangler/3.90.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/wrangler/3.90.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/wrangler/3.87.0/3.90.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/wrangler/3.87.0/3.90.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>typescript-eslint/typescript-eslint (@&#8203;typescript-eslint/eslint-plugin)</summary>

### [`v8.15.0`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/eslint-plugin/CHANGELOG.md#8150-2024-11-18)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.14.0...v8.15.0)

##### 🚀 Features

-   **eslint-plugin:** \[prefer-nullish-coalescing] fix detection of `ignoreConditionalTests` involving boolean `!` operator ([#&#8203;10299](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10299))
-   **eslint-plugin:** new rule `no-unsafe-type-assertion` ([#&#8203;10051](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10051))
-   **eslint-plugin:** added related-getter-setter-pairs rule ([#&#8203;10192](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10192))

##### 🩹 Fixes

-   **utils:** add defaultOptions to meta in rule ([#&#8203;10339](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10339))
-   **eslint-plugin:** report deprecations used in default export ([#&#8203;10330](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10330))
-   **eslint-plugin:** \[explicit-module-boundary-types] and \[explicit-function-return-type] don't report on `as const satisfies` ([#&#8203;10315](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10315))
-   **eslint-plugin:** \[await-thenable, return-await] don't flag awaiting unconstrained type parameter as unnecessary ([#&#8203;10314](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10314))
-   **eslint-plugin:** \[consistent-indexed-object-style] handle circular mapped types ([#&#8203;10301](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10301))

##### ❤️  Thank You

-   Josh Goldberg ✨
-   Kim Sang Du [@&#8203;developer-bandi](https://redirect.github.com/developer-bandi)
-   Luis Sebastian Urrutia Fuentes [@&#8203;LuisUrrutia](https://redirect.github.com/LuisUrrutia)
-   Phillip Huang
-   Ronen Amiel
-   Szydlak [@&#8203;wszydlak](https://redirect.github.com/wszydlak)

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

</details>

<details>
<summary>typescript-eslint/typescript-eslint (@&#8203;typescript-eslint/parser)</summary>

### [`v8.15.0`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/parser/CHANGELOG.md#8150-2024-11-18)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.14.0...v8.15.0)

This was a version bump only for parser to align it with other projects, there were no code changes.

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

</details>

<details>
<summary>cloudflare/workers-sdk (miniflare)</summary>

### [`v3.20241106.1`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/miniflare/CHANGELOG.md#3202411061)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/miniflare@3.20241106.0...miniflare@3.20241106.1)

##### Minor Changes

-   [#&#8203;7286](https://redirect.github.com/cloudflare/workers-sdk/pull/7286) [`563439b`](https://redirect.github.com/cloudflare/workers-sdk/commit/563439bd02c450921b28d721d36be5a70897690d) Thanks [@&#8203;LuisDuarte1](https://redirect.github.com/LuisDuarte1)! - Add proper engine persistance in .wrangler and fix multiple workflows in miniflare

</details>

<details>
<summary>microsoft/TypeScript (typescript)</summary>

### [`v5.7.2`](https://redirect.github.com/microsoft/TypeScript/compare/v5.6.3...d701d908d534e68cfab24b6df15539014ac348a3)

[Compare Source](https://redirect.github.com/microsoft/TypeScript/compare/v5.6.3...v5.7.2)

</details>

<details>
<summary>cloudflare/workers-sdk (wrangler)</summary>

### [`v3.90.0`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#3900)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/wrangler@3.89.0...wrangler@3.90.0)

##### Minor Changes

-   [#&#8203;7315](https://redirect.github.com/cloudflare/workers-sdk/pull/7315) [`31729ee`](https://redirect.github.com/cloudflare/workers-sdk/commit/31729ee63df0fbaf34787ab9e5a53f7180d0ec8c) Thanks [@&#8203;G4brym](https://redirect.github.com/G4brym)! - Update local AI fetcher to forward method and url to upstream

##### Patch Changes

-   Updated dependencies \[[`6ba5903`](https://redirect.github.com/cloudflare/workers-sdk/commit/6ba5903201de34cb3a8a5610fa11825279171a7e)]:
    -   [@&#8203;cloudflare/workers-shared](https://redirect.github.com/cloudflare/workers-shared)[@&#8203;0](https://redirect.github.com/0).8.0
    -   miniflare@3.20241106.1

### [`v3.89.0`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#3890)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/wrangler@3.88.0...wrangler@3.89.0)

##### Minor Changes

-   [#&#8203;7252](https://redirect.github.com/cloudflare/workers-sdk/pull/7252) [`97acf07`](https://redirect.github.com/cloudflare/workers-sdk/commit/97acf07b3e09192b71e81a722029d026a7198b8b) Thanks [@&#8203;Maximo-Guk](https://redirect.github.com/Maximo-Guk)! - feat: Add production_branch and deployment_trigger to pages deploy detailed artifact for wrangler-action pages parity

-   [#&#8203;7263](https://redirect.github.com/cloudflare/workers-sdk/pull/7263) [`1b80dec`](https://redirect.github.com/cloudflare/workers-sdk/commit/1b80decfaf56c8782e49dad685c344288629b668) Thanks [@&#8203;danielrs](https://redirect.github.com/danielrs)! - Fix wrangler pages deployment (list|tail) environment filtering.

##### Patch Changes

-   [#&#8203;7314](https://redirect.github.com/cloudflare/workers-sdk/pull/7314) [`a30c805`](https://redirect.github.com/cloudflare/workers-sdk/commit/a30c8056621f44063082a81d06f10e723844059f) Thanks [@&#8203;Ankcorn](https://redirect.github.com/Ankcorn)! - Fix observability.logs.enabled validation

-   [#&#8203;7285](https://redirect.github.com/cloudflare/workers-sdk/pull/7285) [`fa21312`](https://redirect.github.com/cloudflare/workers-sdk/commit/fa21312c6625680709e05547c13897bc1fa8c9d3) Thanks [@&#8203;penalosa](https://redirect.github.com/penalosa)! - Rename `directory` to `projectRoot` and ensure it's relative to the `wrangler.toml`. This fixes a regression which meant that `.wrangler` temporary folders were inadvertently generated relative to `process.cwd()` rather than the location of the `wrangler.toml` file. It also renames `directory` to `projectRoot`, which affects the \`unstable_startWorker() interface.

-   Updated dependencies \[[`563439b`](https://redirect.github.com/cloudflare/workers-sdk/commit/563439bd02c450921b28d721d36be5a70897690d)]:
    -   miniflare@3.20241106.1

### [`v3.88.0`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#3880)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/wrangler@3.87.0...wrangler@3.88.0)

##### Minor Changes

-   [#&#8203;7173](https://redirect.github.com/cloudflare/workers-sdk/pull/7173) [`b6cbfbd`](https://redirect.github.com/cloudflare/workers-sdk/commit/b6cbfbdd10dfbb732ec12a5c69bd4a74b07de8a0) Thanks [@&#8203;Ankcorn](https://redirect.github.com/Ankcorn)! - Adds \[observability.logs] settings to wrangler. This setting lets developers control the settings for logs as an independent dataset enabling more dataset types in the future. The most specific setting will win if any of the datasets are not enabled.

    It also adds the following setting to the logs config

    -   `invocation_logs` - set to false to disable invocation logs. Defaults to true.

    ```toml
    [observability.logs]
    enabled = true
    invocation_logs = false
    ```

-   [#&#8203;7207](https://redirect.github.com/cloudflare/workers-sdk/pull/7207) [`edec415`](https://redirect.github.com/cloudflare/workers-sdk/commit/edec41591dcf37262d459568c0f454820b90dbaa) Thanks [@&#8203;jonesphillip](https://redirect.github.com/jonesphillip)! - Added r2 bucket lifecycle command to Wrangler including list, add, remove, set

##### Patch Changes

-   [#&#8203;7243](https://redirect.github.com/cloudflare/workers-sdk/pull/7243) [`941d411`](https://redirect.github.com/cloudflare/workers-sdk/commit/941d4110ca84510d235b72b3f98692e4188a7ad4) Thanks [@&#8203;penalosa](https://redirect.github.com/penalosa)! - Include Version Preview URL in Wrangler's output file

-   [#&#8203;7038](https://redirect.github.com/cloudflare/workers-sdk/pull/7038) [`e2e6912`](https://redirect.github.com/cloudflare/workers-sdk/commit/e2e6912bcb7a1f6b7f8081b889a4e08be8a740a1) Thanks [@&#8203;petebacondarwin](https://redirect.github.com/petebacondarwin)! - fix: only show fetch warning if on old compatibility_date

    Now that we have the `allow_custom_ports` compatibility flag, we only need to show the fetch warnings when that flag is not enabled.

    Fixes [https://github.com/cloudflare/workerd/issues/2955](https://redirect.github.com/cloudflare/workerd/issues/2955)

-   [#&#8203;7216](https://redirect.github.com/cloudflare/workers-sdk/pull/7216) [`09e6e90`](https://redirect.github.com/cloudflare/workers-sdk/commit/09e6e905d9825d33b8e90acabb8ff7b962cc908b) Thanks [@&#8203;vicb](https://redirect.github.com/vicb)! - chore(wrangler): update unenv dependency version

-   [#&#8203;7081](https://redirect.github.com/cloudflare/workers-sdk/pull/7081) [`b4a0e74`](https://redirect.github.com/cloudflare/workers-sdk/commit/b4a0e74680440084342477fc9373f9f76ab91c0b) Thanks [@&#8203;penalosa](https://redirect.github.com/penalosa)! - Default the file based registry (`--x-registry`) to on. This should improve stability of multi-worker development

-   Updated dependencies \[]:
    -   miniflare@3.20241106.0

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xOS4wIiwidXBkYXRlZEluVmVyIjoiMzkuMTkuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-11-25 01:36_

---

_Merged by @MichaReiser on 2024-11-25 07:54_

---

_Closed by @MichaReiser on 2024-11-25 07:54_

---

_Branch deleted on 2024-11-25 07:54_

---
