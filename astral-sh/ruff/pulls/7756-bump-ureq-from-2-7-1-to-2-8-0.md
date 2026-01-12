```yaml
number: 7756
title: Bump ureq from 2.7.1 to 2.8.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/ureq-2.8.0
created_at: 2023-10-02T08:45:12Z
updated_at: 2023-10-02T13:50:01Z
url: https://github.com/astral-sh/ruff/pull/7756
synced_at: 2026-01-12T02:39:10Z
```

# Bump ureq from 2.7.1 to 2.8.0

---

_Pull request opened by @dependabot on 2023-10-02 08:45_

Bumps [ureq](https://github.com/algesten/ureq) from 2.7.1 to 2.8.0.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/algesten/ureq/blob/main/CHANGELOG.md">ureq's changelog</a>.</em></p>
<blockquote>
<h1>2.8.0</h1>
<h2>Fixed</h2>
<ul>
<li>Fix regression in IPv6 handling (<a href="https://redirect.github.com/algesten/ureq/issues/635">#635</a>)</li>
<li>Read proxy response to \r\n\r\n (<a href="https://redirect.github.com/algesten/ureq/issues/620">#620</a>)</li>
</ul>
<h2>Added</h2>
<ul>
<li>Auto-detect proxy from env vars (turned off by default) (<a href="https://redirect.github.com/algesten/ureq/issues/649">#649</a>)</li>
<li>Conversion ureq::Response -&gt; http::Response<!-- raw HTML omitted --> (<a href="https://redirect.github.com/algesten/ureq/issues/638">#638</a>)</li>
<li>cargo-deny CI action to disallow copy-left and duplicate deps (<a href="https://redirect.github.com/algesten/ureq/issues/661">#661</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/algesten/ureq/commit/10e1c4adab85a66d3e3f1c9b0b21af3496c9e9df"><code>10e1c4a</code></a> Do not lock down url dep</li>
<li><a href="https://github.com/algesten/ureq/commit/a2bb84306150f072a44bfea1e27bd3eaa101aa04"><code>a2bb843</code></a> Lock down url dep to 2.3.1</li>
<li><a href="https://github.com/algesten/ureq/commit/27be74c4e0f57f8958fb89bf2fed650ec632777d"><code>27be74c</code></a> Update Cargo.lock</li>
<li><a href="https://github.com/algesten/ureq/commit/5b7773c4a2a20ff3889b430f0d69de83417715eb"><code>5b7773c</code></a> Update changelog 2.8.0</li>
<li><a href="https://github.com/algesten/ureq/commit/a4cf7efb845452937d779fc455612069b9dbcc6e"><code>a4cf7ef</code></a> Add cargo-deny check to CI</li>
<li><a href="https://github.com/algesten/ureq/commit/a22d29f849014e97de7053d391b566ed0166cd75"><code>a22d29f</code></a> Version 2.8.0</li>
<li><a href="https://github.com/algesten/ureq/commit/4c835b7d3ece457fbc84ffef6aa6d066b01d5d74"><code>4c835b7</code></a> Fix proxy doc</li>
<li><a href="https://github.com/algesten/ureq/commit/a07f4f4446a9169413072e8bc4357db352c56fa4"><code>a07f4f4</code></a> fix <a href="https://redirect.github.com/algesten/ureq/issues/557">#557</a>: read proxy response until \r\n\r\n</li>
<li><a href="https://github.com/algesten/ureq/commit/bbc6be7df20b227cdc6dfeb143e6ebb673052e24"><code>bbc6be7</code></a> Fix clippy</li>
<li><a href="https://github.com/algesten/ureq/commit/a3bf7f05187d08547a3569920193ea896a5379ee"><code>a3bf7f0</code></a> Turn off default proxy detection</li>
<li>Additional commits viewable in <a href="https://github.com/algesten/ureq/compare/2.7.1...2.8.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=ureq&package-manager=cargo&previous-version=2.7.1&new-version=2.8.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-02 08:45_

---

_Comment by @github-actions[bot] on 2023-10-02 09:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-10-02 13:50_

---

_Closed by @charliermarsh on 2023-10-02 13:50_

---

_Branch deleted on 2023-10-02 13:50_

---
