```yaml
number: 14078
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
created_at: 2024-11-04T01:58:06Z
updated_at: 2024-11-04T02:16:24Z
url: https://github.com/astral-sh/ruff/pull/14078
synced_at: 2026-01-12T15:55:46Z
```

# Update NPM Development dependencies

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [@typescript-eslint/eslint-plugin](https://typescript-eslint.io/packages/eslint-plugin) ([source](https://redirect.github.com/typescript-eslint/typescript-eslint/tree/HEAD/packages/eslint-plugin)) | [`8.11.0` -> `8.12.2`](https://renovatebot.com/diffs/npm/@typescript-eslint%2feslint-plugin/8.11.0/8.12.2) | [![age](https://developer.mend.io/api/mc/badges/age/npm/@typescript-eslint%2feslint-plugin/8.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/@typescript-eslint%2feslint-plugin/8.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/@typescript-eslint%2feslint-plugin/8.11.0/8.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/@typescript-eslint%2feslint-plugin/8.11.0/8.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [@typescript-eslint/parser](https://typescript-eslint.io/packages/parser) ([source](https://redirect.github.com/typescript-eslint/typescript-eslint/tree/HEAD/packages/parser)) | [`8.11.0` -> `8.12.2`](https://renovatebot.com/diffs/npm/@typescript-eslint%2fparser/8.11.0/8.12.2) | [![age](https://developer.mend.io/api/mc/badges/age/npm/@typescript-eslint%2fparser/8.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/@typescript-eslint%2fparser/8.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/@typescript-eslint%2fparser/8.11.0/8.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/@typescript-eslint%2fparser/8.11.0/8.12.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |
| [wrangler](https://redirect.github.com/cloudflare/workers-sdk) ([source](https://redirect.github.com/cloudflare/workers-sdk/tree/HEAD/packages/wrangler)) | [`3.83.0` -> `3.84.1`](https://renovatebot.com/diffs/npm/wrangler/3.83.0/3.84.1) | [![age](https://developer.mend.io/api/mc/badges/age/npm/wrangler/3.84.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/wrangler/3.84.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/wrangler/3.83.0/3.84.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/wrangler/3.83.0/3.84.1?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>typescript-eslint/typescript-eslint (@&#8203;typescript-eslint/eslint-plugin)</summary>

### [`v8.12.2`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/eslint-plugin/CHANGELOG.md#8122-2024-10-29)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.12.1...v8.12.2)

##### 🩹 Fixes

-   **eslint-plugin:** \[switch-exhaustiveness-check] invert `considerDefaultExhaustiveForUnions` ([#&#8203;10223](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10223))

##### ❤️  Thank You

-   Kirk Waiblinger [@&#8203;kirkwaiblinger](https://redirect.github.com/kirkwaiblinger)

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

### [`v8.12.1`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/eslint-plugin/CHANGELOG.md#8121-2024-10-28)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.12.0...v8.12.1)

This was a version bump only for eslint-plugin to align it with other projects, there were no code changes.

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

### [`v8.12.0`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/eslint-plugin/CHANGELOG.md#8120-2024-10-28)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.11.0...v8.12.0)

##### 🚀 Features

-   **eslint-plugin:** \[no-base-to-string] handle String() ([#&#8203;10005](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10005))
-   **eslint-plugin:** \[switch-exhaustiveness-check] add allowDefaultCaseMatchUnionMember option ([#&#8203;9954](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/9954))
-   **eslint-plugin:** \[consistent-indexed-object-style] report mapped types ([#&#8203;10160](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10160))
-   **eslint-plugin:** \[prefer-nullish-coalescing] add support for assignment expressions ([#&#8203;10152](https://redirect.github.com/typescript-eslint/typescript-eslint/pull/10152))

##### ❤️  Thank You

-   Abraham Guo
-   Kim Sang Du [@&#8203;developer-bandi](https://redirect.github.com/developer-bandi)
-   Kirk Waiblinger [@&#8203;kirkwaiblinger](https://redirect.github.com/kirkwaiblinger)
-   YeonJuan [@&#8203;yeonjuan](https://redirect.github.com/yeonjuan)

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

</details>

<details>
<summary>typescript-eslint/typescript-eslint (@&#8203;typescript-eslint/parser)</summary>

### [`v8.12.2`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/parser/CHANGELOG.md#8122-2024-10-29)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.12.1...v8.12.2)

This was a version bump only for parser to align it with other projects, there were no code changes.

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

### [`v8.12.1`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/parser/CHANGELOG.md#8121-2024-10-28)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.12.0...v8.12.1)

This was a version bump only for parser to align it with other projects, there were no code changes.

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

### [`v8.12.0`](https://redirect.github.com/typescript-eslint/typescript-eslint/blob/HEAD/packages/parser/CHANGELOG.md#8120-2024-10-28)

[Compare Source](https://redirect.github.com/typescript-eslint/typescript-eslint/compare/v8.11.0...v8.12.0)

This was a version bump only for parser to align it with other projects, there were no code changes.

You can read about our [versioning strategy](https://main--typescript-eslint.netlify.app/users/versioning) and [releases](https://main--typescript-eslint.netlify.app/users/releases) on our website.

</details>

<details>
<summary>cloudflare/workers-sdk (wrangler)</summary>

### [`v3.84.1`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#3841)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/wrangler@3.84.0...wrangler@3.84.1)

##### Patch Changes

-   [#&#8203;7141](https://redirect.github.com/cloudflare/workers-sdk/pull/7141) [`d938bb3`](https://redirect.github.com/cloudflare/workers-sdk/commit/d938bb395d77e2be9ab708eb4ace722fc39153e8) Thanks [@&#8203;emily-shen](https://redirect.github.com/emily-shen)! - fix: throw a better error if there is an "ASSETS" user binding in a Pages projects

-   [#&#8203;7124](https://redirect.github.com/cloudflare/workers-sdk/pull/7124) [`f8ebdd1`](https://redirect.github.com/cloudflare/workers-sdk/commit/f8ebdd1b2ba7cdc30a17c19dc66aed064213b2a6) Thanks [@&#8203;skepticfx](https://redirect.github.com/skepticfx)! - fix: Modify Cloudchamber deployment labels in interactive mode

### [`v3.84.0`](https://redirect.github.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#3840)

[Compare Source](https://redirect.github.com/cloudflare/workers-sdk/compare/wrangler@3.83.0...wrangler@3.84.0)

##### Minor Changes

-   [#&#8203;6999](https://redirect.github.com/cloudflare/workers-sdk/pull/6999) [`0111edb`](https://redirect.github.com/cloudflare/workers-sdk/commit/0111edb9da466184885085be5e755ceb4970a486) Thanks [@&#8203;garvit-gupta](https://redirect.github.com/garvit-gupta)! - docs: Vectorize GA Announcement Banner

-   [#&#8203;6916](https://redirect.github.com/cloudflare/workers-sdk/pull/6916) [`a33a133`](https://redirect.github.com/cloudflare/workers-sdk/commit/a33a133f884741d347f85f059631ae6461c46fdd) Thanks [@&#8203;garrettgu10](https://redirect.github.com/garrettgu10)! - Local development now supports Vectorize bindings

-   [#&#8203;7004](https://redirect.github.com/cloudflare/workers-sdk/pull/7004) [`15ef013`](https://redirect.github.com/cloudflare/workers-sdk/commit/15ef013f1bd006915d01477e9e65f8ac51e7dce9) Thanks [@&#8203;garvit-gupta](https://redirect.github.com/garvit-gupta)! - feat: Enable Vectorize query by id via Wrangler

-   [#&#8203;7092](https://redirect.github.com/cloudflare/workers-sdk/pull/7092) [`038fdd9`](https://redirect.github.com/cloudflare/workers-sdk/commit/038fdd97aaab9db3b6a76cd0e0d9cf7a786f9ac8) Thanks [@&#8203;jonesphillip](https://redirect.github.com/jonesphillip)! - Added location hint option for the Wrangler R2 bucket create command

-   [#&#8203;7024](https://redirect.github.com/cloudflare/workers-sdk/pull/7024) [`bd66d51`](https://redirect.github.com/cloudflare/workers-sdk/commit/bd66d511a90dd7a635ec94e95f806be7de569212) Thanks [@&#8203;xortive](https://redirect.github.com/xortive)! - feature: allow using a connection string when updating hyperdrive configs

    both `hyperdrive create` and `hyperdrive update` now support updating configs with connection strings.

##### Patch Changes

-   [#&#8203;7091](https://redirect.github.com/cloudflare/workers-sdk/pull/7091) [`68a2a84`](https://redirect.github.com/cloudflare/workers-sdk/commit/68a2a8460375cfa0fba8c7c7384b0168e5e4415d) Thanks [@&#8203;taylorlee](https://redirect.github.com/taylorlee)! - fix: synchronize observability settings during `wrangler versions deploy`

    When running `wrangler versions deploy`, Wrangler will now update `observability` settings in addition to `logpush` and `tail_consumers`. Unlike `wrangler deploy`, it will not disable observability when `observability` is undefined in `wrangler.toml`.

-   [#&#8203;7080](https://redirect.github.com/cloudflare/workers-sdk/pull/7080) [`924ec18`](https://redirect.github.com/cloudflare/workers-sdk/commit/924ec18c249f49700d070e725be675fd5f99259b) Thanks [@&#8203;vicb](https://redirect.github.com/vicb)! - chore(wrangler): update unenv dependency version

-   [#&#8203;7097](https://redirect.github.com/cloudflare/workers-sdk/pull/7097) [`8ca4b32`](https://redirect.github.com/cloudflare/workers-sdk/commit/8ca4b327443c38df55236509e2a782c6496ba89d) Thanks [@&#8203;emily-shen](https://redirect.github.com/emily-shen)! - fix: remove deprecation warnings for `wrangler init`

    We will not be removing `wrangler init` (it just delegates to create-cloudflare now). These warnings were causing confusion for users as it `wrangler init` is still recommended in many places.

-   [#&#8203;7073](https://redirect.github.com/cloudflare/workers-sdk/pull/7073) [`656a444`](https://redirect.github.com/cloudflare/workers-sdk/commit/656a444fc7d363c1b7154fdf73eed0a81b003882) Thanks [@&#8203;penalosa](https://redirect.github.com/penalosa)! - Internal refactor to remove `es-module-lexer` and support `wrangler types` for Workers with Durable Objects & JSX

-   [#&#8203;7024](https://redirect.github.com/cloudflare/workers-sdk/pull/7024) [`bd66d51`](https://redirect.github.com/cloudflare/workers-sdk/commit/bd66d511a90dd7a635ec94e95f806be7de569212) Thanks [@&#8203;xortive](https://redirect.github.com/xortive)! - fix: make individual parameters work for `wrangler hyperdrive create` when not using HoA

    `wrangler hyperdrive create` individual parameters were not setting the database name correctly when calling the api.

-   [#&#8203;7024](https://redirect.github.com/cloudflare/workers-sdk/pull/7024) [`bd66d51`](https://redirect.github.com/cloudflare/workers-sdk/commit/bd66d511a90dd7a635ec94e95f806be7de569212) Thanks [@&#8203;xortive](https://redirect.github.com/xortive)! - refactor: use same param parsing code for `wrangler hyperdrive create` and `wrangler hyperdrive update`

    ensures that going forward, both commands support the same features and have the same names for config flags

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4xNDIuNCIsInVwZGF0ZWRJblZlciI6IjM4LjE0Mi40IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-11-04 01:58_

---

_Merged by @charliermarsh on 2024-11-04 02:16_

---

_Closed by @charliermarsh on 2024-11-04 02:16_

---

_Branch deleted on 2024-11-04 02:16_

---
