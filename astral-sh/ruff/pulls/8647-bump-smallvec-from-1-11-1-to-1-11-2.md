```yaml
number: 8647
title: Bump smallvec from 1.11.1 to 1.11.2
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/smallvec-1.11.2
created_at: 2023-11-13T08:08:36Z
updated_at: 2023-11-13T14:38:13Z
url: https://github.com/astral-sh/ruff/pull/8647
synced_at: 2026-01-12T15:55:26Z
```

# Bump smallvec from 1.11.1 to 1.11.2

---

_@dependabot_

Bumps [smallvec](https://github.com/servo/rust-smallvec) from 1.11.1 to 1.11.2.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/servo/rust-smallvec/releases">smallvec's releases</a>.</em></p>
<blockquote>
<h2>v1.11.2</h2>
<h2>What's Changed</h2>
<ul>
<li>Automated testing improvements by <a href="https://github.com/waywardmonkeys"><code>@​waywardmonkeys</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/322">servo/rust-smallvec#322</a> and <a href="https://redirect.github.com/servo/rust-smallvec/pull/326">servo/rust-smallvec#326</a></li>
<li>fix: don't special-case <code>doc</code> for <code>feature = &quot;const_generics&quot;</code> by <a href="https://github.com/mkroening"><code>@​mkroening</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/328">servo/rust-smallvec#328</a></li>
<li>Code cleanup by <a href="https://github.com/emilio"><code>@​emilio</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/316">servo/rust-smallvec#316</a> and <a href="https://github.com/waywardmonkeys"><code>@​waywardmonkeys</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/323">servo/rust-smallvec#323</a></li>
<li>Minor tweaks to doc formatting. by <a href="https://github.com/waywardmonkeys"><code>@​waywardmonkeys</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/318">servo/rust-smallvec#318</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/mkroening"><code>@​mkroening</code></a> made their first contribution in <a href="https://redirect.github.com/servo/rust-smallvec/pull/328">servo/rust-smallvec#328</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/servo/rust-smallvec/compare/v1.11.1...v1.11.2">https://github.com/servo/rust-smallvec/compare/v1.11.1...v1.11.2</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/servo/rust-smallvec/commit/e29cccc67d9ac4be6b42a2b21443a561f30b1cd9"><code>e29cccc</code></a> Version 1.11.2</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/79aa952cbd1e161c9c40de2168936a2f6afe0d3c"><code>79aa952</code></a> fix: don't special-case <code>doc</code> for <code>feature = &quot;const_generics&quot;</code></li>
<li><a href="https://github.com/servo/rust-smallvec/commit/6fcb81f965f0069851c68583c6b8ceb61569fcc8"><code>6fcb81f</code></a> ci: Add CI for no_std.</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/21284113aaa77b5f25582c0cc78f42dbfa5d0c11"><code>2128411</code></a> Remove most usages of &quot;extern crate&quot;.</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/b9c76c136c6c5ef9c2d1695323fc57d7f4262de8"><code>b9c76c1</code></a> ci: Update to actions/checkout@v4.</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/8d90a0d5be4eb62a8820d7b9b10123d8c7e12adc"><code>8d90a0d</code></a> Minor tweaks to doc formatting.</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/83505258f21c3d425e438968fb4ddbc00a155171"><code>8350525</code></a> Remove trailing whitespace.</li>
<li>See full diff in <a href="https://github.com/servo/rust-smallvec/compare/v1.11.1...v1.11.2">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=smallvec&package-manager=cargo&previous-version=1.11.1&new-version=1.11.2)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-11-13 08:08_

---

_Comment by @github-actions[bot] on 2023-11-13 08:27_

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

_Merged by @charliermarsh on 2023-11-13 14:38_

---

_Closed by @charliermarsh on 2023-11-13 14:38_

---

_Branch deleted on 2023-11-13 14:38_

---
