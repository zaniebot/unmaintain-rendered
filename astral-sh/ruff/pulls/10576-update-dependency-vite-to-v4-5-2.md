```yaml
number: 10576
title: Update dependency vite to v4.5.2
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/npm-vite-vulnerability
created_at: 2024-03-25T17:33:51Z
updated_at: 2024-03-25T22:50:42Z
url: https://github.com/astral-sh/ruff/pull/10576
synced_at: 2026-01-10T22:47:02Z
```

# Update dependency vite to v4.5.2

---

_Pull request opened by @renovate on 2024-03-25 17:33_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [vite](https://togithub.com/vitejs/vite/tree/main/#readme) ([source](https://togithub.com/vitejs/vite/tree/HEAD/packages/vite)) | [`4.4.4` -> `4.5.2`](https://renovatebot.com/diffs/npm/vite/4.4.4/4.5.2) | [![age](https://developer.mend.io/api/mc/badges/age/npm/vite/4.5.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/vite/4.5.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/vite/4.4.4/4.5.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/vite/4.4.4/4.5.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

### GitHub Vulnerability Alerts

#### [CVE-2023-49293](https://togithub.com/vitejs/vite/security/advisories/GHSA-92r3-m2mg-pj97)

### Summary
When Vite's HTML transformation is invoked manually via `server.transformIndexHtml`, the original request URL is passed in unmodified, and the `html` being transformed contains inline module scripts (`<script type="module">...</script>`), it is possible to inject arbitrary HTML into the transformed output by supplying a malicious URL query string to `server.transformIndexHtml`.

### Impact
Only apps using `appType: 'custom'` and using the default Vite HTML middleware are affected. The HTML entry must also contain an inline script. The attack requires a user to click on a malicious URL while running the dev server. Restricted files aren't exposed to the attacker.

### Patches
Fixed in vite@5.0.5, vite@4.5.1, vite@4.4.12

### Details
Suppose `index.html` contains an inline module script:

```html
<script type="module">
  // Inline script
</script>
```

This script is transformed into a proxy script like

```html
<script type="module" src="/index.html?html-proxy&index=0.js"></script>
```

due to Vite's HTML plugin:

https://github.com/vitejs/vite/blob/7fd7c6cebfcad34ae7021ebee28f97b1f28ef3f3/packages/vite/src/node/plugins/html.ts#L429-L465

When `appType: 'spa' | 'mpa'`, Vite serves HTML itself, and `htmlFallbackMiddleware` rewrites `req.url` to the canonical path of `index.html`,

https://github.com/vitejs/vite/blob/73ef074b80fa7252e0c46a37a2c94ba8cba46504/packages/vite/src/node/server/middlewares/htmlFallback.ts#L44-L47

so the `url` passed to `server.transformIndexHtml` is `/index.html`.

However, if `appType: 'custom'`, HTML is served manually, and if `server.transformIndexHtml` is called with the unmodified request URL (as the SSR docs suggest), then the path of the transformed `html-proxy` script varies with the request URL. For example, a request with path `/` produces

```html
<script type="module" src="/@&#8203;id/__x00__/index.html?html-proxy&index=0.js"></script>
```

It is possible to abuse this behavior by crafting a request URL to contain a malicious payload like

```
"></script><script>alert('boom')</script>
```

so a request to http://localhost:5173/?%22%3E%3C/script%3E%3Cscript%3Ealert(%27boom%27)%3C/script%3E produces HTML output like

```html
<script type="module" src="/@&#8203;id/__x00__/?"></script><script>alert("boom")</script>?html-proxy&index=0.js"></script>
```

which demonstrates XSS.

### PoC

- Example 1. Serving HTML from `vite dev` middleware with `appType: 'custom'`
    - Go to https://stackblitz.com/edit/vitejs-vite-9xhma4?file=main.js&terminal=dev-html
    - "Open in New Tab"
    - Edit URL to set query string to `?%22%3E%3C/script%3E%3Cscript%3Ealert(%27boom%27)%3C/script%3E` and navigate
    - Witness XSS:
    - ![image](https://user-images.githubusercontent.com/2456381/287434281-13757894-7a63-4a73-b1e9-d2b024c19d14.png)
- Example 2. Serving HTML from SSR-style Express server (Vite dev server runs in middleware mode):
    - Go to https://stackblitz.com/edit/vitejs-vite-9xhma4?file=main.js&terminal=server
    - (Same steps as above)
- Example 3. Plain `vite dev` (this shows that vanilla `vite dev` is _not_ vulnerable, provided `htmlFallbackMiddleware` is used)
    - Go to https://stackblitz.com/edit/vitejs-vite-9xhma4?file=main.js&terminal=dev
    - (Same steps as above)
    - You should _not_ see the alert box in this case

### Detailed Impact

This will probably predominantly affect [development-mode SSR](https://vitejs.dev/guide/ssr#setting-up-the-dev-server), where `vite.transformHtml` is called using the original `req.url`, per the docs:

https://github.com/vitejs/vite/blob/7fd7c6cebfcad34ae7021ebee28f97b1f28ef3f3/docs/guide/ssr.md?plain=1#L114-L126

However, since this vulnerability affects `server.transformIndexHtml`, the scope of impact may be higher to also include other ad-hoc calls to `server.transformIndexHtml` from outside of Vite's own codebase.

My best guess at bisecting which versions are vulnerable involves the following test script

```js
import fs from 'node:fs/promises';
import * as vite from 'vite';

const html = `
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
  </head>
  <body>
    <script type="module">
      // Inline script
    </script>
  </body>
</html>
`;
const server = await vite.createServer({ appType: 'custom' });
const transformed = await server.transformIndexHtml('/?%22%3E%3C/script%3E%3Cscript%3Ealert(%27boom%27)%3C/script%3E', html);
console.log(transformed);
await server.close();
```

and using it I was able to narrow down to #&#8203;13581. If this is correct, then vulnerable Vite versions are 4.4.0-beta.2 and higher (which includes 4.4.0).

#### [CVE-2024-23331](https://togithub.com/vitejs/vite/security/advisories/GHSA-c24v-8rfc-w8vw)

### Summary
[Vite dev server option](https://vitejs.dev/config/server-options.html#server-fs-deny) `server.fs.deny` can be bypassed on case-insensitive file systems using case-augmented versions of filenames. Notably this affects servers hosted on Windows.

This bypass is similar to https://nvd.nist.gov/vuln/detail/CVE-2023-34092 -- with surface area reduced to hosts having case-insensitive filesystems.

### Patches
Fixed in vite@5.0.12, vite@4.5.2, vite@3.2.8, vite@2.9.17

### Details
Since `picomatch` defaults to case-sensitive glob matching, but the file server doesn't discriminate; a blacklist bypass is possible. 

See `picomatch`  usage, where `nocase` is defaulted to `false`: https://github.com/vitejs/vite/blob/v5.1.0-beta.1/packages/vite/src/node/server/index.ts#L632

By requesting raw filesystem paths using augmented casing, the matcher derived from `config.server.fs.deny` fails to block access to sensitive files. 

### PoC
**Setup**
1. Created vanilla Vite project using `npm create vite@latest` on a Standard Azure hosted Windows 10 instance. 
    - `npm run dev -- --host 0.0.0.0`
    - Publicly accessible for the time being here: http://20.12.242.81:5173/ 
2. Created dummy secret files, e.g. `custom.secret` and `production.pem`
3. Populated `vite.config.js` with
```javascript
export default { server: { fs: { deny: ['.env', '.env.*', '*.{crt,pem}', 'custom.secret'] } } }
```

**Reproduction**
1. `curl -s http://20.12.242.81:5173/@&#8203;fs//`
    - Descriptive error page reveals absolute filesystem path to project root
2. `curl -s http://20.12.242.81:5173/@&#8203;fs/C:/Users/darbonzo/Desktop/vite-project/vite.config.js`
    - Discoverable configuration file reveals locations of secrets
3. `curl -s http://20.12.242.81:5173/@&#8203;fs/C:/Users/darbonzo/Desktop/vite-project/custom.sEcReT`
    - Secrets are directly accessible using case-augmented version of filename

**Proof**
![Screenshot 2024-01-19 022736](https://user-images.githubusercontent.com/907968/298020728-3a8d3c06-fcfd-4009-9182-e842f66a6ea5.png)

### Impact
**Who**
- Users with exposed dev servers on environments with case-insensitive filesystems

**What**
- Files protected by `server.fs.deny` are both discoverable, and accessible

---

### Release Notes

<details>
<summary>vitejs/vite (vite)</summary>

### [`v4.5.2`](https://togithub.com/vitejs/vite/releases/tag/v4.5.2)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.5.1...v4.5.2)

Please refer to [CHANGELOG.md](https://togithub.com/vitejs/vite/blob/v4.5.2/packages/vite/CHANGELOG.md) for details.

### [`v4.5.1`](https://togithub.com/vitejs/vite/releases/tag/v4.5.1)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.5.0...v4.5.1)

Please refer to [CHANGELOG.md](https://togithub.com/vitejs/vite/blob/v4.5.1/packages/vite/CHANGELOG.md) for details.

### [`v4.5.0`](https://togithub.com/vitejs/vite/releases/tag/v4.5.0)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.4.12...v4.5.0)

Please refer to [CHANGELOG.md](https://togithub.com/vitejs/vite/blob/v4.5.0/packages/vite/CHANGELOG.md) for details.

### [`v4.4.12`](https://togithub.com/vitejs/vite/releases/tag/v4.4.12)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.4.11...v4.4.12)

Please refer to [CHANGELOG.md](https://togithub.com/vitejs/vite/blob/v4.4.12/packages/vite/CHANGELOG.md) for details.

### [`v4.4.11`](https://togithub.com/vitejs/vite/releases/tag/v4.4.11)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.4.10...v4.4.11)

Please refer to [CHANGELOG.md](https://togithub.com/vitejs/vite/blob/v4.4.11/packages/vite/CHANGELOG.md) for details.

### [`v4.4.10`](https://togithub.com/vitejs/vite/releases/tag/v4.4.10)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.4.9...v4.4.10)

Please refer to [CHANGELOG.md](https://togithub.com/vitejs/vite/blob/v4.4.10/packages/vite/CHANGELOG.md) for details.

### [`v4.4.9`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small449-2023-08-07-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.4.8...v4.4.9)

-   chore: fix eslint warnings ([#&#8203;14031](https://togithub.com/vitejs/vite/issues/14031)) ([4021a0e](https://togithub.com/vitejs/vite/commit/4021a0e)), closes [#&#8203;14031](https://togithub.com/vitejs/vite/issues/14031)
-   chore(deps): update all non-major dependencies ([#&#8203;13938](https://togithub.com/vitejs/vite/issues/13938)) ([a1b519e](https://togithub.com/vitejs/vite/commit/a1b519e)), closes [#&#8203;13938](https://togithub.com/vitejs/vite/issues/13938)
-   fix: dynamic import vars ignored warning ([#&#8203;14006](https://togithub.com/vitejs/vite/issues/14006)) ([4479431](https://togithub.com/vitejs/vite/commit/4479431)), closes [#&#8203;14006](https://togithub.com/vitejs/vite/issues/14006)
-   fix(build): silence warn dynamic import module when inlineDynamicImports true ([#&#8203;13970](https://togithub.com/vitejs/vite/issues/13970)) ([7a77aaf](https://togithub.com/vitejs/vite/commit/7a77aaf)), closes [#&#8203;13970](https://togithub.com/vitejs/vite/issues/13970)
-   perf: improve build times and memory utilization ([#&#8203;14016](https://togithub.com/vitejs/vite/issues/14016)) ([9d7d45e](https://togithub.com/vitejs/vite/commit/9d7d45e)), closes [#&#8203;14016](https://togithub.com/vitejs/vite/issues/14016)
-   perf: replace startsWith with === ([#&#8203;14005](https://togithub.com/vitejs/vite/issues/14005)) ([f5c1224](https://togithub.com/vitejs/vite/commit/f5c1224)), closes [#&#8203;14005](https://togithub.com/vitejs/vite/issues/14005)

### [`v4.4.8`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small448-2023-07-31-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.4.7...v4.4.8)

-   fix: modulePreload false ([#&#8203;13973](https://togithub.com/vitejs/vite/issues/13973)) ([488085d](https://togithub.com/vitejs/vite/commit/488085d)), closes [#&#8203;13973](https://togithub.com/vitejs/vite/issues/13973)
-   fix: multiple entries with shared css and no JS ([#&#8203;13962](https://togithub.com/vitejs/vite/issues/13962)) ([89a3db0](https://togithub.com/vitejs/vite/commit/89a3db0)), closes [#&#8203;13962](https://togithub.com/vitejs/vite/issues/13962)
-   fix: use file extensions on type imports so they work with `moduleResolution: 'node16'` ([#&#8203;13947](https://togithub.com/vitejs/vite/issues/13947)) ([aeef670](https://togithub.com/vitejs/vite/commit/aeef670)), closes [#&#8203;13947](https://togithub.com/vitejs/vite/issues/13947)
-   fix(css): enhance error message for missing preprocessor dependency ([#&#8203;11485](https://togithub.com/vitejs/vite/issues/11485)) ([65e5c22](https://togithub.com/vitejs/vite/commit/65e5c22)), closes [#&#8203;11485](https://togithub.com/vitejs/vite/issues/11485)
-   fix(esbuild): fix static properties transpile when useDefineForClassFields false ([#&#8203;13992](https://togithub.com/vitejs/vite/issues/13992)) ([4ca7c13](https://togithub.com/vitejs/vite/commit/4ca7c13)), closes [#&#8203;13992](https://togithub.com/vitejs/vite/issues/13992)
-   fix(importAnalysis): strip url base before passing as safeModulePaths ([#&#8203;13712](https://togithub.com/vitejs/vite/issues/13712)) ([1ab06a8](https://togithub.com/vitejs/vite/commit/1ab06a8)), closes [#&#8203;13712](https://togithub.com/vitejs/vite/issues/13712)
-   fix(importMetaGlob): avoid unnecessary hmr of negative glob ([#&#8203;13646](https://togithub.com/vitejs/vite/issues/13646)) ([844451c](https://togithub.com/vitejs/vite/commit/844451c)), closes [#&#8203;13646](https://togithub.com/vitejs/vite/issues/13646)
-   fix(optimizer): avoid double-commit of optimized deps when discovery is disabled ([#&#8203;13865](https://togithub.com/vitejs/vite/issues/13865)) ([df77991](https://togithub.com/vitejs/vite/commit/df77991)), closes [#&#8203;13865](https://togithub.com/vitejs/vite/issues/13865)
-   fix(optimizer): enable experimentalDecorators by default ([#&#8203;13981](https://togithub.com/vitejs/vite/issues/13981)) ([f8a5ffc](https://togithub.com/vitejs/vite/commit/f8a5ffc)), closes [#&#8203;13981](https://togithub.com/vitejs/vite/issues/13981)
-   perf: replace startsWith with === ([#&#8203;13989](https://togithub.com/vitejs/vite/issues/13989)) ([3aab14e](https://togithub.com/vitejs/vite/commit/3aab14e)), closes [#&#8203;13989](https://togithub.com/vitejs/vite/issues/13989)
-   perf: single slash does not need to be replaced ([#&#8203;13980](https://togithub.com/vitejs/vite/issues/13980)) ([66f522c](https://togithub.com/vitejs/vite/commit/66f522c)), closes [#&#8203;13980](https://togithub.com/vitejs/vite/issues/13980)
-   perf: use Intl.DateTimeFormatter instead of toLocaleTimeString ([#&#8203;13951](https://togithub.com/vitejs/vite/issues/13951)) ([af53a1d](https://togithub.com/vitejs/vite/commit/af53a1d)), closes [#&#8203;13951](https://togithub.com/vitejs/vite/issues/13951)
-   perf: use Intl.NumberFormat instead of toLocaleString ([#&#8203;13949](https://togithub.com/vitejs/vite/issues/13949)) ([a48bf88](https://togithub.com/vitejs/vite/commit/a48bf88)), closes [#&#8203;13949](https://togithub.com/vitejs/vite/issues/13949)
-   perf: use magic-string hires boundary for sourcemaps ([#&#8203;13971](https://togithub.com/vitejs/vite/issues/13971)) ([b9a8d65](https://togithub.com/vitejs/vite/commit/b9a8d65)), closes [#&#8203;13971](https://togithub.com/vitejs/vite/issues/13971)
-   chore(reporter): remove unnecessary map ([#&#8203;13972](https://togithub.com/vitejs/vite/issues/13972)) ([dd9d4c1](https://togithub.com/vitejs/vite/commit/dd9d4c1)), closes [#&#8203;13972](https://togithub.com/vitejs/vite/issues/13972)
-   refactor: add new overload to the type of defineConfig ([#&#8203;13958](https://togithub.com/vitejs/vite/issues/13958)) ([24c12fe](https://togithub.com/vitejs/vite/commit/24c12fe)), closes [#&#8203;13958](https://togithub.com/vitejs/vite/issues/13958)

### [`v4.4.7`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small447-2023-07-24-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.4.6...v4.4.7)

-   fix: `optimizeDeps.include` not working with paths inside packages ([#&#8203;13922](https://togithub.com/vitejs/vite/issues/13922)) ([06e4f57](https://togithub.com/vitejs/vite/commit/06e4f57)), closes [#&#8203;13922](https://togithub.com/vitejs/vite/issues/13922)
-   fix: lightningcss fails with html-proxy ([#&#8203;13776](https://togithub.com/vitejs/vite/issues/13776)) ([6b56094](https://togithub.com/vitejs/vite/commit/6b56094)), closes [#&#8203;13776](https://togithub.com/vitejs/vite/issues/13776)
-   fix: prepend `config.base` to vite/env path ([#&#8203;13941](https://togithub.com/vitejs/vite/issues/13941)) ([8e6cee8](https://togithub.com/vitejs/vite/commit/8e6cee8)), closes [#&#8203;13941](https://togithub.com/vitejs/vite/issues/13941)
-   fix(html): support `import.meta.env` define replacement without quotes ([#&#8203;13425](https://togithub.com/vitejs/vite/issues/13425)) ([883089c](https://togithub.com/vitejs/vite/commit/883089c)), closes [#&#8203;13425](https://togithub.com/vitejs/vite/issues/13425)
-   fix(proxy): handle error when proxy itself errors ([#&#8203;13929](https://togithub.com/vitejs/vite/issues/13929)) ([4848e41](https://togithub.com/vitejs/vite/commit/4848e41)), closes [#&#8203;13929](https://togithub.com/vitejs/vite/issues/13929)
-   chore(eslint): allow type annotations ([#&#8203;13920](https://togithub.com/vitejs/vite/issues/13920)) ([d1264fd](https://togithub.com/vitejs/vite/commit/d1264fd)), closes [#&#8203;13920](https://togithub.com/vitejs/vite/issues/13920)

### [`v4.4.6`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small446-2023-07-21-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.4.5...v4.4.6)

-   fix: constrain inject helpers for iife ([#&#8203;13909](https://togithub.com/vitejs/vite/issues/13909)) ([c89f677](https://togithub.com/vitejs/vite/commit/c89f677)), closes [#&#8203;13909](https://togithub.com/vitejs/vite/issues/13909)
-   fix: display manualChunks warning only when a function is not used ([#&#8203;13797](https://togithub.com/vitejs/vite/issues/13797)) ([#&#8203;13798](https://togithub.com/vitejs/vite/issues/13798)) ([51c271f](https://togithub.com/vitejs/vite/commit/51c271f)), closes [#&#8203;13797](https://togithub.com/vitejs/vite/issues/13797) [#&#8203;13798](https://togithub.com/vitejs/vite/issues/13798)
-   fix: do not append `browserHash` on optimized deps during build ([#&#8203;13906](https://togithub.com/vitejs/vite/issues/13906)) ([0fb2340](https://togithub.com/vitejs/vite/commit/0fb2340)), closes [#&#8203;13906](https://togithub.com/vitejs/vite/issues/13906)
-   fix: use Bun's implementation of `ws` instead of the bundled one ([#&#8203;13901](https://togithub.com/vitejs/vite/issues/13901)) ([049404c](https://togithub.com/vitejs/vite/commit/049404c)), closes [#&#8203;13901](https://togithub.com/vitejs/vite/issues/13901)
-   feat(client): add guide to press Esc for closing the overlay ([#&#8203;13896](https://togithub.com/vitejs/vite/issues/13896)) ([da389cc](https://togithub.com/vitejs/vite/commit/da389cc)), closes [#&#8203;13896](https://togithub.com/vitejs/vite/issues/13896)

### [`v4.4.5`](https://togithub.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small445-2023-07-20-small)

[Compare Source](https://togithub.com/vitejs/vite/compare/v4.4.4...v4.4.5)

-   fix: "EISDIR: illegal operation on a directory, realpath" error on RA… ([#&#8203;13655](https://togithub.com/vitejs/vite/issues/13655)) ([6bd5434](https://togithub.com/vitejs/vite/commit/6bd5434)), closes [#&#8203;13655](https://togithub.com/vitejs/vite/issues/13655)
-   fix: transform error message add file info ([#&#8203;13687](https://togithub.com/vitejs/vite/issues/13687)) ([6dca41c](https://togithub.com/vitejs/vite/commit/6dca41c)), closes [#&#8203;13687](https://togithub.com/vitejs/vite/issues/13687)
-   fix: warn when publicDir and outDir are nested ([#&#8203;13742](https://togithub.com/vitejs/vite/issues/13742)) ([4eb3154](https://togithub.com/vitejs/vite/commit/4eb3154)), closes [#&#8203;13742](https://togithub.com/vitejs/vite/issues/13742)
-   fix(build): remove warning about ineffective dynamic import from node_modules ([#&#8203;13884](https://togithub.com/vitejs/vite/issues/13884)) ([33002dd](https://togithub.com/vitejs/vite/commit/33002dd)), closes [#&#8203;13884](https://togithub.com/vitejs/vite/issues/13884)
-   fix(build): style insert order for UMD builds (fix [#&#8203;13668](https://togithub.com/vitejs/vite/issues/13668)) ([#&#8203;13669](https://togithub.com/vitejs/vite/issues/13669)) ([49a1b99](https://togithub.com/vitejs/vite/commit/49a1b99)), closes [#&#8203;13668](https://togithub.com/vitejs/vite/issues/13668) [#&#8203;13669](https://togithub.com/vitejs/vite/issues/13669)
-   fix(deps): update all non-major dependencies ([#&#8203;13872](https://togithub.com/vitejs/vite/issues/13872)) ([975a631](https://togithub.com/vitejs/vite/commit/975a631)), closes [#&#8203;13872](https://togithub.com/vitejs/vite/issues/13872)
-   fix(types): narrow down the return type of `defineConfig` ([#&#8203;13792](https://togithub.com/vitejs/vite/issues/13792)) ([c971f26](https://togithub.com/vitejs/vite/commit/c971f26)), closes [#&#8203;13792](https://togithub.com/vitejs/vite/issues/13792)
-   chore: fix typos ([#&#8203;13862](https://togithub.com/vitejs/vite/issues/13862)) ([f54e8da](https://togithub.com/vitejs/vite/commit/f54e8da)), closes [#&#8203;13862](https://togithub.com/vitejs/vite/issues/13862)
-   chore: replace `any` with `string` ([#&#8203;13850](https://togithub.com/vitejs/vite/issues/13850)) ([4606fd8](https://togithub.com/vitejs/vite/commit/4606fd8)), closes [#&#8203;13850](https://togithub.com/vitejs/vite/issues/13850)
-   chore(deps): update dependency prettier to v3 ([#&#8203;13759](https://togithub.com/vitejs/vite/issues/13759)) ([5a56941](https://togithub.com/vitejs/vite/commit/5a56941)), closes [#&#8203;13759](https://togithub.com/vitejs/vite/issues/13759)
-   docs: fix build.cssMinify link ([#&#8203;13840](https://togithub.com/vitejs/vite/issues/13840)) ([8a2a3e1](https://togithub.com/vitejs/vite/commit/8a2a3e1)), closes [#&#8203;13840](https://togithub.com/vitejs/vite/issues/13840)

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Label `internal` added by @renovate[bot] on 2024-03-25 17:33_

---

_Renamed from "Update dependency vite to v4.5.2 [SECURITY]" to "Update dependency vite to v4.5.2" by @renovate[bot] on 2024-03-25 17:58_

---

_Merged by @zanieb on 2024-03-25 22:50_

---

_Closed by @zanieb on 2024-03-25 22:50_

---

_Branch deleted on 2024-03-25 22:50_

---
