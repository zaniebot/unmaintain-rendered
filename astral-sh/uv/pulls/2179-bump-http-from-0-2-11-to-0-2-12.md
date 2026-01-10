```yaml
number: 2179
title: Bump http from 0.2.11 to 0.2.12
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/http-0.2.12
created_at: 2024-03-04T22:04:21Z
updated_at: 2024-03-04T22:22:46Z
url: https://github.com/astral-sh/uv/pull/2179
synced_at: 2026-01-10T14:54:43Z
```

# Bump http from 0.2.11 to 0.2.12

---

_Pull request opened by @dependabot on 2024-03-04 22:04_

Bumps [http](https://github.com/hyperium/http) from 0.2.11 to 0.2.12.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/hyperium/http/releases">http's releases</a>.</em></p>
<blockquote>
<h2>v0.2.12</h2>
<h2>What's Changed</h2>
<ul>
<li>Add methods to allow trying to allocate in the <code>HeaderMap</code>, returning an error if oversize instead of panicking.</li>
<li>Fix <code>HeaderName::from_lowercase</code> that could allow NUL bytes in some cases.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/hyperium/http/blob/v0.2.12/CHANGELOG.md">http's changelog</a>.</em></p>
<blockquote>
<h1>0.2.12 (March 4, 2024)</h1>
<ul>
<li>Add methods to allow trying to allocate in the <code>HeaderMap</code>, returning an error if oversize instead of panicking.</li>
<li>Fix <code>HeaderName::from_lowercase</code> that could allow NUL bytes in some cases.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/hyperium/http/commit/351b63a991fe508e70a00af13e66099b3055c73f"><code>351b63a</code></a> v0.2.12</li>
<li><a href="https://github.com/hyperium/http/commit/e1a319730e1db0343052841d6dee8973dbcb9729"><code>e1a3197</code></a> fix: HeaderName::from_lowercase allowing NUL bytes in some cases</li>
<li><a href="https://github.com/hyperium/http/commit/9bb325984d18631f4e86c85daf853bbd9aa24a8c"><code>9bb3259</code></a> feat: add <code>HeaderMap::try_</code> methods to handle capacity overflow</li>
<li>See full diff in <a href="https://github.com/hyperium/http/compare/v0.2.11...v0.2.12">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=http&package-manager=cargo&previous-version=0.2.11&new-version=0.2.12)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-03-04 22:04_

---

_Merged by @zanieb on 2024-03-04 22:22_

---

_Closed by @zanieb on 2024-03-04 22:22_

---

_Branch deleted on 2024-03-04 22:22_

---
