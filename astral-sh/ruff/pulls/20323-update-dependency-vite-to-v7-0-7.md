```yaml
number: 20323
title: Update dependency vite to v7.0.7
type: pull_request
state: merged
author: renovate
labels:
  - internal
  - security
assignees: []
merged: true
base: main
head: renovate/npm-vite-vulnerability
created_at: 2025-09-10T02:03:11Z
updated_at: 2025-09-15T09:57:08Z
url: https://github.com/astral-sh/ruff/pull/20323
synced_at: 2026-01-12T15:56:59Z
```

# Update dependency vite to v7.0.7

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Confidence |
|---|---|---|---|
| [vite](https://vite.dev) ([source](https://redirect.github.com/vitejs/vite/tree/HEAD/packages/vite)) | [`7.0.6` -> `7.0.7`](https://renovatebot.com/diffs/npm/vite/7.0.6/7.0.7) | [![age](https://developer.mend.io/api/mc/badges/age/npm/vite/7.0.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/vite/7.0.6/7.0.7?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

### GitHub Vulnerability Alerts

#### [CVE-2025-58752](https://redirect.github.com/vitejs/vite/security/advisories/GHSA-jqfw-vq24-v9c3)

### Summary
Any HTML files on the machine were served regardless of the `server.fs` settings.

### Impact

Only apps that match the following conditions are affected:

- explicitly exposes the Vite dev server to the network (using --host or [server.host config option](https://vitejs.dev/config/server-options.html#server-host))
- `appType: 'spa'` (default) or `appType: 'mpa'` is used

This vulnerability also affects the preview server. The preview server allowed HTML files not under the output directory to be served.

### Details
The [serveStaticMiddleware](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/static.ts#L123) function is in charge of serving static files from the server. It returns the [viteServeStaticMiddleware](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/static.ts#L136) function which runs the needed tests and serves the page. The viteServeStaticMiddleware function [checks if the extension of the requested file is ".html"](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/static.ts#L144). If so, it doesn't serve the page. Instead, the server will go on to the next middlewares, in this case [htmlFallbackMiddleware](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/htmlFallback.ts#L14), and then to [indexHtmlMiddleware](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/indexHtml.ts#L438). These middlewares don't perform any test against allow or deny rules, and they don't make sure that the accessed file is in the root directory of the server. They just find the file and send back its contents to the client.

### PoC
Execute the following shell commands:

```
npm  create  vite@latest
cd vite-project/
echo  "secret" > /tmp/secret.html
npm install
npm run dev
```

Then, in a different shell, run the following command:

`curl  -v  --path-as-is  'http://localhost:5173/../../../../../../../../../../../tmp/secret.html'`

The contents of /tmp/secret.html will be returned.

This will also work for HTML files that are in the root directory of the project, but are in the deny list (or not in the allow list). Test that by stopping the running server (CTRL+C), and running the following commands in the server's shell:

```
echo  'import path from "node:path"; import { defineConfig } from "vite"; export default defineConfig({server: {fs: {deny: [path.resolve(__dirname, "secret_files/*")]}}})'  >  [vite.config.js](http://vite.config.js)
mkdir secret_files
echo "secret txt" > secret_files/secret.txt
echo "secret html" > secret_files/secret.html
npm run dev

```

Then, in a different shell, run the following command:

`curl  -v  --path-as-is  'http://localhost:5173/secret_files/secret.txt'`

You will receive a 403 HTTP Response,  because everything in the secret_files directory is denied.

Now in the same shell run the following command:

`curl  -v  --path-as-is  'http://localhost:5173/secret_files/secret.html'`

You will receive the contents of secret_files/secret.html.

#### [CVE-2025-58751](https://redirect.github.com/vitejs/vite/security/advisories/GHSA-g4jq-h2w9-997c)

### Summary
Files starting with the same name with the public directory were served bypassing the `server.fs` settings.

### Impact
Only apps that match the following conditions are affected:

- explicitly exposes the Vite dev server to the network (using --host or [`server.host` config option](https://vitejs.dev/config/server-options.html#server-host))
- uses [the public directory feature](https://vite.dev/guide/assets.html#the-public-directory) (enabled by default)
- a symlink exists in the public directory

### Details
The [servePublicMiddleware](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/static.ts#L79) function is in charge of serving public files from the server. It returns the [viteServePublicMiddleware](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/static.ts#L106) function which runs the needed tests and serves the page. The viteServePublicMiddleware function [checks if the publicFiles variable is defined](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/static.ts#L111), and then uses it to determine if the requested page is public. In the case that the publicFiles is undefined, the code will treat the requested page as a public page, and go on with the serving function. [publicFiles may be undefined if there is a symbolic link anywhere inside the public directory](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/publicDir.ts#L21). In that case, every requested page will be passed to the public serving function. The serving function is based on the [sirv](https://redirect.github.com/lukeed/sirv) library. Vite patches the library to add the possibility to test loading access to pages, but when the public page middleware [disables this functionality](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/static.ts#L89) since public pages are meant to be available always, regardless of whether they are in the allow or deny list.

In the case of public pages, the serving function is [provided with the path to the public directory](https://redirect.github.com/vitejs/vite/blob/9719497adec4ad5ead21cafa19a324bb1d480194/packages/vite/src/node/server/middlewares/static.ts#L85) as a root directory. The code of the sirv library [uses the join function to get the full path to the requested file](https://redirect.github.com/lukeed/sirv/blob/d061616827dd32d53b61ec9530c9445c8f592620/packages/sirv/index.mjs#L42). For example, if the public directory is "/www/public", and the requested file is "myfile", the code will join them to the string "/www/public/myfile". The code will then pass this string to the normalize function. Afterwards, the code will [use the string's startsWith function](https://redirect.github.com/lukeed/sirv/blob/d061616827dd32d53b61ec9530c9445c8f592620/packages/sirv/index.mjs#L43) to determine whether the created path is within the given directory or not. Only if it is, it will be served.

Since [sirv trims the trailing slash of the public directory](https://redirect.github.com/lukeed/sirv/blob/d061616827dd32d53b61ec9530c9445c8f592620/packages/sirv/index.mjs#L119), the string's startsWith function may return true even if the created path is not within the public directory. For example, if the server's root is at "/www", and the public directory is at "/www/p", if the created path will be "/www/private.txt", the startsWith function will still return true, because the string "/www/private.txt" starts with  "/www/p". To achieve this, the attacker will use ".." to ask for the file "../private.txt". The code will then join it to the "/www/p" string, and will receive "/www/p/../private.txt". Then, the normalize function will return "/www/private.txt", which will then be passed to the startsWith function, which will return true, and the processing of the page will continue without checking the deny list (since this is the public directory middleware which doesn't check that).

### PoC
Execute the following shell commands:

```
npm  create  vite@latest
cd vite-project/
mkdir p
cd p
ln -s a b
cd ..
echo  'import path from "node:path"; import { defineConfig } from "vite"; export default defineConfig({publicDir: path.resolve(__dirname, "p/"), server: {fs: {deny: [path.resolve(__dirname, "private.txt")]}}})' > vite.config.js
echo  "secret" > private.txt
npm install
npm run dev
```

Then, in a different shell, run the following command:

`curl -v --path-as-is 'http://localhost:5173/private.txt'`

You will receive a 403 HTTP Response,  because private.txt is denied.

Now in the same shell run the following command:

`curl -v --path-as-is 'http://localhost:5173/../private.txt'`

You will receive the contents of private.txt.

### Related links
- https://github.com/lukeed/sirv/commit/f0113f3f8266328d804ee808f763a3c11f8997eb

---

### Release Notes

<details>
<summary>vitejs/vite (vite)</summary>

### [`v7.0.7`](https://redirect.github.com/vitejs/vite/releases/tag/v7.0.7)

[Compare Source](https://redirect.github.com/vitejs/vite/compare/v7.0.6...v7.0.7)

Please refer to [CHANGELOG.md](https://redirect.github.com/vitejs/vite/blob/v7.0.7/packages/vite/CHANGELOG.md) for details.

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS45Ny4xMCIsInVwZGF0ZWRJblZlciI6IjQxLjk3LjEwIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCIsInNlY3VyaXR5Il19-->


---

_Label `internal` added by @renovate[bot] on 2025-09-10 02:03_

---

_Label `security` added by @renovate[bot] on 2025-09-10 02:03_

---

_Merged by @MichaReiser on 2025-09-15 09:57_

---

_Closed by @MichaReiser on 2025-09-15 09:57_

---

_Branch deleted on 2025-09-15 09:57_

---
