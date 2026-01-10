```yaml
number: 10605
title: Update dependency vite to v5 - autoclosed
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/vite-5.x
created_at: 2024-03-26T09:58:40Z
updated_at: 2024-03-26T10:40:42Z
url: https://github.com/astral-sh/ruff/pull/10605
synced_at: 2026-01-10T22:47:02Z
```

# Update dependency vite to v5 - autoclosed

---

_Pull request opened by @renovate on 2024-03-26 09:58_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [vite](https://togithub.com/vitejs/vite/tree/main/#readme) ([source](https://togithub.com/vitejs/vite/tree/HEAD/packages/vite)) | [`^4.0.0` -> `^5.0.0`](https://renovatebot.com/diffs/npm/vite/4.5.2/5.2.6) | [![age](https://developer.mend.io/api/mc/badges/age/npm/vite/5.2.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/vite/5.2.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/vite/4.5.2/5.2.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/vite/4.5.2/5.2.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>vitejs/vite (vite)</summary>

### [`v5.2.6`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small526-2024-03-24-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.2.5...v5.2.6)

-   fix: `fs.deny` with globs with directories ([#&#8203;16250](https://togithub.com/vitejs/vite/issues/16250)) ([ba5269c](https://togithub.com/vitejs/vite/commit/ba5269c)), closes [#&#8203;16250](https://togithub.com/vitejs/vite/issues/16250)

### [`v5.2.5`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small525-2024-03-24-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.2.4...v5.2.5)

-   fix: avoid SSR requests in waitForRequestIdle ([#&#8203;16246](https://togithub.com/vitejs/vite/issues/16246)) ([7093f77](https://togithub.com/vitejs/vite/commit/7093f77)), closes [#&#8203;16246](https://togithub.com/vitejs/vite/issues/16246)
-   docs: clarify enforce vs hook.order ([#&#8203;16226](https://togithub.com/vitejs/vite/issues/16226)) ([3a73e48](https://togithub.com/vitejs/vite/commit/3a73e48)), closes [#&#8203;16226](https://togithub.com/vitejs/vite/issues/16226)

### [`v5.2.4`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small524-2024-03-23-small)

-   fix: dont resolve imports with malformed URI ([#&#8203;16244](https://togithub.com/vitejs/vite/issues/16244)) ([fbf69d5](https://togithub.com/vitejs/vite/commit/fbf69d5)), closes [#&#8203;16244](https://togithub.com/vitejs/vite/issues/16244)

### [`v5.2.3`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small523-2024-03-22-small)

-   fix: handle warmup request error correctly ([#&#8203;16223](https://togithub.com/vitejs/vite/issues/16223)) ([d7c5256](https://togithub.com/vitejs/vite/commit/d7c5256)), closes [#&#8203;16223](https://togithub.com/vitejs/vite/issues/16223)
-   fix: skip encode if is data uri ([#&#8203;16233](https://togithub.com/vitejs/vite/issues/16233)) ([8617e76](https://togithub.com/vitejs/vite/commit/8617e76)), closes [#&#8203;16233](https://togithub.com/vitejs/vite/issues/16233)
-   fix(optimizer): fix `optimizeDeps.include` glob syntax for `./*` exports ([#&#8203;16230](https://togithub.com/vitejs/vite/issues/16230)) ([f184c80](https://togithub.com/vitejs/vite/commit/f184c80)), closes [#&#8203;16230](https://togithub.com/vitejs/vite/issues/16230)
-   fix(runtime): fix sourcemap with `prepareStackTrace` ([#&#8203;16220](https://togithub.com/vitejs/vite/issues/16220)) ([dad7f4f](https://togithub.com/vitejs/vite/commit/dad7f4f)), closes [#&#8203;16220](https://togithub.com/vitejs/vite/issues/16220)
-   chore: `utf8` replaced with `utf-8` ([#&#8203;16232](https://togithub.com/vitejs/vite/issues/16232)) ([9800c73](https://togithub.com/vitejs/vite/commit/9800c73)), closes [#&#8203;16232](https://togithub.com/vitejs/vite/issues/16232)

### [`v5.2.2`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small522-2024-03-20-small)

-   fix(importAnalysis): skip encode in ssr ([#&#8203;16213](https://togithub.com/vitejs/vite/issues/16213)) ([e4d2d60](https://togithub.com/vitejs/vite/commit/e4d2d60)), closes [#&#8203;16213](https://togithub.com/vitejs/vite/issues/16213)

### [`v5.2.1`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small521-2024-03-20-small)

-   fix: encode path uri only ([#&#8203;16212](https://togithub.com/vitejs/vite/issues/16212)) ([0b2e40b](https://togithub.com/vitejs/vite/commit/0b2e40b)), closes [#&#8203;16212](https://togithub.com/vitejs/vite/issues/16212)

### [`v5.2.0`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#520-2024-03-20)

-   fix: update client.ts@cleanUrl to accomodate blob protocol ([#&#8203;16182](https://togithub.com/vitejs/vite/issues/16182)) ([1a3b1d7](https://togithub.com/vitejs/vite/commit/1a3b1d7)), closes [#&#8203;16182](https://togithub.com/vitejs/vite/issues/16182)
-   fix(assets): avoid splitting `,` inside query parameter of image URI in srcset property ([#&#8203;16081](https://togithub.com/vitejs/vite/issues/16081)) ([50caf67](https://togithub.com/vitejs/vite/commit/50caf67)), closes [#&#8203;16081](https://togithub.com/vitejs/vite/issues/16081)
-   chore(deps): update all non-major dependencies ([#&#8203;16186](https://togithub.com/vitejs/vite/issues/16186)) ([842643d](https://togithub.com/vitejs/vite/commit/842643d)), closes [#&#8203;16186](https://togithub.com/vitejs/vite/issues/16186)
-   perf(transformRequest): fast-path watch and sourcemap handling ([#&#8203;16170](https://togithub.com/vitejs/vite/issues/16170)) ([de60f1e](https://togithub.com/vitejs/vite/commit/de60f1e)), closes [#&#8203;16170](https://togithub.com/vitejs/vite/issues/16170)
-   docs: add `@shikiji/vitepress-twoslash` ([#&#8203;16168](https://togithub.com/vitejs/vite/issues/16168)) ([6f8a320](https://togithub.com/vitejs/vite/commit/6f8a320)), closes [#&#8203;16168](https://togithub.com/vitejs/vite/issues/16168)

### [`v5.1.7`](https://togithub.com/vitejs/vite/releases/tag/v5.1.7)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.1.6...v5.1.7)

Please refer to [CHANGELOG.md](https://togithub.com/vitejs/vite/blob/v5.1.7/packages/vite/CHANGELOG.md) for details.

### [`v5.1.6`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small516-2024-03-11-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.1.5...v5.1.6)

-   chore(deps): update all non-major dependencies ([#&#8203;16131](https://togithub.com/vitejs/vite/issues/16131)) ([a862ecb](https://togithub.com/vitejs/vite/commit/a862ecb)), closes [#&#8203;16131](https://togithub.com/vitejs/vite/issues/16131)
-   fix: check for publicDir before checking if it is a parent directory ([#&#8203;16046](https://togithub.com/vitejs/vite/issues/16046)) ([b6fb323](https://togithub.com/vitejs/vite/commit/b6fb323)), closes [#&#8203;16046](https://togithub.com/vitejs/vite/issues/16046)
-   fix: escape single quote when relative base is used ([#&#8203;16060](https://togithub.com/vitejs/vite/issues/16060)) ([8f74ce4](https://togithub.com/vitejs/vite/commit/8f74ce4)), closes [#&#8203;16060](https://togithub.com/vitejs/vite/issues/16060)
-   fix: handle function property extension in namespace import ([#&#8203;16113](https://togithub.com/vitejs/vite/issues/16113)) ([f699194](https://togithub.com/vitejs/vite/commit/f699194)), closes [#&#8203;16113](https://togithub.com/vitejs/vite/issues/16113)
-   fix: server middleware mode resolve ([#&#8203;16122](https://togithub.com/vitejs/vite/issues/16122)) ([8403546](https://togithub.com/vitejs/vite/commit/8403546)), closes [#&#8203;16122](https://togithub.com/vitejs/vite/issues/16122)
-   fix(esbuild): update tsconfck to fix bug that could cause a deadlock  ([#&#8203;16124](https://togithub.com/vitejs/vite/issues/16124)) ([fd9de04](https://togithub.com/vitejs/vite/commit/fd9de04)), closes [#&#8203;16124](https://togithub.com/vitejs/vite/issues/16124)
-   fix(worker): hide "The emitted file overwrites" warning if the content is same ([#&#8203;16094](https://togithub.com/vitejs/vite/issues/16094)) ([60dfa9e](https://togithub.com/vitejs/vite/commit/60dfa9e)), closes [#&#8203;16094](https://togithub.com/vitejs/vite/issues/16094)
-   fix(worker): throw error when circular worker import is detected and support self referencing worker ([eef9da1](https://togithub.com/vitejs/vite/commit/eef9da1)), closes [#&#8203;16103](https://togithub.com/vitejs/vite/issues/16103)
-   style(utils): remove null check ([#&#8203;16112](https://togithub.com/vitejs/vite/issues/16112)) ([0d2df52](https://togithub.com/vitejs/vite/commit/0d2df52)), closes [#&#8203;16112](https://togithub.com/vitejs/vite/issues/16112)
-   refactor(runtime): share more code between runtime and main bundle ([#&#8203;16063](https://togithub.com/vitejs/vite/issues/16063)) ([93be84e](https://togithub.com/vitejs/vite/commit/93be84e)), closes [#&#8203;16063](https://togithub.com/vitejs/vite/issues/16063)

### [`v5.1.5`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small515-2024-03-04-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.1.4...v5.1.5)

-   fix: `__vite__mapDeps` code injection ([#&#8203;15732](https://togithub.com/vitejs/vite/issues/15732)) ([aff54e1](https://togithub.com/vitejs/vite/commit/aff54e1)), closes [#&#8203;15732](https://togithub.com/vitejs/vite/issues/15732)
-   fix: analysing build chunk without dependencies ([#&#8203;15469](https://togithub.com/vitejs/vite/issues/15469)) ([bd52283](https://togithub.com/vitejs/vite/commit/bd52283)), closes [#&#8203;15469](https://togithub.com/vitejs/vite/issues/15469)
-   fix: import with query with imports field ([#&#8203;16085](https://togithub.com/vitejs/vite/issues/16085)) ([ab823ab](https://togithub.com/vitejs/vite/commit/ab823ab)), closes [#&#8203;16085](https://togithub.com/vitejs/vite/issues/16085)
-   fix: normalize literal-only entry pattern ([#&#8203;16010](https://togithub.com/vitejs/vite/issues/16010)) ([1dccc37](https://togithub.com/vitejs/vite/commit/1dccc37)), closes [#&#8203;16010](https://togithub.com/vitejs/vite/issues/16010)
-   fix: optimizeDeps.entries with literal-only pattern(s) ([#&#8203;15853](https://togithub.com/vitejs/vite/issues/15853)) ([49300b3](https://togithub.com/vitejs/vite/commit/49300b3)), closes [#&#8203;15853](https://togithub.com/vitejs/vite/issues/15853)
-   fix: output correct error for empty import specifier ([#&#8203;16055](https://togithub.com/vitejs/vite/issues/16055)) ([a9112eb](https://togithub.com/vitejs/vite/commit/a9112eb)), closes [#&#8203;16055](https://togithub.com/vitejs/vite/issues/16055)
-   fix: upgrade esbuild to 0.20.x ([#&#8203;16062](https://togithub.com/vitejs/vite/issues/16062)) ([899d9b1](https://togithub.com/vitejs/vite/commit/899d9b1)), closes [#&#8203;16062](https://togithub.com/vitejs/vite/issues/16062)
-   fix(runtime): runtime HMR affects only imported files ([#&#8203;15898](https://togithub.com/vitejs/vite/issues/15898)) ([57463fc](https://togithub.com/vitejs/vite/commit/57463fc)), closes [#&#8203;15898](https://togithub.com/vitejs/vite/issues/15898)
-   fix(scanner): respect  `experimentalDecorators: true` ([#&#8203;15206](https://togithub.com/vitejs/vite/issues/15206)) ([4144781](https://togithub.com/vitejs/vite/commit/4144781)), closes [#&#8203;15206](https://togithub.com/vitejs/vite/issues/15206)
-   revert: "fix: upgrade esbuild to 0.20.x" ([#&#8203;16072](https://togithub.com/vitejs/vite/issues/16072)) ([11cceea](https://togithub.com/vitejs/vite/commit/11cceea)), closes [#&#8203;16072](https://togithub.com/vitejs/vite/issues/16072)
-   refactor: share code with vite runtime ([#&#8203;15907](https://togithub.com/vitejs/vite/issues/15907)) ([b20d542](https://togithub.com/vitejs/vite/commit/b20d542)), closes [#&#8203;15907](https://togithub.com/vitejs/vite/issues/15907)
-   refactor(runtime): use functions from `pathe` ([#&#8203;16061](https://togithub.com/vitejs/vite/issues/16061)) ([aac2ef7](https://togithub.com/vitejs/vite/commit/aac2ef7)), closes [#&#8203;16061](https://togithub.com/vitejs/vite/issues/16061)
-   chore(deps): update all non-major dependencies ([#&#8203;16028](https://togithub.com/vitejs/vite/issues/16028)) ([7cfe80d](https://togithub.com/vitejs/vite/commit/7cfe80d)), closes [#&#8203;16028](https://togithub.com/vitejs/vite/issues/16028)

### [`v5.1.4`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small514-2024-02-21-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.1.3...v5.1.4)

-   perf: remove unnecessary regex s modifier ([#&#8203;15766](https://togithub.com/vitejs/vite/issues/15766)) ([8dc1b73](https://togithub.com/vitejs/vite/commit/8dc1b73)), closes [#&#8203;15766](https://togithub.com/vitejs/vite/issues/15766)
-   fix: fs cached checks disabled by default for yarn pnp ([#&#8203;15920](https://togithub.com/vitejs/vite/issues/15920)) ([8b11fea](https://togithub.com/vitejs/vite/commit/8b11fea)), closes [#&#8203;15920](https://togithub.com/vitejs/vite/issues/15920)
-   fix: resolve directory correctly when `fs.cachedChecks: true` ([#&#8203;15983](https://togithub.com/vitejs/vite/issues/15983)) ([4fe971f](https://togithub.com/vitejs/vite/commit/4fe971f)), closes [#&#8203;15983](https://togithub.com/vitejs/vite/issues/15983)
-   fix: srcSet with optional descriptor ([#&#8203;15905](https://togithub.com/vitejs/vite/issues/15905)) ([81b3bd0](https://togithub.com/vitejs/vite/commit/81b3bd0)), closes [#&#8203;15905](https://togithub.com/vitejs/vite/issues/15905)
-   fix(deps): update all non-major dependencies ([#&#8203;15959](https://togithub.com/vitejs/vite/issues/15959)) ([571a3fd](https://togithub.com/vitejs/vite/commit/571a3fd)), closes [#&#8203;15959](https://togithub.com/vitejs/vite/issues/15959)
-   fix(watch): build watch fails when outDir is empty string ([#&#8203;15979](https://togithub.com/vitejs/vite/issues/15979)) ([1d263d3](https://togithub.com/vitejs/vite/commit/1d263d3)), closes [#&#8203;15979](https://togithub.com/vitejs/vite/issues/15979)

### [`v5.1.3`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small513-2024-02-15-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.1.2...v5.1.3)

-   fix: cachedTransformMiddleware for direct css requests ([#&#8203;15919](https://togithub.com/vitejs/vite/issues/15919)) ([5099028](https://togithub.com/vitejs/vite/commit/5099028)), closes [#&#8203;15919](https://togithub.com/vitejs/vite/issues/15919)
-   refactor(runtime): minor tweaks ([#&#8203;15904](https://togithub.com/vitejs/vite/issues/15904)) ([63a39c2](https://togithub.com/vitejs/vite/commit/63a39c2)), closes [#&#8203;15904](https://togithub.com/vitejs/vite/issues/15904)
-   refactor(runtime): seal ES module namespace object instead of feezing ([#&#8203;15914](https://togithub.com/vitejs/vite/issues/15914)) ([4172f02](https://togithub.com/vitejs/vite/commit/4172f02)), closes [#&#8203;15914](https://togithub.com/vitejs/vite/issues/15914)

### [`v5.1.2`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small512-2024-02-14-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.1.1...v5.1.2)

-   fix: normalize import file path info ([#&#8203;15772](https://togithub.com/vitejs/vite/issues/15772)) ([306df44](https://togithub.com/vitejs/vite/commit/306df44)), closes [#&#8203;15772](https://togithub.com/vitejs/vite/issues/15772)
-   fix(build): do not output build time when build fails ([#&#8203;15711](https://togithub.com/vitejs/vite/issues/15711)) ([added3e](https://togithub.com/vitejs/vite/commit/added3e)), closes [#&#8203;15711](https://togithub.com/vitejs/vite/issues/15711)
-   fix(runtime): pass path instead of fileURL to `isFilePathESM` ([#&#8203;15908](https://togithub.com/vitejs/vite/issues/15908)) ([7b15607](https://togithub.com/vitejs/vite/commit/7b15607)), closes [#&#8203;15908](https://togithub.com/vitejs/vite/issues/15908)
-   fix(worker): support UTF-8 encoding in inline workers (fixes [#&#8203;12117](https://togithub.com/vitejs/vite/issues/12117)) ([#&#8203;15866](https://togithub.com/vitejs/vite/issues/15866)) ([570e0f1](https://togithub.com/vitejs/vite/commit/570e0f1)), closes [#&#8203;12117](https://togithub.com/vitejs/vite/issues/12117) [#&#8203;15866](https://togithub.com/vitejs/vite/issues/15866)
-   chore: update license file ([#&#8203;15885](https://togithub.com/vitejs/vite/issues/15885)) ([d9adf18](https://togithub.com/vitejs/vite/commit/d9adf18)), closes [#&#8203;15885](https://togithub.com/vitejs/vite/issues/15885)
-   chore(deps): update all non-major dependencies ([#&#8203;15874](https://togithub.com/vitejs/vite/issues/15874)) ([d16ce5d](https://togithub.com/vitejs/vite/commit/d16ce5d)), closes [#&#8203;15874](https://togithub.com/vitejs/vite/issues/15874)
-   chore(deps): update dependency dotenv-expand to v11 ([#&#8203;15875](https://togithub.com/vitejs/vite/issues/15875)) ([642d528](https://togithub.com/vitejs/vite/commit/642d528)), closes [#&#8203;15875](https://togithub.com/vitejs/vite/issues/15875)

### [`v5.1.1`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small511-2024-02-09-small)

-   fix: empty CSS file was output when only .css?url is used ([#&#8203;15846](https://togithub.com/vitejs/vite/issues/15846)) ([b2873ac](https://togithub.com/vitejs/vite/commit/b2873ac)), closes [#&#8203;15846](https://togithub.com/vitejs/vite/issues/15846)
-   fix: skip not only .js but also .mjs manifest entries ([#&#8203;15841](https://togithub.com/vitejs/vite/issues/15841)) ([3d860e7](https://togithub.com/vitejs/vite/commit/3d860e7)), closes [#&#8203;15841](https://togithub.com/vitejs/vite/issues/15841)
-   chore: post 5.1 release edits ([#&#8203;15840](https://togithub.com/vitejs/vite/issues/15840)) ([9da6502](https://togithub.com/vitejs/vite/commit/9da6502)), closes [#&#8203;15840](https://togithub.com/vitejs/vite/issues/15840)

### [`v5.1.0`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#510-2024-02-08)

-   chore: revert [#&#8203;15746](https://togithub.com/vitejs/vite/issues/15746) ([#&#8203;15839](https://togithub.com/vitejs/vite/issues/15839)) ([ed875f8](https://togithub.com/vitejs/vite/commit/ed875f8)), closes [#&#8203;15746](https://togithub.com/vitejs/vite/issues/15746) [#&#8203;15839](https://togithub.com/vitejs/vite/issues/15839)
-   fix: pass `customLogger` to `loadConfigFromFile` (fix [#&#8203;15824](https://togithub.com/vitejs/vite/issues/15824)) ([#&#8203;15831](https://togithub.com/vitejs/vite/issues/15831)) ([55a3427](https://togithub.com/vitejs/vite/commit/55a3427)), closes [#&#8203;15824](https://togithub.com/vitejs/vite/issues/15824) [#&#8203;15831](https://togithub.com/vitejs/vite/issues/15831)
-   fix(deps): update all non-major dependencies ([#&#8203;15803](https://togithub.com/vitejs/vite/issues/15803)) ([e0a6ef2](https://togithub.com/vitejs/vite/commit/e0a6ef2)), closes [#&#8203;15803](https://togithub.com/vitejs/vite/issues/15803)
-   refactor: remove `vite build --force` ([#&#8203;15837](https://togithub.com/vitejs/vite/issues/15837)) ([f1a4242](https://togithub.com/vitejs/vite/commit/f1a4242)), closes [#&#8203;15837](https://togithub.com/vitejs/vite/issues/15837)

### [`v5.0.13`](https://togithub.com/vitejs/vite/releases/tag/v5.0.13)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.12...v5.0.13)

Please refer to [CHANGELOG.md](https://togithub.com/vitejs/vite/blob/v5.0.13/packages/vite/CHANGELOG.md) for details.

### [`v5.0.12`](https://togithub.com/vitejs/vite/releases/tag/v5.0.12)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.11...v5.0.12)

Please refer to [CHANGELOG.md](https://togithub.com/vitejs/vite/blob/v5.0.12/packages/vite/CHANGELOG.md) for details.

### [`v5.0.11`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small5011-2024-01-05-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.10...v5.0.11)

-   fix: don't pretransform classic script links ([#&#8203;15361](https://togithub.com/vitejs/vite/issues/15361)) ([19e3c9a](https://togithub.com/vitejs/vite/commit/19e3c9a)), closes [#&#8203;15361](https://togithub.com/vitejs/vite/issues/15361)
-   fix: inject `__vite__mapDeps` code before sourcemap file comment ([#&#8203;15483](https://togithub.com/vitejs/vite/issues/15483)) ([d2aa096](https://togithub.com/vitejs/vite/commit/d2aa096)), closes [#&#8203;15483](https://togithub.com/vitejs/vite/issues/15483)
-   fix(assets): avoid splitting `,` inside base64 value of `srcset` attribute ([#&#8203;15422](https://togithub.com/vitejs/vite/issues/15422)) ([8de7bd2](https://togithub.com/vitejs/vite/commit/8de7bd2)), closes [#&#8203;15422](https://togithub.com/vitejs/vite/issues/15422)
-   fix(html): handle offset magic-string slice error ([#&#8203;15435](https://togithub.com/vitejs/vite/issues/15435)) ([5ea9edb](https://togithub.com/vitejs/vite/commit/5ea9edb)), closes [#&#8203;15435](https://togithub.com/vitejs/vite/issues/15435)
-   chore(deps): update dependency strip-literal to v2 ([#&#8203;15475](https://togithub.com/vitejs/vite/issues/15475)) ([49d21fe](https://togithub.com/vitejs/vite/commit/49d21fe)), closes [#&#8203;15475](https://togithub.com/vitejs/vite/issues/15475)
-   chore(deps): update tj-actions/changed-files action to v41 ([#&#8203;15476](https://togithub.com/vitejs/vite/issues/15476)) ([2a540ee](https://togithub.com/vitejs/vite/commit/2a540ee)), closes [#&#8203;15476](https://togithub.com/vitejs/vite/issues/15476)

### [`v5.0.10`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small5010-2023-12-15-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.9...v5.0.10)

-   fix: omit protocol does not require pre-transform ([#&#8203;15355](https://togithub.com/vitejs/vite/issues/15355)) ([d9ae1b2](https://togithub.com/vitejs/vite/commit/d9ae1b2)), closes [#&#8203;15355](https://togithub.com/vitejs/vite/issues/15355)
-   fix(build): use base64 for inline SVG if it contains both single and double quotes ([#&#8203;15271](https://togithub.com/vitejs/vite/issues/15271)) ([1bbff16](https://togithub.com/vitejs/vite/commit/1bbff16)), closes [#&#8203;15271](https://togithub.com/vitejs/vite/issues/15271)

### [`v5.0.9`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small509-2023-12-14-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.8...v5.0.9)

-   fix: htmlFallbackMiddleware for favicon ([#&#8203;15301](https://togithub.com/vitejs/vite/issues/15301)) ([c902545](https://togithub.com/vitejs/vite/commit/c902545)), closes [#&#8203;15301](https://togithub.com/vitejs/vite/issues/15301)
-   fix: more stable hash calculation for depsOptimize ([#&#8203;15337](https://togithub.com/vitejs/vite/issues/15337)) ([2b39fe6](https://togithub.com/vitejs/vite/commit/2b39fe6)), closes [#&#8203;15337](https://togithub.com/vitejs/vite/issues/15337)
-   fix(scanner): catch all external files for glob imports ([#&#8203;15286](https://togithub.com/vitejs/vite/issues/15286)) ([129d0d0](https://togithub.com/vitejs/vite/commit/129d0d0)), closes [#&#8203;15286](https://togithub.com/vitejs/vite/issues/15286)
-   fix(server): avoid chokidar throttling on startup ([#&#8203;15347](https://togithub.com/vitejs/vite/issues/15347)) ([56a5740](https://togithub.com/vitejs/vite/commit/56a5740)), closes [#&#8203;15347](https://togithub.com/vitejs/vite/issues/15347)
-   fix(worker): replace `import.meta` correctly for IIFE worker ([#&#8203;15321](https://togithub.com/vitejs/vite/issues/15321)) ([08d093c](https://togithub.com/vitejs/vite/commit/08d093c)), closes [#&#8203;15321](https://togithub.com/vitejs/vite/issues/15321)
-   feat: log re-optimization reasons ([#&#8203;15339](https://togithub.com/vitejs/vite/issues/15339)) ([b1a6c84](https://togithub.com/vitejs/vite/commit/b1a6c84)), closes [#&#8203;15339](https://togithub.com/vitejs/vite/issues/15339)
-   chore: temporary typo ([#&#8203;15329](https://togithub.com/vitejs/vite/issues/15329)) ([7b71854](https://togithub.com/vitejs/vite/commit/7b71854)), closes [#&#8203;15329](https://togithub.com/vitejs/vite/issues/15329)
-   perf: avoid computing paths on each request ([#&#8203;15318](https://togithub.com/vitejs/vite/issues/15318)) ([0506812](https://togithub.com/vitejs/vite/commit/0506812)), closes [#&#8203;15318](https://togithub.com/vitejs/vite/issues/15318)
-   perf: temporary hack to avoid fs checks for /[@&#8203;react-refresh](https://togithub.com/react-refresh) ([#&#8203;15299](https://togithub.com/vitejs/vite/issues/15299)) ([b1d6211](https://togithub.com/vitejs/vite/commit/b1d6211)), closes [#&#8203;15299](https://togithub.com/vitejs/vite/issues/15299)

### [`v5.0.8`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small508-2023-12-12-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.7...v5.0.8)

-   perf: cached fs utils ([#&#8203;15279](https://togithub.com/vitejs/vite/issues/15279)) ([c9b61c4](https://togithub.com/vitejs/vite/commit/c9b61c4)), closes [#&#8203;15279](https://togithub.com/vitejs/vite/issues/15279)
-   fix: missing warmupRequest in transformIndexHtml ([#&#8203;15303](https://togithub.com/vitejs/vite/issues/15303)) ([103820f](https://togithub.com/vitejs/vite/commit/103820f)), closes [#&#8203;15303](https://togithub.com/vitejs/vite/issues/15303)
-   fix: public files map will be updated on add/unlink in windows ([#&#8203;15317](https://togithub.com/vitejs/vite/issues/15317)) ([921ca41](https://togithub.com/vitejs/vite/commit/921ca41)), closes [#&#8203;15317](https://togithub.com/vitejs/vite/issues/15317)
-   fix(build): decode urls in CSS files (fix [#&#8203;15109](https://togithub.com/vitejs/vite/issues/15109)) ([#&#8203;15246](https://togithub.com/vitejs/vite/issues/15246)) ([ea6a7a6](https://togithub.com/vitejs/vite/commit/ea6a7a6)), closes [#&#8203;15109](https://togithub.com/vitejs/vite/issues/15109) [#&#8203;15246](https://togithub.com/vitejs/vite/issues/15246)
-   fix(deps): update all non-major dependencies ([#&#8203;15304](https://togithub.com/vitejs/vite/issues/15304)) ([bb07f60](https://togithub.com/vitejs/vite/commit/bb07f60)), closes [#&#8203;15304](https://togithub.com/vitejs/vite/issues/15304)
-   fix(ssr): check esm file with normal file path ([#&#8203;15307](https://togithub.com/vitejs/vite/issues/15307)) ([1597170](https://togithub.com/vitejs/vite/commit/1597170)), closes [#&#8203;15307](https://togithub.com/vitejs/vite/issues/15307)

### [`v5.0.7`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small507-2023-12-08-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.6...v5.0.7)

-   fix: suppress terser warning if minify disabled ([#&#8203;15275](https://togithub.com/vitejs/vite/issues/15275)) ([3e42611](https://togithub.com/vitejs/vite/commit/3e42611)), closes [#&#8203;15275](https://togithub.com/vitejs/vite/issues/15275)
-   fix: symbolic links in public dir ([#&#8203;15264](https://togithub.com/vitejs/vite/issues/15264)) ([ef2a024](https://togithub.com/vitejs/vite/commit/ef2a024)), closes [#&#8203;15264](https://togithub.com/vitejs/vite/issues/15264)
-   fix(html): skip inlining icon and manifest links ([#&#8203;14958](https://togithub.com/vitejs/vite/issues/14958)) ([8ad81b4](https://togithub.com/vitejs/vite/commit/8ad81b4)), closes [#&#8203;14958](https://togithub.com/vitejs/vite/issues/14958)
-   chore: remove unneeded condition in getRealPath ([#&#8203;15267](https://togithub.com/vitejs/vite/issues/15267)) ([8e4655c](https://togithub.com/vitejs/vite/commit/8e4655c)), closes [#&#8203;15267](https://togithub.com/vitejs/vite/issues/15267)
-   perf: cache empty optimizer result ([#&#8203;15245](https://togithub.com/vitejs/vite/issues/15245)) ([8409b66](https://togithub.com/vitejs/vite/commit/8409b66)), closes [#&#8203;15245](https://togithub.com/vitejs/vite/issues/15245)

### [`v5.0.6`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small506-2023-12-06-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.5...v5.0.6)

-   perf: in-memory public files check ([#&#8203;15195](https://togithub.com/vitejs/vite/issues/15195)) ([0f9e1bf](https://togithub.com/vitejs/vite/commit/0f9e1bf)), closes [#&#8203;15195](https://togithub.com/vitejs/vite/issues/15195)
-   chore: remove unneccessary eslint-disable-next-line regexp/no-unused-capturing-group ([#&#8203;15247](https://togithub.com/vitejs/vite/issues/15247)) ([35a5bcf](https://togithub.com/vitejs/vite/commit/35a5bcf)), closes [#&#8203;15247](https://togithub.com/vitejs/vite/issues/15247)

### [`v5.0.5`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small505-2023-12-04-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.4...v5.0.5)

-   fix: emit `vite:preloadError` for chunks without deps ([#&#8203;15203](https://togithub.com/vitejs/vite/issues/15203)) ([d8001c5](https://togithub.com/vitejs/vite/commit/d8001c5)), closes [#&#8203;15203](https://togithub.com/vitejs/vite/issues/15203)
-   fix: esbuild glob import resolve error ([#&#8203;15140](https://togithub.com/vitejs/vite/issues/15140)) ([676804d](https://togithub.com/vitejs/vite/commit/676804d)), closes [#&#8203;15140](https://togithub.com/vitejs/vite/issues/15140)
-   fix: json error with position ([#&#8203;15225](https://togithub.com/vitejs/vite/issues/15225)) ([14be75f](https://togithub.com/vitejs/vite/commit/14be75f)), closes [#&#8203;15225](https://togithub.com/vitejs/vite/issues/15225)
-   fix: proxy html path should be encoded ([#&#8203;15223](https://togithub.com/vitejs/vite/issues/15223)) ([5b85040](https://togithub.com/vitejs/vite/commit/5b85040)), closes [#&#8203;15223](https://togithub.com/vitejs/vite/issues/15223)
-   fix(deps): update all non-major dependencies ([#&#8203;15233](https://togithub.com/vitejs/vite/issues/15233)) ([ad3adda](https://togithub.com/vitejs/vite/commit/ad3adda)), closes [#&#8203;15233](https://togithub.com/vitejs/vite/issues/15233)
-   fix(hmr): don't consider CSS dep as a circular dep ([#&#8203;15229](https://togithub.com/vitejs/vite/issues/15229)) ([5f2cdec](https://togithub.com/vitejs/vite/commit/5f2cdec)), closes [#&#8203;15229](https://togithub.com/vitejs/vite/issues/15229)
-   feat: add '\*.mov' to client.d.ts ([#&#8203;15189](https://togithub.com/vitejs/vite/issues/15189)) ([d93a211](https://togithub.com/vitejs/vite/commit/d93a211)), closes [#&#8203;15189](https://togithub.com/vitejs/vite/issues/15189)
-   feat(server): allow disabling built-in shortcuts ([#&#8203;15218](https://togithub.com/vitejs/vite/issues/15218)) ([7fd7c6c](https://togithub.com/vitejs/vite/commit/7fd7c6c)), closes [#&#8203;15218](https://togithub.com/vitejs/vite/issues/15218)
-   chore: replace 'some' with 'includes' in resolveEnvPrefix ([#&#8203;15220](https://togithub.com/vitejs/vite/issues/15220)) ([ee12f30](https://togithub.com/vitejs/vite/commit/ee12f30)), closes [#&#8203;15220](https://togithub.com/vitejs/vite/issues/15220)
-   chore: update the website url for homepage in package.json ([#&#8203;15181](https://togithub.com/vitejs/vite/issues/15181)) ([282bd8f](https://togithub.com/vitejs/vite/commit/282bd8f)), closes [#&#8203;15181](https://togithub.com/vitejs/vite/issues/15181)
-   chore: update vitest to 1.0.0-beta.6 ([#&#8203;15194](https://togithub.com/vitejs/vite/issues/15194)) ([2fce647](https://togithub.com/vitejs/vite/commit/2fce647)), closes [#&#8203;15194](https://togithub.com/vitejs/vite/issues/15194)
-   refactor: make HMR agnostic to environment ([#&#8203;15179](https://togithub.com/vitejs/vite/issues/15179)) ([0571b7c](https://togithub.com/vitejs/vite/commit/0571b7c)), closes [#&#8203;15179](https://togithub.com/vitejs/vite/issues/15179)
-   refactor: use dedicated regex methods ([#&#8203;15228](https://togithub.com/vitejs/vite/issues/15228)) ([0348137](https://togithub.com/vitejs/vite/commit/0348137)), closes [#&#8203;15228](https://togithub.com/vitejs/vite/issues/15228)
-   perf: remove debug only prettifyUrl call ([#&#8203;15204](https://togithub.com/vitejs/vite/issues/15204)) ([73e971f](https://togithub.com/vitejs/vite/commit/73e971f)), closes [#&#8203;15204](https://togithub.com/vitejs/vite/issues/15204)
-   perf: skip computing sourceRoot in injectSourcesContent ([#&#8203;15207](https://togithub.com/vitejs/vite/issues/15207)) ([1df1fd1](https://togithub.com/vitejs/vite/commit/1df1fd1)), closes [#&#8203;15207](https://togithub.com/vitejs/vite/issues/15207)

### [`v5.0.4`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small504-2023-11-29-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.3...v5.0.4)

-   fix: bindCLIShortcuts to proper server ([#&#8203;15162](https://togithub.com/vitejs/vite/issues/15162)) ([67ac572](https://togithub.com/vitejs/vite/commit/67ac572)), closes [#&#8203;15162](https://togithub.com/vitejs/vite/issues/15162)
-   fix: revert "fix: js fallback sourcemap content should be using original content ([#&#8203;15135](https://togithub.com/vitejs/vite/issues/15135))" ([#&#8203;15178](https://togithub.com/vitejs/vite/issues/15178)) ([d2a2493](https://togithub.com/vitejs/vite/commit/d2a2493)), closes [#&#8203;15135](https://togithub.com/vitejs/vite/issues/15135) [#&#8203;15178](https://togithub.com/vitejs/vite/issues/15178)
-   fix(define): allow define process.env ([#&#8203;15173](https://togithub.com/vitejs/vite/issues/15173)) ([ec401da](https://togithub.com/vitejs/vite/commit/ec401da)), closes [#&#8203;15173](https://togithub.com/vitejs/vite/issues/15173)
-   fix(resolve): respect order of browser in mainFields when resolving ([#&#8203;15137](https://togithub.com/vitejs/vite/issues/15137)) ([4a111aa](https://togithub.com/vitejs/vite/commit/4a111aa)), closes [#&#8203;15137](https://togithub.com/vitejs/vite/issues/15137)
-   feat: preserve vite.middlewares connect instance after restarts ([#&#8203;15166](https://togithub.com/vitejs/vite/issues/15166)) ([9474c4b](https://togithub.com/vitejs/vite/commit/9474c4b)), closes [#&#8203;15166](https://togithub.com/vitejs/vite/issues/15166)
-   refactor: align with Promise.withResolvers() ([#&#8203;15171](https://togithub.com/vitejs/vite/issues/15171)) ([642f9bc](https://togithub.com/vitejs/vite/commit/642f9bc)), closes [#&#8203;15171](https://togithub.com/vitejs/vite/issues/15171)

### [`v5.0.3`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small503-2023-11-28-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.2...v5.0.3)

-   fix: `generateCodeFrame` infinite loop ([#&#8203;15093](https://togithub.com/vitejs/vite/issues/15093)) ([6619de7](https://togithub.com/vitejs/vite/commit/6619de7)), closes [#&#8203;15093](https://togithub.com/vitejs/vite/issues/15093)
-   fix: js fallback sourcemap content should be using original content ([#&#8203;15135](https://togithub.com/vitejs/vite/issues/15135)) ([227d56d](https://togithub.com/vitejs/vite/commit/227d56d)), closes [#&#8203;15135](https://togithub.com/vitejs/vite/issues/15135)
-   fix(css): render correct asset url when CSS chunk name is nested ([#&#8203;15154](https://togithub.com/vitejs/vite/issues/15154)) ([ef403c0](https://togithub.com/vitejs/vite/commit/ef403c0)), closes [#&#8203;15154](https://togithub.com/vitejs/vite/issues/15154)
-   fix(css): use non-nested chunk name if facadeModule is not CSS file ([#&#8203;15155](https://togithub.com/vitejs/vite/issues/15155)) ([811e392](https://togithub.com/vitejs/vite/commit/811e392)), closes [#&#8203;15155](https://togithub.com/vitejs/vite/issues/15155)
-   fix(dev): bind plugin context functions ([#&#8203;14569](https://togithub.com/vitejs/vite/issues/14569)) ([cb3243c](https://togithub.com/vitejs/vite/commit/cb3243c)), closes [#&#8203;14569](https://togithub.com/vitejs/vite/issues/14569)
-   chore(deps): update all non-major dependencies ([#&#8203;15145](https://togithub.com/vitejs/vite/issues/15145)) ([7ff2c0a](https://togithub.com/vitejs/vite/commit/7ff2c0a)), closes [#&#8203;15145](https://togithub.com/vitejs/vite/issues/15145)
-   build: handle latest json-stable-stringify replacement ([#&#8203;15049](https://togithub.com/vitejs/vite/issues/15049)) ([bcc4a61](https://togithub.com/vitejs/vite/commit/bcc4a61)), closes [#&#8203;15049](https://togithub.com/vitejs/vite/issues/15049)

### [`v5.0.2`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small502-2023-11-21-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v5.0.1...v5.0.2)

-   fix: make htmlFallback more permissive ([#&#8203;15059](https://togithub.com/vitejs/vite/issues/15059)) ([6fcceeb](https://togithub.com/vitejs/vite/commit/6fcceeb)), closes [#&#8203;15059](https://togithub.com/vitejs/vite/issues/15059)

### [`v5.0.1`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small501-2023-11-21-small)

-   test: avoid read check when running as root ([#&#8203;14884](https://togithub.com/vitejs/vite/issues/14884)) ([1d9516c](https://togithub.com/vitejs/vite/commit/1d9516c)), closes [#&#8203;14884](https://togithub.com/vitejs/vite/issues/14884)
-   perf(hmr): skip traversed modules when checking circular imports ([#&#8203;15034](https://togithub.com/vitejs/vite/issues/15034)) ([41e437f](https://togithub.com/vitejs/vite/commit/41e437f)), closes [#&#8203;15034](https://togithub.com/vitejs/vite/issues/15034)
-   fix: run htmlFallbackMiddleware for no accept header requests ([#&#8203;15025](https://togithub.com/vitejs/vite/issues/15025)) ([b93dfe3](https://togithub.com/vitejs/vite/commit/b93dfe3)), closes [#&#8203;15025](https://togithub.com/vitejs/vite/issues/15025)
-   fix: update type CSSModulesOptions interface ([#&#8203;14987](https://togithub.com/vitejs/vite/issues/14987)) ([d0b2153](https://togithub.com/vitejs/vite/commit/d0b2153)), closes [#&#8203;14987](https://togithub.com/vitejs/vite/issues/14987)
-   fix(legacy): error in build with --watch and manifest enabled ([#&#8203;14450](https://togithub.com/vitejs/vite/issues/14450)) ([b9ee620](https://togithub.com/vitejs/vite/commit/b9ee620)), closes [#&#8203;14450](https://togithub.com/vitejs/vite/issues/14450)
-   chore: add comment about crossorigin attribute for script module ([#&#8203;15040](https://togithub.com/vitejs/vite/issues/15040)) ([03c371e](https://togithub.com/vitejs/vite/commit/03c371e)), closes [#&#8203;15040](https://togithub.com/vitejs/vite/issues/15040)
-   chore: cleanup v5 beta changelog ([#&#8203;14694](https://togithub.com/vitejs/vite/issues/14694)) ([531d3cb](https://togithub.com/vitejs/vite/commit/531d3cb)), closes [#&#8203;14694](https://togithub.com/vitejs/vite/issues/14694)

### [`v5.0.0`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#500-2023-11-16)

### [`v4.5.3`](https://togithub.com/vitejs/vite/compare/v4.5.2...aac695e9f8f29da43c2f7c50c549fa3d3dfeeadc)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.5.2...v4.5.3)

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

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Label `internal` added by @renovate[bot] on 2024-03-26 09:58_

---

_Comment by @renovate[bot] on 2024-03-26 09:58_

### ⚠ Artifact update problem

Renovate failed to update an artifact related to this branch. You probably do not want to merge this PR as-is.

♻ Renovate will retry this branch, including artifacts, only when one of the following happens:

 - any of the package files in this branch needs updating, or 
 - the branch becomes conflicted, or
 - you click the rebase/retry checkbox if found above, or
 - you rename this PR's title to start with "rebase!" to trigger it manually

The artifact failure details are included below:

##### File name: playground/package-lock.json

```
npm ERR! code ERESOLVE
npm ERR! ERESOLVE could not resolve
npm ERR! 
npm ERR! While resolving: @vitejs/plugin-react-swc@3.3.2
npm ERR! Found: vite@5.2.6
npm ERR! node_modules/vite
npm ERR!   dev vite@"^5.0.0" from the root project
npm ERR! 
npm ERR! Could not resolve dependency:
npm ERR! peer vite@"^4" from @vitejs/plugin-react-swc@3.3.2
npm ERR! node_modules/@vitejs/plugin-react-swc
npm ERR!   dev @vitejs/plugin-react-swc@"^3.0.0" from the root project
npm ERR! 
npm ERR! Conflicting peer dependency: vite@4.5.3
npm ERR! node_modules/vite
npm ERR!   peer vite@"^4" from @vitejs/plugin-react-swc@3.3.2
npm ERR!   node_modules/@vitejs/plugin-react-swc
npm ERR!     dev @vitejs/plugin-react-swc@"^3.0.0" from the root project
npm ERR! 
npm ERR! Fix the upstream dependency conflict, or retry
npm ERR! this command with --force or --legacy-peer-deps
npm ERR! to accept an incorrect (and potentially broken) dependency resolution.
npm ERR! 
npm ERR! 
npm ERR! For a full report see:
npm ERR! /tmp/renovate/cache/others/npm/_logs/2024-03-26T09_58_32_567Z-eresolve-report.txt

npm ERR! A complete log of this run can be found in: /tmp/renovate/cache/others/npm/_logs/2024-03-26T09_58_32_567Z-debug-0.log

```



---

_Renamed from "Update dependency vite to v5" to "Update dependency vite to v5 - autoclosed" by @renovate[bot] on 2024-03-26 10:40_

---

_Closed by @renovate[bot] on 2024-03-26 10:40_

---

_Branch deleted on 2024-03-26 10:40_

---
