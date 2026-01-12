```yaml
number: 9086
title: Bump syn from 2.0.39 to 2.0.40
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/syn-2.0.40
created_at: 2023-12-11T08:44:01Z
updated_at: 2023-12-11T09:01:05Z
url: https://github.com/astral-sh/ruff/pull/9086
synced_at: 2026-01-10T23:40:55Z
```

# Bump syn from 2.0.39 to 2.0.40

---

_Pull request opened by @dependabot on 2023-12-11 08:44_

Bumps [syn](https://github.com/dtolnay/syn) from 2.0.39 to 2.0.40.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/syn/releases">syn's releases</a>.</em></p>
<blockquote>
<h2>2.0.40</h2>
<ul>
<li>Fix some edge cases of handling None-delimited groups in expression parser (<a href="https://redirect.github.com/dtolnay/syn/issues/1539">#1539</a>, <a href="https://redirect.github.com/dtolnay/syn/issues/1541">#1541</a>, <a href="https://redirect.github.com/dtolnay/syn/issues/1542">#1542</a>, <a href="https://redirect.github.com/dtolnay/syn/issues/1543">#1543</a>, <a href="https://redirect.github.com/dtolnay/syn/issues/1544">#1544</a>, <a href="https://redirect.github.com/dtolnay/syn/issues/1545">#1545</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/syn/commit/cf7f40a96a7329fb4215a7a1d8d9e7c93439b169"><code>cf7f40a</code></a> Release 2.0.40</li>
<li><a href="https://github.com/dtolnay/syn/commit/1ce8ccf5cd92824d1058019c0ae95b0616f6ac01"><code>1ce8ccf</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1538">#1538</a> from dtolnay/testinvisible</li>
<li><a href="https://github.com/dtolnay/syn/commit/d06bff8883e794c0d9d5fe2998210dec113b6eff"><code>d06bff8</code></a> Add test for parsing Delimiter::None in expressions</li>
<li><a href="https://github.com/dtolnay/syn/commit/9ec66d42bba0da0041ab6448604b72f673e185f1"><code>9ec66d4</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1545">#1545</a> from dtolnay/groupedlet</li>
<li><a href="https://github.com/dtolnay/syn/commit/384621acc640779ee4a8250317c26a42e8ceef90"><code>384621a</code></a> Fix None-delimited let expression in stmt position</li>
<li><a href="https://github.com/dtolnay/syn/commit/5325b6d1711c1c9c6787cb6239b472b5f8b5daaf"><code>5325b6d</code></a> Add test of let expr surrounded in None-delimited group</li>
<li><a href="https://github.com/dtolnay/syn/commit/0ddfc27cf79573db0b5533c583ff895e9c7f3f72"><code>0ddfc27</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1544">#1544</a> from dtolnay/nonedelimloop</li>
<li><a href="https://github.com/dtolnay/syn/commit/9c99b3f62ecebb2ac22f8ed99039934b0a18d0e6"><code>9c99b3f</code></a> Fix stmt boundary after None-delimited group containing loop</li>
<li><a href="https://github.com/dtolnay/syn/commit/2781584ea868cce6c9ae750bc442e3b4b2dfd887"><code>2781584</code></a> Add test of None-delimited group containing loop in match arm</li>
<li><a href="https://github.com/dtolnay/syn/commit/d33292808432c8530b53c951f2780d7128c4bd0b"><code>d332928</code></a> Simplify token stream construction in Delimiter::None tests</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/syn/compare/2.0.39...2.0.40">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=syn&package-manager=cargo&previous-version=2.0.39&new-version=2.0.40)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-11 08:44_

---

_Merged by @konstin on 2023-12-11 08:50_

---

_Closed by @konstin on 2023-12-11 08:50_

---

_Branch deleted on 2023-12-11 08:50_

---

_Comment by @github-actions[bot] on 2023-12-11 09:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
