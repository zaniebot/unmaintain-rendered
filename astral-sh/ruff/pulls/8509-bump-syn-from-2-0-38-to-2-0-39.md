```yaml
number: 8509
title: Bump syn from 2.0.38 to 2.0.39
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/syn-2.0.39
created_at: 2023-11-06T08:30:11Z
updated_at: 2023-11-06T14:19:49Z
url: https://github.com/astral-sh/ruff/pull/8509
synced_at: 2026-01-10T23:40:55Z
```

# Bump syn from 2.0.38 to 2.0.39

---

_Pull request opened by @dependabot on 2023-11-06 08:30_

Bumps [syn](https://github.com/dtolnay/syn) from 2.0.38 to 2.0.39.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/syn/releases">syn's releases</a>.</em></p>
<blockquote>
<h2>2.0.39</h2>
<ul>
<li>Fix parsing of return expression in match guards (<a href="https://redirect.github.com/dtolnay/syn/issues/1528">#1528</a>)</li>
<li>Improve error message on labeled loop as value expression for break (<a href="https://redirect.github.com/dtolnay/syn/issues/1531">#1531</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/syn/commit/924217c1734c085ee6cdb5e198d36d610c234313"><code>924217c</code></a> Release 2.0.39</li>
<li><a href="https://github.com/dtolnay/syn/commit/95aeeb559866368d8b910d388c8e04b96083666a"><code>95aeeb5</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1531">#1531</a> from dtolnay/breaklabel</li>
<li><a href="https://github.com/dtolnay/syn/commit/b88f86fae5c1ac284179181bf7a34549e6d96d7c"><code>b88f86f</code></a> Ignore single_element_loop clippy lint in test</li>
<li><a href="https://github.com/dtolnay/syn/commit/a876185366557cb0e0e175c3be48721c8cc0d8e6"><code>a876185</code></a> Improve error on break followed by labeled loop</li>
<li><a href="https://github.com/dtolnay/syn/commit/32ab9794d6b75248c1946ae18baab12a46137eb0"><code>32ab979</code></a> Add test of ambiguous label parsing in break</li>
<li><a href="https://github.com/dtolnay/syn/commit/6f658f88a213e25b7ac7ec2dc77c537de679b745"><code>6f658f8</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1530">#1530</a> from dtolnay/canbeginexpr</li>
<li><a href="https://github.com/dtolnay/syn/commit/20497e1a555812cfe05769b4d8c117c4e0e4edbd"><code>20497e1</code></a> More precise decision to parse expression after return keyword</li>
<li><a href="https://github.com/dtolnay/syn/commit/c6a651aa12447d50c22db3189ef38cff90fc267b"><code>c6a651a</code></a> Update name of ExprReturn parse function to match variant name</li>
<li><a href="https://github.com/dtolnay/syn/commit/c2745901009f7d65e7b708cd80031642038f765e"><code>c274590</code></a> Indicate that peek argument refers to syn::Ident</li>
<li><a href="https://github.com/dtolnay/syn/commit/3e67cb0c660fc906770d212465652bfeb7a75727"><code>3e67cb0</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1529">#1529</a> from dtolnay/up</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/syn/compare/2.0.38...2.0.39">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=syn&package-manager=cargo&previous-version=2.0.38&new-version=2.0.39)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-11-06 08:30_

---

_Comment by @github-actions[bot] on 2023-11-06 08:50_

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

_Merged by @charliermarsh on 2023-11-06 14:19_

---

_Closed by @charliermarsh on 2023-11-06 14:19_

---

_Branch deleted on 2023-11-06 14:19_

---
