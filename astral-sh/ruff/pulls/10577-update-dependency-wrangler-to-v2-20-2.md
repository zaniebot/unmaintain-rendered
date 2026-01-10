```yaml
number: 10577
title: Update dependency wrangler to v2.20.2
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/npm-wrangler-vulnerability
created_at: 2024-03-25T17:34:20Z
updated_at: 2024-03-25T22:50:24Z
url: https://github.com/astral-sh/ruff/pull/10577
synced_at: 2026-01-10T22:47:02Z
```

# Update dependency wrangler to v2.20.2

---

_Pull request opened by @renovate on 2024-03-25 17:34_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [wrangler](https://togithub.com/cloudflare/workers-sdk) ([source](https://togithub.com/cloudflare/workers-sdk/tree/HEAD/packages/wrangler)) | [`2.0.27` -> `2.20.2`](https://renovatebot.com/diffs/npm/wrangler/2.0.27/2.20.2) | [![age](https://developer.mend.io/api/mc/badges/age/npm/wrangler/2.20.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/wrangler/2.20.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/wrangler/2.0.27/2.20.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/wrangler/2.0.27/2.20.2?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

### GitHub Vulnerability Alerts

#### [CVE-2023-3348](https://togithub.com/cloudflare/workers-sdk/security/advisories/GHSA-8c93-4hch-xgxp)

### Impact 
The Wrangler command line tool (<=wrangler@3.1.0 or <=wrangler@2.20.1) was affected by a directory traversal vulnerability when running a local development server for Pages (wrangler pages dev command). This vulnerability enabled an attacker in the same network as the victim to connect to the local development server and access the victim's files present outside of the directory for the development server.

### Patches
Wrangler2: Upgrade to v2.20.1 or higher.
Wrangler3: Upgrade to v3.1.1 or higher.

### References
[Workers SDK on Github](https://togithub.com/cloudflare/workers-sdk)
[Wrangler docs](https://developers.cloudflare.com/workers/wrangler/)
[CVE-2023-3348](https://www.cve.org/CVERecord?id=CVE-2023-3348)

#### [CVE-2023-7080](https://togithub.com/cloudflare/workers-sdk/security/advisories/GHSA-f8mp-x433-5wpf)

### Impact
The V8 inspector intentionally allows arbitrary code execution within the Workers sandbox for debugging. `wrangler dev` would previously start an inspector server listening on all network interfaces. This would allow an attacker on the local network to connect to the inspector and run arbitrary code. Additionally, the inspector server did not validate `Origin`/`Host` headers, granting an attacker that can trick any user on the local network into opening a malicious website the ability to run code. If `wrangler dev --remote` was being used, an attacker could access production resources if they were bound to the worker.

### Patches
This issue was fixed in `wrangler@3.19.0` and `wrangler@2.20.2`. Whilst `wrangler dev`'s inspector server listens on local interfaces by default as of `wrangler@3.16.0`, an [SSRF vulnerability in `miniflare`](https://togithub.com/cloudflare/workers-sdk/security/advisories/GHSA-fwvg-2739-22v7) allowed access from the local network until `wrangler@3.18.0`. `wrangler@3.19.0` and `wrangler@2.20.2` introduced validation for the `Origin`/`Host` headers.

### Workarounds
Unfortunately, Wrangler doesn't provide any configuration for which host that inspector server should listen on. Please upgrade to at least `wrangler@3.16.0`, and configure Wrangler to listen on local interfaces instead with `wrangler dev --ip 127.0.0.1` to prevent SSRF. This removes the local network as an attack vector, but does not prevent an attack from visiting a malicious website.

### References
- [https://github.com/cloudflare/workers-sdk/issues/4430](https://togithub.com/cloudflare/workers-sdk/issues/4430)
- [https://github.com/cloudflare/workers-sdk/pull/4437](https://togithub.com/cloudflare/workers-sdk/pull/4437)
- [https://github.com/cloudflare/workers-sdk/pull/4535](https://togithub.com/cloudflare/workers-sdk/pull/4535)
- [https://github.com/cloudflare/workers-sdk/pull/4550](https://togithub.com/cloudflare/workers-sdk/pull/4550)

---

### Release Notes

<details>
<summary>cloudflare/workers-sdk (wrangler)</summary>

### [`v2.20.2`](https://togithub.com/cloudflare/workers-sdk/releases/tag/wrangler%402.20.2)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.20.1...wrangler@2.20.2)

##### Patch Changes

-   [#&#8203;4609](https://togithub.com/cloudflare/workers-sdk/pull/4609) [`c228c912`](https://togithub.com/cloudflare/workers-sdk/commit/c228c9120f42f7e0135fafe406bc71a766e7bba3) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - fix: pin `workerd` to `1.20230404.0`

-   [#&#8203;4587](https://togithub.com/cloudflare/workers-sdk/pull/4587) [`49a46960`](https://togithub.com/cloudflare/workers-sdk/commit/49a469601adaa9eb9e1f2d6de197c1979d5c6c1b) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - Change dev registry and inspector server to listen on 127.0.0.1 instead of all interfaces

-   [#&#8203;4587](https://togithub.com/cloudflare/workers-sdk/pull/4587) [`49a46960`](https://togithub.com/cloudflare/workers-sdk/commit/49a469601adaa9eb9e1f2d6de197c1979d5c6c1b) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - fix: validate `Host` and `Orgin` headers where appropriate

    `Host` and `Origin` headers are now checked when connecting to the inspector proxy. If these don't match what's expected, the request will fail.

### [`v2.20.1`](https://togithub.com/cloudflare/workers-sdk/releases/tag/wrangler%402.20.1)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.20.0...wrangler@2.20.1)

##### Patch Changes

-   [#&#8203;3820](https://togithub.com/cloudflare/workers-sdk/pull/3820) [`546c2319`](https://togithub.com/cloudflare/workers-sdk/commit/546c2319268fc592f069d9c41b5dabdcf84cc94f) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - fix: Prevent `wrangler pages dev` from serving asset files outside of the build output directory

### [`v2.20.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2200)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.19.0...wrangler@2.20.0)

##### Minor Changes

-   [#&#8203;2966](https://togithub.com/cloudflare/workers-sdk/pull/2966) [`e351afcf`](https://togithub.com/cloudflare/workers-sdk/commit/e351afcff4f265f85ff3e4674cc3083eb5cd5027) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - feat: Add support for the undocumented `_worker.js/` directory in Pages

<!---->

-   [#&#8203;3095](https://togithub.com/cloudflare/workers-sdk/pull/3095) [`133c0423`](https://togithub.com/cloudflare/workers-sdk/commit/133c0423ccb4c2b35a1dd26157ce9a24c6a743bb) Thanks [@&#8203;zebp](https://togithub.com/zebp)! - feat: add support for placement in wrangler config

    Allows a `placement` object in the wrangler config with a mode of `off` or `smart` to configure [Smart placement](https://developers.cloudflare.com/workers/platform/smart-placement/). Enabling Smart Placement can be done in your `wrangler.toml` like:

    ```toml
    [placement]
    mode = "smart"
    ```

<!---->

-   [#&#8203;3140](https://togithub.com/cloudflare/workers-sdk/pull/3140) [`5fd080c8`](https://togithub.com/cloudflare/workers-sdk/commit/5fd080c88ee7991cde107f8723f06ea2fd2c651d) Thanks [@&#8203;penalosa](https://togithub.com/penalosa)! - feat: Support sourcemaps in DevTools

    Intercept requests from DevTools in Wrangler to inject sourcemaps and enable folders in the Sources Panel of DevTools. When errors are thrown in your Worker, DevTools should now show your source file in the Sources panel, rather than Wrangler's bundled output.

##### Patch Changes

-   [#&#8203;2912](https://togithub.com/cloudflare/workers-sdk/pull/2912) [`5079f476`](https://togithub.com/cloudflare/workers-sdk/commit/5079f4767f862cb7c42f4b2b5484b0391fbe5fae) Thanks [@&#8203;petebacondarwin](https://togithub.com/petebacondarwin)! - fix: do not render "value of stdout.lastframe() is undefined" if the output is an empty string

    Fixes [#&#8203;2907](https://togithub.com/cloudflare/workers-sdk/issues/2907)

<!---->

-   [#&#8203;3133](https://togithub.com/cloudflare/workers-sdk/pull/3133) [`d0788008`](https://togithub.com/cloudflare/workers-sdk/commit/d078800804899c3c8e083260f8cfdfc0397d6110) Thanks [@&#8203;dario-piotrowicz](https://togithub.com/dario-piotrowicz)! - fix pages building not taking into account the nodejs_compat flag (and improve the related error message)

<!---->

-   [#&#8203;3146](https://togithub.com/cloudflare/workers-sdk/pull/3146) [`5b234cfd`](https://togithub.com/cloudflare/workers-sdk/commit/5b234cfd554aff08d065b96d7d49dfb36f40caa3) Thanks [@&#8203;jspspike](https://togithub.com/jspspike)! - Added output for tail being in "sampling mode"

### [`v2.19.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2190)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.18.0...wrangler@2.19.0)

##### Minor Changes

-   [#&#8203;3091](https://togithub.com/cloudflare/workers-sdk/pull/3091) [`c32f514c`](https://togithub.com/cloudflare/workers-sdk/commit/c32f514ca40e8b13dc9e86fdc76577b9adeb70f5) Thanks [@&#8203;edevil](https://togithub.com/edevil)! - Added initial commands for integrating with Constellation AI.

### [`v2.18.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2180)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.17.0...wrangler@2.18.0)

##### Minor Changes

-   [#&#8203;3098](https://togithub.com/cloudflare/workers-sdk/pull/3098) [`8818f551`](https://togithub.com/cloudflare/workers-sdk/commit/8818f5516ca909cc941deb953b6359030a8c0301) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - fix: improve Workers Sites asset upload reliability

    -   Wrangler no longer buffers all assets into memory before uploading. This should prevent out-of-memory errors when publishing sites with many large files.
    -   Wrangler now limits the number of in-flight asset upload requests to 5, fixing the `Too many bulk operations already in progress` error.
    -   Wrangler now correctly logs upload progress. Previously, the reported percentage was per upload request group, not across all assets.
    -   Wrangler no longer logs all assets to the console by default. Instead, it will just log the first 100. The rest can be shown by setting the `WRANGLER_LOG=debug` environment variable. A splash of colour has also been added.

### [`v2.17.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2170)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.16.0...wrangler@2.17.0)

##### Minor Changes

-   [#&#8203;3004](https://togithub.com/cloudflare/workers-sdk/pull/3004) [`6d5000a7`](https://togithub.com/cloudflare/workers-sdk/commit/6d5000a7b80b29eb57139c6334f40c564c9ad0c9) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - feat: teach `wrangler docs` to use algolia search index

    This PR lets you search Cloudflare's entire docs via `wrangler docs [search term here]`.

    By default, if the search fails to find what you're looking for, you'll get an error like this:

        ✘ [ERROR] Could not find docs for: <search term goes here>. Please try again with another search term.

    If you provide the `--yes` or `-y` flag, wrangler will open the docs to https://developers.cloudflare.com/workers/wrangler/commands/, even if the search fails.

### [`v2.16.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2160)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.15.1...wrangler@2.16.0)

##### Minor Changes

-   [#&#8203;3058](https://togithub.com/cloudflare/workers-sdk/pull/3058) [`1bd50f56`](https://togithub.com/cloudflare/workers-sdk/commit/1bd50f56a7215bb9a9480a8e8560862acef9e326) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - chore: upgrade `miniflare@3` to [`3.0.0-next.13`](https://togithub.com/cloudflare/miniflare/releases/tag/v3.0.0-next.13)

    Notably, this adds native support for Windows to `wrangler dev --experimental-local`, logging for incoming requests, and support for a bunch of newer R2 features.

##### Patch Changes

-   [#&#8203;3058](https://togithub.com/cloudflare/workers-sdk/pull/3058) [`1bd50f56`](https://togithub.com/cloudflare/workers-sdk/commit/1bd50f56a7215bb9a9480a8e8560862acef9e326) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - fix: disable persistence without `--persist` in `--experimental-local`

    This ensures `--experimental-local` doesn't persist data on the file-system, unless the `--persist` flag is set.
    Data is still always persisted between reloads.

<!---->

-   [#&#8203;3055](https://togithub.com/cloudflare/workers-sdk/pull/3055) [`5f48c405`](https://togithub.com/cloudflare/workers-sdk/commit/5f48c405c663de0c6b2bfc27005246f1fdec6987) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: Teach D1 commands to read auth configuration from wrangler.toml

    This PR fixes a bug in how D1 handles a user's accounts. We've updated the D1 commands to read from config (typically via wrangler.toml) before trying to run commands. This means if an `account_id` is defined in config, we'll use that instead of erroring out when there are multiple accounts to pick from.

    Fixes [#&#8203;3046](https://togithub.com/cloudflare/workers-sdk/issues/3046)

<!---->

-   [#&#8203;3058](https://togithub.com/cloudflare/workers-sdk/pull/3058) [`1bd50f56`](https://togithub.com/cloudflare/workers-sdk/commit/1bd50f56a7215bb9a9480a8e8560862acef9e326) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - fix: disable route validation when using `--experimental-local`

    This ensures `wrangler dev --experimental-local` doesn't require a login or an internet connection if a `route` is configured.

### [`v2.15.1`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2151)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.15.0...wrangler@2.15.1)

##### Patch Changes

-   [#&#8203;2783](https://togithub.com/cloudflare/workers-sdk/pull/2783) [`4c55baf9`](https://togithub.com/cloudflare/workers-sdk/commit/4c55baf9cd0e3d8915272471476017e0d379a988) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - feat: Add `**/*.wasm?module` as default module rule (alias of `**/*.wasm`)

<!---->

-   [#&#8203;2989](https://togithub.com/cloudflare/workers-sdk/pull/2989) [`86e942bb`](https://togithub.com/cloudflare/workers-sdk/commit/86e942bbb943750ee57e209a214e08926fb32ac5) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - fix: Durable Object proxying websockets over local dev registry

### [`v2.15.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2150)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.14.0...wrangler@2.15.0)

##### Minor Changes

-   [#&#8203;2769](https://togithub.com/cloudflare/workers-sdk/pull/2769) [`0a779904`](https://togithub.com/cloudflare/workers-sdk/commit/0a77990457652af36c60c52bf9c38c3a69945de4) Thanks [@&#8203;penalosa](https://togithub.com/penalosa)! - feature: Support modules with `--no-bundle`

    When the `--no-bundle` flag is set, Wrangler now has support for uploading additional modules alongside the entrypoint. This will allow modules to be imported at runtime on Cloudflare's Edge. This respects Wrangler's [module rules](https://developers.cloudflare.com/workers/wrangler/configuration/#bundling) configuration, which means that only imports of non-JS modules will trigger an upload by default. For instance, the following code will now work with `--no-bundle` (assuming the `example.wasm` file exists at the correct path):

    ```js
    // index.js
    import wasm from './example.wasm'

    export default {
      async fetch() {
        await WebAssembly.instantiate(wasm, ...)
        ...
      }
    }
    ```

    For JS modules, it's necessary to specify an additional [module rule](https://developers.cloudflare.com/workers/wrangler/configuration/#bundling) (or rules) in your `wrangler.toml` to configure your modules as ES modules or Common JS modules. For instance, to upload additional JavaScript files as ES modules, add the following module rule to your `wrangler.toml`, which tells Wrangler that all `**/*.js` files are ES modules.

    ```toml
    rules = [
      { type = "ESModule", globs = ["**/*.js"]},
    ]
    ```

    If you have Common JS modules, you'd configure Wrangler with a CommonJS rule (the following rule tells Wrangler that all `.cjs` files are Common JS modules):

    ```toml
    rules = [
      { type = "CommonJS", globs = ["**/*.cjs"]},
    ]
    ```

    In most projects, adding a single rule will be sufficient. However, for advanced usecases where you're mixing ES modules and Common JS modules, you'll need to use multiple rule definitions. For instance, the following set of rules will match all `.mjs` files as ES modules, all `.cjs` files as Common JS modules, and the `nested/say-hello.js` file as Common JS.

    ```toml
    rules = [
      { type = "CommonJS", globs = ["nested/say-hello.js", "**/*.cjs"]},
      { type = "ESModule", globs = ["**/*.mjs"]}
    ]
    ```

    If multiple rules overlap, Wrangler will log a warning about the duplicate rules, and will discard additional rules that matches a module. For example, the following rule configuration classifies `dep.js` as both a Common JS module and an ES module:

    ```toml
    rules = [
      { type = "CommonJS", globs = ["dep.js"]},
      { type = "ESModule", globs = ["dep.js"]}
    ]
    ```

    Wrangler will treat `dep.js` as a Common JS module, since that was the first rule that matched, and will log the following warning:

        ▲ [WARNING] Ignoring duplicate module: dep.js (esm)

    This also adds a new configuration option to `wrangler.toml`: `base_dir`. Defaulting to the directory of your Worker's main entrypoint, this tells Wrangler where your additional modules are located, and determines the module paths against which your module rule globs are matched.

    For instance, given the following directory structure:

        - wrangler.toml
        - src/
          - index.html
          - vendor/
            - dependency.js
          - js/
            - index.js

    If your `wrangler.toml` had `main = "src/js/index.js"`, you would need to set `base_dir = "src"` in order to be able to import `src/vendor/dependency.js` and `src/index.html` from `src/js/index.js`.

##### Patch Changes

-   [#&#8203;2957](https://togithub.com/cloudflare/workers-sdk/pull/2957) [`084b2c58`](https://togithub.com/cloudflare/workers-sdk/commit/084b2c58ba051811afe4adf1518cab033ba62872) Thanks [@&#8203;esimons](https://togithub.com/esimons)! - fix: Respect querystring params when calling `.fetch` on a worker instantiated with `unstable_dev`

    Previously, querystring params would be stripped, causing issues for test cases that depended on them. For example, given the following worker script:

    ```js
    export default {
    	fetch(req) {
    		const url = new URL(req.url);
    		const name = url.searchParams.get("name");
    		return new Response("Hello, " + name);
    	},
    };
    ```

    would fail the following test case:

    ```js
    const worker = await unstable_dev("script.js");
    const res = await worker.fetch("http://worker?name=Walshy");
    const text = await res.text();
    // Following fails, as returned text is 'Hello, null'
    expect(text).toBe("Hello, Walshy");
    ```

<!---->

-   [#&#8203;2840](https://togithub.com/cloudflare/workers-sdk/pull/2840) [`e311bbbf`](https://togithub.com/cloudflare/workers-sdk/commit/e311bbbf64343badd4bba7eb017b796a89eaf9fe) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - fix: make `WRANGLER_LOG` case-insensitive, warn on unexpected values, and fallback to `log` if invalid

    Previously, levels set via the `WRANGLER_LOG` environment-variable were case-sensitive.
    If an unexpected level was set, Wrangler would fallback to `none`, hiding all logs.
    The fallback has now been switched to `log`, and lenient case-insensitive matching is used when setting the level.

<!---->

-   [#&#8203;2044](https://togithub.com/cloudflare/workers-sdk/pull/2044) [`eebad0d9`](https://togithub.com/cloudflare/workers-sdk/commit/eebad0d9e593237b4db61047c94e2ec5b47a7b3c) Thanks [@&#8203;kuba-orlik](https://togithub.com/kuba-orlik)! - fix: allow programmatic dev workers to be stopped and started in a single session

<!---->

-   [#&#8203;2735](https://togithub.com/cloudflare/workers-sdk/pull/2735) [`3f7a75cc`](https://togithub.com/cloudflare/workers-sdk/commit/3f7a75ccc252567be3e9062ff0c6fd7e00201d0e) Thanks [@&#8203;JacobMGEvans](https://togithub.com/JacobMGEvans)! - Fix: Generate Remote URL
    Previous URL was pointing to the old cloudflare/templates repo,
    updated the URL to point to templates in the workers-sdk monorepo.

### [`v2.14.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2140)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.13.0...wrangler@2.14.0)

##### Minor Changes

-   [#&#8203;2942](https://togithub.com/cloudflare/workers-sdk/pull/2942) [`dc1465ea`](https://togithub.com/cloudflare/workers-sdk/commit/dc1465ea64acf3fc9c1442e7df73f14df7dc8630) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - chore: upgrade `miniflare` to [`2.13.0`](https://togithub.com/cloudflare/miniflare/releases/tag/v2.13.0)

<!---->

-   [#&#8203;2914](https://togithub.com/cloudflare/workers-sdk/pull/2914) [`9af1a640`](https://togithub.com/cloudflare/workers-sdk/commit/9af1a640237ab26e6332e73e3656d16ca9a96e64) Thanks [@&#8203;edevil](https://togithub.com/edevil)! - feat: add support for send email bindings

    Support send email bindings in order to send emails from a worker. There
    are three types of bindings:

    -   Unrestricted: can send email to any verified destination address.
    -   Restricted: can only send email to the supplied destination address (which
        does not need to be specified when sending the email but also needs to be a
        verified destination address).
    -   Allowlist: can only send email to the supplied list of verified destination
        addresses.

##### Patch Changes

-   [#&#8203;2931](https://togithub.com/cloudflare/workers-sdk/pull/2931) [`5f6c4c0c`](https://togithub.com/cloudflare/workers-sdk/commit/5f6c4c0c4542ada3552e1bf099ecdda677c08a3d) Thanks [@&#8203;Skye-31](https://togithub.com/Skye-31)! - Fix: Pages Dev incorrectly allowing people to turn off local mode

    Local mode is not currently supported in Pages Dev, and errors when people attempt to use it. Previously, wrangler hid the "toggle local mode" button when using Pages dev, but this got broken somewhere along the line.

### [`v2.13.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2130)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.12.3...wrangler@2.13.0)

##### Minor Changes

-   [#&#8203;2905](https://togithub.com/cloudflare/workers-sdk/pull/2905) [`6fd06241`](https://togithub.com/cloudflare/workers-sdk/commit/6fd062419c6b66c2c5beb7c45ec84a32dfa89e01) Thanks [@&#8203;edevil](https://togithub.com/edevil)! - feat: support external imports from `cloudflare:...` prefixed modules

    Going forward Workers will be providing built-in modules (similar to `node:...`) that can be imported using the `cloudflare:...` prefix. This change adds support to the Wrangler bundler to mark these imports as external.

<!---->

-   [#&#8203;2607](https://togithub.com/cloudflare/workers-sdk/pull/2607) [`163dccf4`](https://togithub.com/cloudflare/workers-sdk/commit/163dccf41453e1790fe8e5231d8c1cb8b6ef5a18) Thanks [@&#8203;jspspike@gmail.com](https://togithub.com/jspspike@gmail.com)! - feature: add `wrangler deployment view` and `wrangler rollback` subcommands

`wrangler deployments view [deployment-id]` will get the details of a deployment, including bindings and usage model information.
This information can be used to help debug bad deployments.

`wrangler rollback [deployment-id]` will rollback to a specific deployment in the runtime. This will be useful in situations like recovering from a bad
deployment quickly while resolving issues. If a deployment id is not specified wrangler will rollback to the previous deployment. This rollback only changes the code in the runtime and doesn't affect any code or configurations
in a developer's local setup.

`wrangler deployments list` will list the 10 most recent deployments. This command originally existed as `wrangler deployments`

example of `view <deployment-id>` output:

### [`v2.12.3`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2123)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.12.2...wrangler@2.12.3)

##### Patch Changes

-   [#&#8203;2884](https://togithub.com/cloudflare/workers-sdk/pull/2884) [`e33bea9b`](https://togithub.com/cloudflare/workers-sdk/commit/e33bea9b2b3060c47bd7d453fdbb31889c52c45e) Thanks [@&#8203;WalshyDev](https://togithub.com/WalshyDev)! - Changed console.debug for logger.debug in Pages uploading. This will ensure the debug logs are only sent when debug logging is enabled with `WRANGLER_LOG=debug`.

<!---->

-   [#&#8203;2878](https://togithub.com/cloudflare/workers-sdk/pull/2878) [`6ebb23d5`](https://togithub.com/cloudflare/workers-sdk/commit/6ebb23d5b832a49a95b3a169ccf032a6260f22ef) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - feat: Add D1 binding support to Email Workers

    This PR makes it possible to query D1 from an Email Worker, assuming a binding has been setup.

    As D1 is in alpha and not considered "production-ready", this changeset is a patch, rather than a minor bump to wrangler.

### [`v2.12.2`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2122)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.12.1...wrangler@2.12.2)

##### Patch Changes

-   [#&#8203;2873](https://togithub.com/cloudflare/workers-sdk/pull/2873) [`5bcc333d`](https://togithub.com/cloudflare/workers-sdk/commit/5bcc333d2be77751c6e362ff3365c90ad60a0928) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - fix: Prevent compile loop when using `_worker.js` and `wrangler pages dev`

### [`v2.12.1`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2121)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.12.0...wrangler@2.12.1)

##### Patch Changes

-   [#&#8203;2839](https://togithub.com/cloudflare/workers-sdk/pull/2839) [`ad4b123b`](https://togithub.com/cloudflare/workers-sdk/commit/ad4b123bb9fee51c0cca442e4d0ee6ebeeb020b1) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - fix: remove `vitest` from `wrangler init`'s generated `tsconfig.json` `types` array

    Previously, `wrangler init` generated a `tsconfig.json` with `"types": ["@&#8203;cloudflare/workers-types", "vitest"]`, even if Vitest tests weren't generated.
    Unlike Jest, Vitest [doesn't provide global APIs by default](https://vitest.dev/config/#globals), so there's no need for ambient types.

<!---->

-   [#&#8203;2806](https://togithub.com/cloudflare/workers-sdk/pull/2806) [`8d462c0c`](https://togithub.com/cloudflare/workers-sdk/commit/8d462c0c6fb92dc503747d66565f7bb10bb937d0) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - chore: Remove the `--experimental-worker-bundle` option from Pages Functions

<!---->

-   [#&#8203;2845](https://togithub.com/cloudflare/workers-sdk/pull/2845) [`e3c036d7`](https://togithub.com/cloudflare/workers-sdk/commit/e3c036d773ddc1e4498b016818a52a073909cf36) Thanks [@&#8203;Cyb3r-Jak3](https://togithub.com/Cyb3r-Jak3)! - feature: include .wrangler directory in gitignore template used with `wrangler init`

<!---->

-   [#&#8203;2806](https://togithub.com/cloudflare/workers-sdk/pull/2806) [`8d462c0c`](https://togithub.com/cloudflare/workers-sdk/commit/8d462c0c6fb92dc503747d66565f7bb10bb937d0) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - feat: Add `--outdir` as an option when running `wrangler pages functions build`.

    This deprecates `--outfile` when building a Pages Plugin with `--plugin`.

    When building functions normally, `--outdir` may be used to produce a human-inspectable format of the `_worker.bundle` that is produced.

<!---->

-   [#&#8203;2806](https://togithub.com/cloudflare/workers-sdk/pull/2806) [`8d462c0c`](https://togithub.com/cloudflare/workers-sdk/commit/8d462c0c6fb92dc503747d66565f7bb10bb937d0) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - Enable bundling in Pages Functions by default.

    We now enable bundling by default for a `functions/` folder and for an `_worker.js` in Pages Functions. This allows you to use external modules such as Wasm. You can disable this behavior in Direct Upload projects by using the `--no-bundle` argument in `wrangler pages publish` and `wrangler pages dev`.

<!---->

-   [#&#8203;2836](https://togithub.com/cloudflare/workers-sdk/pull/2836) [`42fb97e5`](https://togithub.com/cloudflare/workers-sdk/commit/42fb97e5de4ed323a706c432b4ff9f73a2a8abbb) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - fix: preserve the entrypoint filename when running `wrangler publish --outdir <dir>`.

    Previously, this entrypoint filename would sometimes be overwritten with some internal filenames. It should now be based off of the entrypoint you provide for your Worker.

<!---->

-   [#&#8203;2828](https://togithub.com/cloudflare/workers-sdk/pull/2828) [`891ddf19`](https://togithub.com/cloudflare/workers-sdk/commit/891ddf19f4f9d268c52fb236f2195a7ff919601e) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - fix: Bring `pages dev` logging in line with `dev` proper's

    `wrangler pages dev` now defaults to logging at the `log` level (rather than the previous `warn` level), and the `pages` prefix is removed.

<!---->

-   [#&#8203;2855](https://togithub.com/cloudflare/workers-sdk/pull/2855) [`226e63fa`](https://togithub.com/cloudflare/workers-sdk/commit/226e63fa3dc24153fb950fc9fb98d040ede30a13) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - fix: `--experimental-local` with `wrangler pages dev`

    We previously had a bug which logged an error (`local worker: TypeError: generateASSETSBinding2 is not a function`). This has now been fixed.

<!---->

-   [#&#8203;2831](https://togithub.com/cloudflare/workers-sdk/pull/2831) [`2b641765`](https://togithub.com/cloudflare/workers-sdk/commit/2b641765975d98e6e04342533fa088f51a96acab) Thanks [@&#8203;Skye-31](https://togithub.com/Skye-31)! - Fix: Show correct link for how to create an API token in a non-interactive environment

### [`v2.12.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2120)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.11.1...wrangler@2.12.0)

##### Minor Changes

-   [#&#8203;2810](https://togithub.com/cloudflare/workers-sdk/pull/2810) [`62784131`](https://togithub.com/cloudflare/workers-sdk/commit/62784131385d641c3512b09565d801a5ecd39725) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - chore: upgrade `@miniflare/tre` to [`3.0.0-next.12`](https://togithub.com/cloudflare/miniflare/releases/tag/v3.0.0-next.12), incorporating changes from [`3.0.0-next.11`](https://togithub.com/cloudflare/miniflare/releases/tag/v3.0.0-next.11)

    Notably, this brings the following improvements to `wrangler dev --experimental-local`:

    -   Adds support for Durable Objects and D1
    -   Fixes an issue blocking clean exits and script reloads
    -   Bumps to `better-sqlite3@&#8203;8`, allowing installation on Node 19

<!---->

-   [#&#8203;2665](https://togithub.com/cloudflare/workers-sdk/pull/2665) [`4756d6a1`](https://togithub.com/cloudflare/workers-sdk/commit/4756d6a143168da84548203b7b0bc23db0b92c95) Thanks [@&#8203;alankemp](https://togithub.com/alankemp)! - Add new \[unsafe.metadata] section to wrangler.toml allowing arbitary data to be added to the metadata section of the upload

##### Patch Changes

-   [#&#8203;2539](https://togithub.com/cloudflare/workers-sdk/pull/2539) [`3725086c`](https://togithub.com/cloudflare/workers-sdk/commit/3725086c4b6854f6fb1e7e0517d3a526d0f9567b) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - feat: Add support for the `nodejs_compat` Compatibility Flag when bundling a Worker with Wrangler

    This new Compatibility Flag is incompatible with the legacy `--node-compat` CLI argument and `node_compat` configuration option. If you want to use the new runtime Node.js compatibility features, please remove the `--node-compat` argument from your CLI command or your config file.

### [`v2.11.1`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2111)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.11.0...wrangler@2.11.1)

##### Patch Changes

-   [#&#8203;2795](https://togithub.com/cloudflare/workers-sdk/pull/2795) [`ec3e181e`](https://togithub.com/cloudflare/workers-sdk/commit/ec3e181eb91534ff79198847f7bf01a606fe2b4a) Thanks [@&#8203;penalosa](https://togithub.com/penalosa)! - fix: Adds a `duplex: "half"` property to R2 fetch requests with stream bodies in order to be compatible with undici v5.20

<!---->

-   [#&#8203;2789](https://togithub.com/cloudflare/workers-sdk/pull/2789) [`4ca8c0b0`](https://togithub.com/cloudflare/workers-sdk/commit/4ca8c0b02878ec259c6d48a47a2aa6e47f59bea0) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - fix: Support overriding the next URL in subsequent executions with `next("/new-path")` from within a Pages Plugin

### [`v2.11.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2110)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.10.0...wrangler@2.11.0)

##### Minor Changes

-   [#&#8203;2651](https://togithub.com/cloudflare/workers-sdk/pull/2651) [`a9adfbe7`](https://togithub.com/cloudflare/workers-sdk/commit/a9adfbe7ea32d4a7c8121d7b34ce9ed51c826033) Thanks [@&#8203;penalosa](https://togithub.com/penalosa)! - Previously, wrangler dev would not work if the root of your zone wasn't behind Cloudflare. This behaviour has changed so that now only the route which your Worker is exposed on has to be behind Cloudflare.

<!---->

-   [#&#8203;2708](https://togithub.com/cloudflare/workers-sdk/pull/2708) [`b3346cfb`](https://togithub.com/cloudflare/workers-sdk/commit/b3346cfbecb2c20f7cce3c3bf8a585b7fd8811aa) Thanks [@&#8203;Skye-31](https://togithub.com/Skye-31)! - Feat: Pages now supports Proxying (200 status) redirects in it's \_redirects file

    This will look something like the following, where a request to /users/123 will appear as that in the browser, but will internally go to /users/\[id].html.

        /users/:id /users/[id] 200

##### Patch Changes

-   [#&#8203;2766](https://togithub.com/cloudflare/workers-sdk/pull/2766) [`7912e63a`](https://togithub.com/cloudflare/workers-sdk/commit/7912e63aa15affc6e3d4a8cfe5dd54348c6e79ba) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - fix: correctly detect `service-worker` format when using `typeof module`

    Wrangler automatically detects whether your code is a `modules` or `service-worker` format Worker based on the presence of a `default` `export`. This check currently works by building your entrypoint with `esbuild` and looking at the output metafile.

    Previously, we were passing `format: "esm"` to `esbuild` when performing this check, which enables *format conversion*. This may introduce `export default` into the built code, even if it wasn't there to start with, resulting in incorrect format detections.

    This change removes `format: "esm"` which disables format conversion when bundling is disabled. See https://esbuild.github.io/api/#format for more details.

<!---->

-   [#&#8203;2780](https://togithub.com/cloudflare/workers-sdk/pull/2780) [`80f1187a`](https://togithub.com/cloudflare/workers-sdk/commit/80f1187a4f90764de53d7488d4e13ea73b748ced) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - fix: Ensure we don't mangle internal constructor names in the wrangler bundle when building with esbuild

    Undici changed how they referenced `FormData`, which meant that when used in our bundle process, we were failing to upload `multipart/form-data` bodies. This affected `wrangler pages publish` and `wrangler publish`.

<!---->

-   [#&#8203;2720](https://togithub.com/cloudflare/workers-sdk/pull/2720) [`de0cb57a`](https://togithub.com/cloudflare/workers-sdk/commit/de0cb57a7a537e6a8554621451b3fd76ffb2e1d1) Thanks [@&#8203;JacobMGEvans](https://togithub.com/JacobMGEvans)! - Fix: Upgraded to ES2022 for improved compatibility
    Upgraded worker code target version from ES2020 to ES2022 for better compatibility and unblocking of a workaround related to [issue #&#8203;2029](https://togithub.com/cloudflare/workers-sdk/issues/2029). The worker runtime now uses the same V8 version as recent Chrome and is 99% ES2016+ compliant. The only thing we don't support on the Workers runtime, the remaining 1%, is the ES2022 RegEx feature as seen in the compat table for the latest Chrome version.

    Compatibility table: https://kangax.github.io/compat-table/es2016plus/

    [resolves #&#8203;2716](https://togithub.com/cloudflare/workers-sdk/issues/2716)

<!---->

-   [#&#8203;2771](https://togithub.com/cloudflare/workers-sdk/pull/2771) [`4ede044e`](https://togithub.com/cloudflare/workers-sdk/commit/4ede044e9247fdc689cbe537dcc5afbda71cf99c) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - chore: upgrade `miniflare` to [`2.12.1`](https://togithub.com/cloudflare/miniflare/releases/tag/v2.12.1) and `@miniflare/tre` to [`3.0.0-next.10`](https://togithub.com/cloudflare/miniflare/releases/tag/v3.0.0-next.10)

### [`v2.10.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#2100)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.9.1...wrangler@2.10.0)

##### Minor Changes

-   [#&#8203;2567](https://togithub.com/cloudflare/workers-sdk/pull/2567) [`02ea098e`](https://togithub.com/cloudflare/workers-sdk/commit/02ea098ed2f0af58dc287a8f37819c96279235eb) Thanks [@&#8203;matthewdavidrodgers](https://togithub.com/matthewdavidrodgers)! - Add mtls-certificate commands and binding support

    Functionality implemented first as an api, which is used in the cli standard
    api commands

    Note that this adds a new OAuth scope, so OAuth users will need to log out and
    log back in to use the new 'mtls-certificate' commands
    However, publishing with mtls-certifcate bindings (bindings of type
    'mtls_certificate') will work without the scope.

<!---->

-   [#&#8203;2717](https://togithub.com/cloudflare/workers-sdk/pull/2717) [`c5943c9f`](https://togithub.com/cloudflare/workers-sdk/commit/c5943c9fe54e8bcf9ee1bf8ca992d2f8b84360a1) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - Upgrade `miniflare` to [`2.12.0`](https://togithub.com/cloudflare/miniflare/releases/tag/v2.12.0), including support for R2 multipart upload bindings, the `nodejs_compat` compatibility flag, D1 fixes and more!

<!---->

-   [#&#8203;2579](https://togithub.com/cloudflare/workers-sdk/pull/2579) [`bf558bdc`](https://togithub.com/cloudflare/workers-sdk/commit/bf558bdc6133acdffbfb08f6b8bd053bf1f25113) Thanks [@&#8203;JacobMGEvans](https://togithub.com/JacobMGEvans)! - Added additional fields to the output of `wrangler deployments` command. The additional fields are from the new value in the response `annotations` which includes `workers/triggered_by` and `rollback_from`

    Example:

        Deployment ID: Galaxy-Class
        Created on:    2021-01-04T00:00:00.000000Z

        Trigger:       Upload from Wrangler 🤠
        Rollback from: MOCK-DEPLOYMENT-ID-2222

##### Patch Changes

-   [#&#8203;2624](https://togithub.com/cloudflare/workers-sdk/pull/2624) [`882bf592`](https://togithub.com/cloudflare/workers-sdk/commit/882bf592fa3a16a8a020808f70ef936bc7f87209) Thanks [@&#8203;CarmenPopoviciu](https://togithub.com/CarmenPopoviciu)! - Add wasm support in `wrangler pages publish`

    Currently it is not possible to import `wasm` modules in either Pages
    Functions or Pages Advanced Mode projects.

    This commit caries out work to address the aforementioned issue by
    enabling `wasm` module imports in `wrangler pages publish`. As a result,
    Pages users can now import their `wasm` modules withing their Functions
    or `_worker.js` files, and `wrangler pages publish` will correctly
    bundle everything and serve these "external" modules.

<!---->

-   [#&#8203;2683](https://togithub.com/cloudflare/workers-sdk/pull/2683) [`68a2a19e`](https://togithub.com/cloudflare/workers-sdk/commit/68a2a19ec962aeeb34059e1e98d088e021048739) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - Fix internal middleware system to allow D1 databases and `--test-scheduled` to be used together

<!---->

-   [#&#8203;2609](https://togithub.com/cloudflare/workers-sdk/pull/2609) [`58ac8a78`](https://togithub.com/cloudflare/workers-sdk/commit/58ac8a783b95c2c884781e5c8af675fe8036644b) Thanks [@&#8203;dario-piotrowicz](https://togithub.com/dario-piotrowicz)! - fix: make sure that the pages publish --no-bundle flag is correctly recognized

<!---->

-   [#&#8203;2696](https://togithub.com/cloudflare/workers-sdk/pull/2696) [`4bc78470`](https://togithub.com/cloudflare/workers-sdk/commit/4bc784706193e24b7a92a19c0ac76eaa7ddcb1c6) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: don't throw an error when omitting preview_database_id, warn instead

<!---->

-   [#&#8203;2715](https://togithub.com/cloudflare/workers-sdk/pull/2715) [`e33294b0`](https://togithub.com/cloudflare/workers-sdk/commit/e33294b07ced96707cad9d71feb768b4ac435f76) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: make it possible to run SQL against preview databases

<!---->

-   [#&#8203;2685](https://togithub.com/cloudflare/workers-sdk/pull/2685) [`2b1177ad`](https://togithub.com/cloudflare/workers-sdk/commit/2b1177ad524b33a4364bc6bed58c3e0b4c59775e) Thanks [@&#8203;CarmenPopoviciu](https://togithub.com/CarmenPopoviciu)! - You can now import Wasm modules in Pages Functions and Pages Functions Advanced Mode (`_worker.js`).
    This change specifically enables `wasm` module imports in `wrangler pages functions build`.
    As a result, Pages users can now import their `wasm` modules within their Functions or
    `_worker.js` files, and `wrangler pages functions build` will correctly bundle everything
    and output the expected result file.

### [`v2.9.1`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#291)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.9.0...wrangler@2.9.1)

##### Patch Changes

-   [#&#8203;2652](https://togithub.com/cloudflare/wrangler2/pull/2652) [`2efd4537`](https://togithub.com/cloudflare/wrangler2/commit/2efd4537cb141e88fe9a674c2fd093b40a3c9d63) Thanks [@&#8203;mrkldshv](https://togithub.com/mrkldshv)! - fix: change `jest` to `vitest` types in generated TypeScript config

<!---->

-   [#&#8203;2657](https://togithub.com/cloudflare/wrangler2/pull/2657) [`8d21b2ea`](https://togithub.com/cloudflare/wrangler2/commit/8d21b2eae4ca5b3eb96c19cbb5c95b470e69942e) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: remove the need to login when running `d1 migrations list --local`

<!---->

-   [#&#8203;2592](https://togithub.com/cloudflare/wrangler2/pull/2592) [`dd66618b`](https://togithub.com/cloudflare/wrangler2/commit/dd66618b2cc63a89424f471f6153be9518f1f087) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: bump esbuild to 0.16.3 (fixes a bug in esbuild's 0.15.13 release that would cause it to hang, is the latest release before a major breaking change that requires us to refactor)

### [`v2.9.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#290)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.8.1...wrangler@2.9.0)

##### Minor Changes

-   [#&#8203;2629](https://togithub.com/cloudflare/workers-sdk/pull/2629) [`151733e5`](https://togithub.com/cloudflare/workers-sdk/commit/151733e5d98e95f363b548f9959c2baf418eb7b5) Thanks [@&#8203;mrbbot](https://togithub.com/mrbbot)! - Prefer the `workerd` `exports` condition when bundling.

    This can be used to build isomorphic libraries that have different implementations depending on the JavaScript runtime they're running in.
    When bundling, Wrangler will try to load the [`workerd` key](https://runtime-keys.proposal.wintercg.org/#workerd).
    This is the [standard key](https://runtime-keys.proposal.wintercg.org/#workerd) for the Cloudflare Workers runtime.
    Learn more about the [conditional `exports` field here](https://nodejs.org/api/packages.html#conditional-exports).

##### Patch Changes

-   [#&#8203;2409](https://togithub.com/cloudflare/workers-sdk/pull/2409) [`89d78c0a`](https://togithub.com/cloudflare/workers-sdk/commit/89d78c0ac444885298ac052b0b3de23b69fb029b) Thanks [@&#8203;penalosa](https://togithub.com/penalosa)! - Wrangler now supports an `--experimental-json-config` flag which will read your configuration from a `wrangler.json` file, rather than `wrangler.toml`. The format of this file is exactly the same as the `wrangler.toml` configuration file, except that the syntax is `JSONC` (JSON with comments) rather than `TOML`. This is experimental, and is not recommended for production use.

<!---->

-   [#&#8203;2623](https://togithub.com/cloudflare/workers-sdk/pull/2623) [`04d8a312`](https://togithub.com/cloudflare/workers-sdk/commit/04d8a3124fbcf20049b39a96654d7f3c850c032b) Thanks [@&#8203;dario-piotrowicz](https://togithub.com/dario-piotrowicz)! - fix d1 directory not being created when running the `wrangler d1 execute` command with the `--yes`/`-y` flag

<!---->

-   [#&#8203;2608](https://togithub.com/cloudflare/workers-sdk/pull/2608) [`70daffeb`](https://togithub.com/cloudflare/workers-sdk/commit/70daffebab4788322769350ab714959e3254c3b4) Thanks [@&#8203;dario-piotrowicz](https://togithub.com/dario-piotrowicz)! - fix: Add support for D1 databases when bundling an `_worker.js` on `wrangler pages publish`

<!---->

-   [#&#8203;2597](https://togithub.com/cloudflare/workers-sdk/pull/2597) [`416babf0`](https://togithub.com/cloudflare/workers-sdk/commit/416babf050ed3608e0fd747111561a4c7207185e) Thanks [@&#8203;petebacondarwin](https://togithub.com/petebacondarwin)! - fix: do not crash in wrangler dev when passing a request object to fetch

    This reverts and fixes the changes in [https://github.com/cloudflare/workers-sdk/pull/1769](https://togithub.com/cloudflare/workers-sdk/pull/1769)
    which does not support creating requests from requests whose bodies have already been consumed.

    Fixes [#&#8203;2562](https://togithub.com/cloudflare/workers-sdk/issues/2562)

<!---->

-   [#&#8203;2622](https://togithub.com/cloudflare/workers-sdk/pull/2622) [`9778b33e`](https://togithub.com/cloudflare/workers-sdk/commit/9778b33eb27f721fb743a970fb1520782ab0d573) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: implement `d1 list --json` with clean output for piping into other commands

    Before:

### [`v2.8.1`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#281)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.8.0...wrangler@2.8.1)

##### Patch Changes

-   [#&#8203;2501](https://togithub.com/cloudflare/workers-sdk/pull/2501) [`a0e5a491`](https://togithub.com/cloudflare/workers-sdk/commit/a0e5a4913621cffe757b2d14b6f3f466831f3d7f) Thanks [@&#8203;geelen](https://togithub.com/geelen)! - fix: make it possible to query d1 databases from durable objects

    This PR makes it possible to access D1 from Durable Objects.

    To be able to query D1 from your Durable Object, you'll need to install the latest version of wrangler, and redeploy your Worker.

    For a D1 binding like:

    ```toml
    [[d1_databases]]
    binding = "DB" # i.e. available in your Worker on env.DB
    database_name = "my-database-name"
    database_id = "UUID-GOES-HERE"
    preview_database_id = "UUID-GOES-HERE"
    ```

    You'll be able to access your D1 database via `env.DB` in your Durable Object.

<!---->

-   [#&#8203;2280](https://togithub.com/cloudflare/workers-sdk/pull/2280) [`ef110923`](https://togithub.com/cloudflare/workers-sdk/commit/ef1109235fb81200f32b953e9d448f9684d0363c) Thanks [@&#8203;penalosa](https://togithub.com/penalosa)! - Support `queue` and `trace` events in module middleware. This means that `queue` and `trace` events should work properly with the `--test-scheduled` flag

<!---->

-   [#&#8203;2526](https://togithub.com/cloudflare/workers-sdk/pull/2526) [`69d379a4`](https://togithub.com/cloudflare/workers-sdk/commit/69d379a48dd49c9c1812c89b2c7f1680e2196c0f) Thanks [@&#8203;jrf0110](https://togithub.com/jrf0110)! - Adds unstable_pages module to JS API

<!---->

-   [#&#8203;2558](https://togithub.com/cloudflare/workers-sdk/pull/2558) [`b910f644`](https://togithub.com/cloudflare/workers-sdk/commit/b910f644c7440ad034ffcaab288fcbb64deaa83b) Thanks [@&#8203;caass](https://togithub.com/caass)! - Add metrics for deployments

<!---->

-   [#&#8203;2554](https://togithub.com/cloudflare/workers-sdk/pull/2554) [`fbeaf609`](https://togithub.com/cloudflare/workers-sdk/commit/fbeaf6090e5eca4730358caa1838d0866d7b6006) Thanks [@&#8203;CarmenPopoviciu](https://togithub.com/CarmenPopoviciu)! - feat: Add support for wasm module imports in `wrangler pages dev`

    Currently it is not possible to import `wasm` modules in either Pages
    Functions or Pages Advanced Mode projects.

    This commit caries out work to address the aforementioned issue by
    enabling `wasm` module imports in `wrangler pages dev`. As a result,
    Pages users can now import their `wasm` modules withing their Functions
    or `_worker.js` files, and `wrangler pages dev` will correctly bundle
    everything and serve these "external" modules.

        import hello from "./hello.wasm"

        export async function onRequest() {
        	const module = await WebAssembly.instantiate(hello);
        	return new Response(module.exports.hello);
        }

<!---->

-   [#&#8203;2563](https://togithub.com/cloudflare/workers-sdk/pull/2563) [`5ba39569`](https://togithub.com/cloudflare/workers-sdk/commit/5ba39569e7ca6e341635b9beb8edebc60ad17dcd) Thanks [@&#8203;CarmenPopoviciu](https://togithub.com/CarmenPopoviciu)! - fix: Copy module imports related files to outdir

    When we bundle a Worker `esbuild` takes care of writing the
    results to the output directory. However, if the Worker contains
    any `external` imports, such as text/wasm/binary module imports,
    that cannot be inlined into the same bundle file, `bundleWorker`
    will not copy these files to the output directory. This doesn't
    affect `wrangler publish` per se, because of how the Worker
    upload FormData is created. It does however create some
    inconsistencies when running `wrangler publish --outdir` or
    `wrangler publish --outdir --dry-run`, in that, `outdir` will
    not contain those external import files.

    This commit addresses this issue by making sure the aforementioned
    files do get copied over to `outdir` together with `esbuild`'s
    resulting bundle files.

### [`v2.8.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#280)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.7.1...wrangler@2.8.0)

##### Minor Changes

-   [#&#8203;2538](https://togithub.com/cloudflare/workers-sdk/pull/2538) [`af4f27c5`](https://togithub.com/cloudflare/workers-sdk/commit/af4f27c5966f52e605ab7c16ff9746b7802d3479) Thanks [@&#8203;edevil](https://togithub.com/edevil)! - feat: support EmailEvent event type in `wrangler tail`.

<!---->

-   [#&#8203;2404](https://togithub.com/cloudflare/workers-sdk/pull/2404) [`3f824347`](https://togithub.com/cloudflare/workers-sdk/commit/3f824347c624a2cedf4af2b6bbd781b8581b08b5) Thanks [@&#8203;petebacondarwin](https://togithub.com/petebacondarwin)! - feat: support bundling the raw Pages `_worker.js` before deploying

    Previously, if you provided a `_worker.js` file, then Pages would simply check the
    file for disallowed imports and then deploy the file as-is.

    Not bundling the `_worker.js` file means that it cannot containing imports to other
    JS files, but also prevents Wrangler from adding shims such as the one for the D1 alpha
    release.

    This change adds the ability to tell Wrangler to pass the `_worker.js` through the
    normal Wrangler bundling process before deploying by setting the `--bundle`
    command line argument to `wrangler pages dev` and `wrangler pages publish`.

    This is in keeping with the same flag for `wrangler publish`.

    Currently bundling is opt-in, flag defaults to `false` if not provided.

##### Patch Changes

-   [#&#8203;2525](https://togithub.com/cloudflare/workers-sdk/pull/2525) [`fe8c6917`](https://togithub.com/cloudflare/workers-sdk/commit/fe8c69173821cfa5b0277e018df3a6207234b213) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: send `wrangler docs d1` to the right place

<!---->

-   [#&#8203;2542](https://togithub.com/cloudflare/workers-sdk/pull/2542) [`b44e1a75`](https://togithub.com/cloudflare/workers-sdk/commit/b44e1a7525248a4482248695742f3020347e3502) Thanks [@&#8203;GregBrimble](https://togithub.com/GregBrimble)! - chore: Rename `--bundle` to `--no-bundle` in Pages commands to make similar to Workers

<!---->

-   [#&#8203;2551](https://togithub.com/cloudflare/workers-sdk/pull/2551) [`bfffe595`](https://togithub.com/cloudflare/workers-sdk/commit/bfffe59558675a3c943fc24bb8b4e29066ae0581) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: wrangler init --from-dash incorrectly expects index.ts while writing index.js

    This PR fixes a bug where Wrangler would write a `wrangler.toml` expecting an index.ts file, while writing an index.js file.

<!---->

-   [#&#8203;2529](https://togithub.com/cloudflare/workers-sdk/pull/2529) [`2270507c`](https://togithub.com/cloudflare/workers-sdk/commit/2270507c7e7c7f0be4c39a9ee283147c0fe245cd) Thanks [@&#8203;CarmenPopoviciu](https://togithub.com/CarmenPopoviciu)! - Remove "experimental \_routes.json" warnings

    `_routes.json` is no longer considered an experimental feature, so let's
    remove all warnings we have in place for that.

<!---->

-   [#&#8203;2548](https://togithub.com/cloudflare/workers-sdk/pull/2548) [`4db768fa`](https://togithub.com/cloudflare/workers-sdk/commit/4db768fa5e05e4351b08113a20525c700324d502) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: path should be optional for wrangler d1 backup download

    This PR fixes a bug that forces folks to provide a `--output` flag to `wrangler d1 backup download`.

<!---->

-   [#&#8203;2528](https://togithub.com/cloudflare/workers-sdk/pull/2528) [`18208091`](https://togithub.com/cloudflare/workers-sdk/commit/18208091335e6fa60e736cdeed46462c4be42a38) Thanks [@&#8203;caass](https://togithub.com/caass)! - Add some guidance when folks encounter a 10021 error.

    Error code 10021 can occur when your worker doesn't pass startup validation. This error message will make it a little easier to reason about what happened and what to do next.

    Closes [#&#8203;2519](https://togithub.com/cloudflare/workers-sdk/issues/2519)

<!---->

-   [#&#8203;1769](https://togithub.com/cloudflare/workers-sdk/pull/1769) [`6a67efe9`](https://togithub.com/cloudflare/workers-sdk/commit/6a67efe9ae1da27fb95ffb030959465781bc74b6) Thanks [@&#8203;dario-piotrowicz](https://togithub.com/dario-piotrowicz)! - allow `fetch()` calls locally to accept URL Objects

### [`v2.7.1`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#271)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.7.0...wrangler@2.7.1)

##### Patch Changes

-   [#&#8203;2523](https://togithub.com/cloudflare/workers-sdk/pull/2523) [`a5e9958c`](https://togithub.com/cloudflare/workers-sdk/commit/a5e9958c7e37dd38c00ac6b713a21441491777fd) Thanks [@&#8203;jahands](https://togithub.com/jahands)! - fix: unstable_dev() experimental options incorrectly applying defaults

    A subtle difference when removing object-spreading of experimental unstable_dev() options caused `wrangler pages dev` interactivity to stop working. This switches back to object-spreading the passed in options on top of the defaults, fixing the issue.

### [`v2.7.0`](https://togithub.com/cloudflare/workers-sdk/blob/HEAD/packages/wrangler/CHANGELOG.md#270)

[Compare Source](https://togithub.com/cloudflare/workers-sdk/compare/wrangler@2.6.2...wrangler@2.7.0)

##### Minor Changes

-   [#&#8203;2465](https://togithub.com/cloudflare/workers-sdk/pull/2465) [`e1c2f5b9`](https://togithub.com/cloudflare/workers-sdk/commit/e1c2f5b9653ecc183bbc8a33531babd26e10d241) Thanks [@&#8203;JacobMGEvans](https://togithub.com/JacobMGEvans)! - After this PR, `wrangler init --yes` will generate a test for your new Worker project, using Vitest with TypeScript.
    When using `wrangler init`, and choosing to create a Typescript project, you will now be asked if Wrangler should write tests for you, using Vitest.

    This resolves issue [#&#8203;2436](https://togithub.com/cloudflare/workers-sdk/issues/2436).

<!---->

-   [#&#8203;2333](https://togithub.com/cloudflare/workers-sdk/pull/2333) [`71691421`](https://togithub.com/cloudflare/workers-sdk/commit/7169142171b1c9b4ff2f19b8b46871932ef7d10a) Thanks [@&#8203;markjmiller](https://togithub.com/markjmiller)! - Remove the experimental binding warning from Dispatch Namespace since [it is GA](https://blog.cloudflare.com/workers-for-platforms-ga/).

##### Patch Changes

-   [#&#8203;2460](https://togithub.com/cloudflare/workers-sdk/pull/2460) [`c2b2dfb8`](https://togithub.com/cloudflare/workers-sdk/commit/c2b2dfb8e5b44ee73418a01682e65d0ca1320797) Thanks [@&#8203;rozenmd](https://togithub.com/rozenmd)! - fix: resolve unstable_dev flakiness in tests by awaiting the dev registry

<!---->

-   [#&#8203;2439](https://togithub.com/cloudflare/workers-sdk/pull/2439) [`616f8739`](https://togithub.com/cloudflare/workers-sdk/commit/616f8739253381e8d691084961159d1a0073d81f) Thanks [@&#8203;petebacondarwin](https://togithub.com/petebacondarwin)! - fix(wrangler): do not login or read wrangler.toml when applying D1 migrations in local mode.

    When applying D1 migrations to a deployed database, it is important that we are logged in
    and that we have the database ID from the wrangler.toml.

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

_Label `internal` added by @renovate[bot] on 2024-03-25 17:34_

---

_Renamed from "Update dependency wrangler to v2.20.2 [SECURITY]" to "Update dependency wrangler to v2.20.2" by @renovate[bot] on 2024-03-25 17:58_

---

_Merged by @zanieb on 2024-03-25 22:50_

---

_Closed by @zanieb on 2024-03-25 22:50_

---

_Branch deleted on 2024-03-25 22:50_

---
