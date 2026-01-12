```yaml
number: 7404
title: Bump syn from 2.0.29 to 2.0.33
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/syn-2.0.33
created_at: 2023-09-15T08:34:16Z
updated_at: 2023-09-15T09:07:46Z
url: https://github.com/astral-sh/ruff/pull/7404
synced_at: 2026-01-12T02:39:10Z
```

# Bump syn from 2.0.29 to 2.0.33

---

_Pull request opened by @dependabot on 2023-09-15 08:34_

Bumps [syn](https://github.com/dtolnay/syn) from 2.0.29 to 2.0.33.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/syn/releases">syn's releases</a>.</em></p>
<blockquote>
<h2>2.0.33</h2>
<ul>
<li>Special handling for the <code>(/*ERROR*/)</code> placeholder that rustc uses for macros that fail to expand</li>
</ul>
<h2>2.0.32</h2>
<ul>
<li>Add <code>Path::require_ident</code> accessor (<a href="https://redirect.github.com/dtolnay/syn/issues/1496">#1496</a>, thanks <a href="https://github.com/Fancyflame"><code>@​Fancyflame</code></a>)</li>
</ul>
<h2>2.0.31</h2>
<ul>
<li>Parse generics and where-clause on const items (<a href="https://redirect.github.com/rust-lang/rust/issues/113521">rust-lang/rust#113521</a>)</li>
</ul>
<h2>2.0.30</h2>
<ul>
<li>Parse unnamed struct/union type syntax (<a href="https://redirect.github.com/rust-lang/rust/issues/49804">rust-lang/rust#49804</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/syn/commit/5e3f55e684b7e33424b9f551105463418b196eb4"><code>5e3f55e</code></a> Release 2.0.33</li>
<li><a href="https://github.com/dtolnay/syn/commit/3e04809f5218c6d5fb2b09a6c55933e785825c75"><code>3e04809</code></a> Pull in proc-macro2 error placeholder change</li>
<li><a href="https://github.com/dtolnay/syn/commit/2cd5608a4c37810bb0947b0c161a20695b3ce487"><code>2cd5608</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1508">#1508</a> from dtolnay/error</li>
<li><a href="https://github.com/dtolnay/syn/commit/84cfe09484f6468bc85fb01db11c6a10fcb2d988"><code>84cfe09</code></a> Fall through to 'Unrecognized literal' error</li>
<li><a href="https://github.com/dtolnay/syn/commit/a80570c81bf15c9afcf8e2470ab06f60f7a8183d"><code>a80570c</code></a> Parse rustc's representation of macro expansion error</li>
<li><a href="https://github.com/dtolnay/syn/commit/41b83c86f7e702c49b93bc184e026de4650c0d8e"><code>41b83c8</code></a> Release 2.0.32</li>
<li><a href="https://github.com/dtolnay/syn/commit/e05bcea1f6c03980bdae95828f6fd5fd061de7ef"><code>e05bcea</code></a> Reword Path::require_ident documentation</li>
<li><a href="https://github.com/dtolnay/syn/commit/07a89ddd28cdc1b477e8ee842322adfc33a59802"><code>07a89dd</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/syn/issues/1496">#1496</a> from Fancyflame/master</li>
<li><a href="https://github.com/dtolnay/syn/commit/81945ea8d25e1c3792db741ea4a005b23edcfe41"><code>81945ea</code></a> Update actions/checkout@v3 -&gt; v4</li>
<li><a href="https://github.com/dtolnay/syn/commit/b20e2c8245f739dffc8095b4406bdff4cc179972"><code>b20e2c8</code></a> Release 2.0.31</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/syn/compare/2.0.29...2.0.33">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=syn&package-manager=cargo&previous-version=2.0.29&new-version=2.0.33)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-15 08:34_

---

_Merged by @MichaReiser on 2023-09-15 08:49_

---

_Closed by @MichaReiser on 2023-09-15 08:49_

---

_Branch deleted on 2023-09-15 08:49_

---

_Comment by @github-actions[bot] on 2023-09-15 09:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
