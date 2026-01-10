```yaml
number: 2522
title: Bump rustls from 0.21.10 to 0.23.2
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/rustls-0.23.2
created_at: 2024-03-18T21:42:21Z
updated_at: 2024-03-18T21:52:05Z
url: https://github.com/astral-sh/uv/pull/2522
synced_at: 2026-01-10T14:49:08Z
```

# Bump rustls from 0.21.10 to 0.23.2

---

_Pull request opened by @dependabot on 2024-03-18 21:42_

Bumps [rustls](https://github.com/rustls/rustls) from 0.21.10 to 0.23.2.
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rustls/rustls/commit/bbef4b3ea72e610c8afad8d33706ee2e0e6a3c47"><code>bbef4b3</code></a> Prepare 0.23.2</li>
<li><a href="https://github.com/rustls/rustls/commit/295cfdef46b293493e44d2371959a889e1e36dcd"><code>295cfde</code></a> Mention rustls-post-quantum in providers docs</li>
<li><a href="https://github.com/rustls/rustls/commit/62e154cb997a81a1425ec99cf1b8da69acf49dd8"><code>62e154c</code></a> Add example client</li>
<li><a href="https://github.com/rustls/rustls/commit/092f3b569a36bb5754a53cfac0009e7efcf0850d"><code>092f3b5</code></a> Implement X25519Kyber768Draft00 key exchange</li>
<li><a href="https://github.com/rustls/rustls/commit/d4ec42ec1c294ce35b3366de77a293fee4f10cc2"><code>d4ec42e</code></a> Switch to using <code>SupportedKxGroup::start_and_complete()</code></li>
<li><a href="https://github.com/rustls/rustls/commit/96f07d7e208fb3222d4f1073efbd9c84c6216f5c"><code>96f07d7</code></a> Support KEM-shaped key exchange algorithms</li>
<li><a href="https://github.com/rustls/rustls/commit/6304e8f24c3577521b96fb4428fef086c7271845"><code>6304e8f</code></a> Introduce rustls-post-quantum &quot;crate&quot; skeleton</li>
<li><a href="https://github.com/rustls/rustls/commit/0398ac50fe18521da038aa39b55eec6c83450523"><code>0398ac5</code></a> deps: log 0.4.20 -&gt; 0.4.21</li>
<li><a href="https://github.com/rustls/rustls/commit/7588262aaca2636daeca7a69c84bb36d05e14338"><code>7588262</code></a> deps: rustls-pki-types 1.3.0 -&gt; 1.3.1</li>
<li><a href="https://github.com/rustls/rustls/commit/811d55eda430b917e070976899d3e9786541b3ea"><code>811d55e</code></a> deps: asn1 0.16.0 -&gt; 0.16.1</li>
<li>Additional commits viewable in <a href="https://github.com/rustls/rustls/compare/v/0.21.10...v/0.23.2">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=rustls&package-manager=cargo&previous-version=0.21.10&new-version=0.23.2)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

Dependabot will resolve any conflicts with this PR as long as you don't alter it yourself. You can also trigger a rebase manually by commenting `@dependabot rebase`.

[//]: # (dependabot-automerge-start)
[//]: # (dependabot-automerge-end)

---

<details>
<summary>Dependabot commands and options</summary>
<br />

You can trigger Dependabot actions by commenting on this PR:
- `@dependabot rebase` will rebase this PR
- `@dependabot recreate` will recreate this PR, overwriting any edits that have been made to it
- `@dependabot merge` will merge this PR after your CI passes on it
- `@dependabot squash and merge` will squash and merge this PR after your CI passes on it
- `@dependabot cancel merge` will cancel a previously requested merge and block automerging
- `@dependabot reopen` will reopen this PR if it is closed
- `@dependabot close` will close this PR and stop Dependabot recreating it. You can achieve the same result by closing it manually
- `@dependabot show <dependency name> ignore conditions` will show all of the ignore conditions of the specified dependency
- `@dependabot ignore this major version` will close this PR and stop Dependabot creating any more for this major version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this minor version` will close this PR and stop Dependabot creating any more for this minor version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this dependency` will close this PR and stop Dependabot creating any more for this dependency (unless you reopen the PR or upgrade to it yourself)


</details>

---

_Label `internal` added by @dependabot[bot] on 2024-03-18 21:42_

---

_Comment by @charliermarsh on 2024-03-18 21:52_

We need to stay in-sync with reqwest.

@dependabot ignore this minor version

---

_Closed by @charliermarsh on 2024-03-18 21:52_

---

_Comment by @dependabot[bot] on 2024-03-18 21:52_

OK, I won't notify you about version 0.23.x again, unless you re-open this PR.

---

_Branch deleted on 2024-03-18 21:52_

---
