```yaml
number: 15656
title: Update dependency vite to v6.0.9
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/npm-vite-vulnerability
created_at: 2025-01-21T21:44:03Z
updated_at: 2025-01-22T07:29:03Z
url: https://github.com/astral-sh/ruff/pull/15656
synced_at: 2026-01-12T15:55:52Z
```

# Update dependency vite to v6.0.9

---

_@renovate_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [vite](https://vite.dev) ([source](https://redirect.github.com/vitejs/vite/tree/HEAD/packages/vite)) | [`6.0.7` -> `6.0.9`](https://renovatebot.com/diffs/npm/vite/6.0.7/6.0.9) | [![age](https://developer.mend.io/api/mc/badges/age/npm/vite/6.0.9?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/vite/6.0.9?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/vite/6.0.7/6.0.9?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/vite/6.0.7/6.0.9?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

### GitHub Vulnerability Alerts

#### [CVE-2025-24010](https://redirect.github.com/vitejs/vite/security/advisories/GHSA-vg6x-rcgg-rjx6)

### Summary
Vite allowed any websites to send any requests to the development server and read the response due to default CORS settings and lack of validation on the Origin header for WebSocket connections.

### Upgrade Path
Users that does not match either of the following conditions should be able to upgrade to a newer version of Vite that fixes the vulnerability without any additional configuration.

- Using the backend integration feature
- Using a reverse proxy in front of Vite
- Accessing the development server via a domain other than `localhost` or `*.localhost`
- Using a plugin / framework that connects to the WebSocket server on their own from the browser

#### Using the backend integration feature
If you are using the backend integration feature and not setting [`server.origin`](https://vite.dev/config/server-options.html#server-origin), you need to add the origin of the backend server to the [`server.cors.origin`](https://redirect.github.com/expressjs/cors#configuration-options) option. Make sure to set a specific origin rather than `*`, otherwise any origin can access your development server.

#### Using a reverse proxy in front of Vite
If you are using a reverse proxy in front of Vite and sending requests to Vite with a hostname other than `localhost` or `*.localhost`, you need to add the hostname to the new [`server.allowedHosts`](https://vite.dev/config/server-options.html#server-allowedhosts) option. For example, if the reverse proxy is sending requests to `http://vite:5173`, you need to add `vite` to the `server.allowedHosts` option.

#### Accessing the development server via a domain other than `localhost` or `*.localhost`
You need to add the hostname to the new [`server.allowedHosts`](https://vite.dev/config/server-options.html#server-allowedhosts) option. For example, if you are accessing the development server via `http://foo.example.com:8080`, you need to add `foo.example.com` to the `server.allowedHosts` option.

#### Using a plugin / framework that connects to the WebSocket server on their own from the browser
If you are using a plugin / framework, try upgrading to a newer version of Vite that fixes the vulnerability. If the WebSocket connection appears not to be working, the plugin / framework may have a code that connects to the WebSocket server on their own from the browser.

In that case, you can either:

- fix the plugin / framework code to the make it compatible with the new version of Vite
- set `legacy.skipWebSocketTokenCheck: true` to opt-out the fix for [2] while the plugin / framework is incompatible with the new version of Vite
  - When enabling this option, **make sure that you are aware of the security implications** described in the impact section of [2] above.

### Mitigation without upgrading Vite

#### [1]: Permissive default CORS settings
Set `server.cors` to `false` or limit `server.cors.origin` to trusted origins.

#### [2]: Lack of validation on the Origin header for WebSocket connections
There aren't any mitigations for this.

#### [3]: Lack of validation on the Host header for HTTP requests
Use Chrome 94+ or use HTTPS for the development server.

### Details

There are three causes that allowed malicious websites to send any requests to the development server:

#### [1]: Permissive default CORS settings

Vite sets the [`Access-Control-Allow-Origin`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) header depending on [`server.cors`](https://vite.dev/config/server-options.html#server-cors) option. The default value was `true` which sets `Access-Control-Allow-Origin: *`. This allows websites on any origin to `fetch` contents served on the development server.

Attack scenario:

1. The attacker serves a malicious web page (`http://malicious.example.com`).
2. The user accesses the malicious web page.
3. The attacker sends a `fetch('http://127.0.0.1:5173/main.js')` request by JS in that malicious web page. This request is normally blocked by same-origin policy, but that's not the case for the reasons above.
4. The attacker gets the content of `http://127.0.0.1:5173/main.js`.

#### [2]: Lack of validation on the Origin header for WebSocket connections

Vite starts a WebSocket server to handle HMR and other functionalities. This WebSocket server [did not perform validation on the Origin header](https://redirect.github.com/vitejs/vite/blob/v6.0.7/packages/vite/src/node/server/ws.ts#L145-L157) and was vulnerable to Cross-Site WebSocket Hijacking (CSWSH) attacks. With that attack, an attacker can read and write messages on the WebSocket connection. Vite only sends some information over the WebSocket connection ([list of the file paths that changed, the file content where the errored happened, etc.](https://redirect.github.com/vitejs/vite/blob/v6.0.7/packages/vite/types/hmrPayload.d.ts#L12-L72)), but plugins can send arbitrary messages and may include more sensitive information.

Attack scenario:

1. The attacker serves a malicious web page (`http://malicious.example.com`).
2. The user accesses the malicious web page.
3. The attacker runs `new WebSocket('http://127.0.0.1:5173', 'vite-hmr')` by JS in that malicious web page.
4. The user edits some files.
5. Vite sends some HMR messages over WebSocket.
6. The attacker gets the content of the HMR messages.

#### [3]: Lack of validation on the Host header for HTTP requests

Unless [`server.https`](https://vite.dev/config/server-options.html#server-https) is set, Vite starts the development server on HTTP. Non-HTTPS servers are vulnerable to DNS rebinding attacks without validation on the Host header. But Vite did not perform validation on the Host header. By exploiting this vulnerability, an attacker can send arbitrary requests to the development server bypassing the same-origin policy.

1. The attacker serves a malicious web page that is served on **HTTP** (`http://malicious.example.com:5173`) (HTTPS won't work).
2. The user accesses the malicious web page.
3. The attacker changes the DNS to point to 127.0.0.1 (or other private addresses).
4. The attacker sends a `fetch('/main.js')` request by JS in that malicious web page.
5. The attacker gets the content of `http://127.0.0.1:5173/main.js` bypassing the same origin policy.

### Impact

#### [1]: Permissive default CORS settings
Users with the default `server.cors` option may:

- get the source code stolen by malicious websites
- give the attacker access to functionalities that are not supposed to be exposed externally
  - Vite core does not have any functionality that causes changes somewhere else when receiving a request, but plugins may implement those functionalities and servers behind `server.proxy` may have those functionalities.

#### [2]: Lack of validation on the Origin header for WebSocket connections
All users may get the file paths of the files that changed and the file content where the error happened be stolen by malicious websites.

For users that is using a plugin that sends messages over WebSocket, that content may be stolen by malicious websites.

For users that is using a plugin that has a functionality that is triggered by messages over WebSocket, that functionality may be exploited by malicious websites.

#### [3]: Lack of validation on the Host header for HTTP requests
Users using HTTP for the development server and using a browser that is not Chrome 94+ may:

- get the source code stolen by malicious websites
- give the attacker access to functionalities that are not supposed to be exposed externally
  - Vite core does not have any functionality that causes changes somewhere else when receiving a request, but plugins may implement those functionalities and servers behind `server.proxy` may have those functionalities.

Chrome 94+ users are not affected for [3], because [sending a request to a private network page from public non-HTTPS page is forbidden](https://developer.chrome.com/blog/private-network-access-update#chrome_94) since Chrome 94.

### Related Information
Safari has [a bug that blocks requests to loopback addresses from HTTPS origins](https://bugs.webkit.org/show_bug.cgi?id=171934). This means when the user is using Safari and Vite is listening on lookback addresses, there's another condition of "the malicious web page is served on HTTP" to make [1] and [2] to work.

### PoC

#### [2]: Lack of validation on the Origin header for WebSocket connections
1. I used the `react` template which utilizes HMR functionality.

```
npm create vite@latest my-vue-app-react -- --template react
```

2. Then on a malicious server, serve the following POC html:
```html
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8" />
        <title>vite CSWSH</title>
    </head>
    <body>
        <div id="logs"></div>
        <script>
            const div = document.querySelectorAll('#logs')[0];
            const ws = new WebSocket('ws://localhost:5173','vite-hmr');
            ws.onmessage = event => {
                const logLine = document.createElement('p');
                logLine.innerHTML = event.data;
                div.append(logLine);
            };
        </script>
    </body>
</html>
```

3. Kick off Vite 

```
npm run dev
```

4. Load the development server (open `http://localhost:5173/`) as well as the malicious page in the browser. 
5. Edit `src/App.jsx` file and intentionally place a syntax error
6. Notice how the malicious page can view the websocket messages and a snippet of the source code is exposed

Here's a video demonstrating the POC:

https://github.com/user-attachments/assets/a4ad05cd-0b34-461c-9ff6-d7c8663d6961

---

### Release Notes

<details>
<summary>vitejs/vite (vite)</summary>

### [`v6.0.9`](https://redirect.github.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small609-2025-01-20-small)

[Compare Source](https://redirect.github.com/vitejs/vite/compare/v6.0.8...v6.0.9)

-   fix!: check host header to prevent DNS rebinding attacks and introduce `server.allowedHosts` ([bd896fb](https://redirect.github.com/vitejs/vite/commit/bd896fb5f312fc0ff1730166d1d142fc0d34ba6d))
-   fix!: default `server.cors: false` to disallow fetching from untrusted origins ([b09572a](https://redirect.github.com/vitejs/vite/commit/b09572acc939351f4e4c50ddf793017a92c678b1))
-   fix: verify token for HMR WebSocket connection ([029dcd6](https://redirect.github.com/vitejs/vite/commit/029dcd6d77d3e3ef10bc38e9a0829784d9760fdb))

### [`v6.0.8`](https://redirect.github.com/vitejs/vite/blob/HEAD/packages/vite/CHANGELOG.md#small608-2025-01-20-small)

[Compare Source](https://redirect.github.com/vitejs/vite/compare/v6.0.7...v6.0.8)

-   fix: avoid SSR HMR for HTML files ([#&#8203;19193](https://redirect.github.com/vitejs/vite/issues/19193)) ([3bd55bc](https://redirect.github.com/vitejs/vite/commit/3bd55bcb7e831d2c4f66c90d7bbb3e1fbf7a02b6)), closes [#&#8203;19193](https://redirect.github.com/vitejs/vite/issues/19193)
-   fix: build time display 7m 60s ([#&#8203;19108](https://redirect.github.com/vitejs/vite/issues/19108)) ([cf0d2c8](https://redirect.github.com/vitejs/vite/commit/cf0d2c8e232a1af716c71cdd2218d180f7ecc02b)), closes [#&#8203;19108](https://redirect.github.com/vitejs/vite/issues/19108)
-   fix: don't resolve URL starting with double slash ([#&#8203;19059](https://redirect.github.com/vitejs/vite/issues/19059)) ([35942cd](https://redirect.github.com/vitejs/vite/commit/35942cde11fd8a68fa89bf25f7aa1ddb87d775b2)), closes [#&#8203;19059](https://redirect.github.com/vitejs/vite/issues/19059)
-   fix: ensure `server.close()` only called once ([#&#8203;19204](https://redirect.github.com/vitejs/vite/issues/19204)) ([db81c2d](https://redirect.github.com/vitejs/vite/commit/db81c2dada961f40c0882b5182adf2f34bb5c178)), closes [#&#8203;19204](https://redirect.github.com/vitejs/vite/issues/19204)
-   fix: resolve.conditions in ResolvedConfig was `defaultServerConditions` ([#&#8203;19174](https://redirect.github.com/vitejs/vite/issues/19174)) ([ad75c56](https://redirect.github.com/vitejs/vite/commit/ad75c56dce5618a3a416e18f9a5c3880d437a107)), closes [#&#8203;19174](https://redirect.github.com/vitejs/vite/issues/19174)
-   fix: tree shake stringified JSON imports ([#&#8203;19189](https://redirect.github.com/vitejs/vite/issues/19189)) ([f2aed62](https://redirect.github.com/vitejs/vite/commit/f2aed62d0bf1b66e870ee6b4aab80cd1702793ab)), closes [#&#8203;19189](https://redirect.github.com/vitejs/vite/issues/19189)
-   fix: use shared sigterm callback ([#&#8203;19203](https://redirect.github.com/vitejs/vite/issues/19203)) ([47039f4](https://redirect.github.com/vitejs/vite/commit/47039f4643179be31a8d7c7fbff83c5c13deb787)), closes [#&#8203;19203](https://redirect.github.com/vitejs/vite/issues/19203)
-   fix(deps): update all non-major dependencies ([#&#8203;19098](https://redirect.github.com/vitejs/vite/issues/19098)) ([8639538](https://redirect.github.com/vitejs/vite/commit/8639538e6498d1109da583ad942c1472098b5919)), closes [#&#8203;19098](https://redirect.github.com/vitejs/vite/issues/19098)
-   fix(optimizer): use correct default install state path for yarn PnP ([#&#8203;19119](https://redirect.github.com/vitejs/vite/issues/19119)) ([e690d8b](https://redirect.github.com/vitejs/vite/commit/e690d8bb1e5741e81df5b7a6a5c8c3c1c971fa41)), closes [#&#8203;19119](https://redirect.github.com/vitejs/vite/issues/19119)
-   fix(types): improve `ESBuildOptions.include / exclude` type to allow `readonly (string | RegExp)[]`  ([ea53e70](https://redirect.github.com/vitejs/vite/commit/ea53e7095297ea4192490fd58556414cc59a8975)), closes [#&#8203;19146](https://redirect.github.com/vitejs/vite/issues/19146)
-   chore(deps): update dependency pathe to v2 ([#&#8203;19139](https://redirect.github.com/vitejs/vite/issues/19139)) ([71506f0](https://redirect.github.com/vitejs/vite/commit/71506f0a8deda5254cb49c743cd439dfe42859ce)), closes [#&#8203;19139](https://redirect.github.com/vitejs/vite/issues/19139)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xMDcuMCIsInVwZGF0ZWRJblZlciI6IjM5LjEwNy4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCIsInNlY3VyaXR5Il19-->


---

_Label `internal` added by @renovate[bot] on 2025-01-21 21:44_

---

_Label `security` added by @renovate[bot] on 2025-01-21 21:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-21 22:15_

---

_Label `security` removed by @MichaReiser on 2025-01-22 07:28_

---

_Merged by @MichaReiser on 2025-01-22 07:29_

---

_Closed by @MichaReiser on 2025-01-22 07:29_

---

_Branch deleted on 2025-01-22 07:29_

---
