```yaml
number: 9607
title: Bump smallvec from 1.11.2 to 1.13.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/smallvec-1.13.1
created_at: 2024-01-22T08:15:37Z
updated_at: 2024-01-22T08:34:29Z
url: https://github.com/astral-sh/ruff/pull/9607
synced_at: 2026-01-10T22:57:09Z
```

# Bump smallvec from 1.11.2 to 1.13.1

---

_Pull request opened by @dependabot on 2024-01-22 08:15_

Bumps [smallvec](https://github.com/servo/rust-smallvec) from 1.11.2 to 1.13.1.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/servo/rust-smallvec/releases">smallvec's releases</a>.</em></p>
<blockquote>
<h2>v1.13.1</h2>
<ul>
<li>Remove the optional <code>get-size</code> feature, to avoid a cyclic dependency (<a href="https://redirect.github.com/servo/rust-smallvec/issues/335">#335</a>).</li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/servo/rust-smallvec/compare/v1.13.0...v1.13.1">https://github.com/servo/rust-smallvec/compare/v1.13.0...v1.13.1</a></p>
<h2>v1.13.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Impl get_size::GetSize (behind feature flag) by <a href="https://github.com/amandasaurus"><code>@​amandasaurus</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/335">servo/rust-smallvec#335</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/servo/rust-smallvec/compare/v1.12.0...v1.13.0">https://github.com/servo/rust-smallvec/compare/v1.12.0...v1.13.0</a></p>
<h2>v1.12.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Add <code>from_const_with_len_unchecked</code> by <a href="https://github.com/Expyron"><code>@​Expyron</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/329">servo/rust-smallvec#329</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/Expyron"><code>@​Expyron</code></a> made their first contribution in <a href="https://redirect.github.com/servo/rust-smallvec/pull/329">servo/rust-smallvec#329</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/servo/rust-smallvec/compare/v1.11.2...v1.12.0">https://github.com/servo/rust-smallvec/compare/v1.11.2...v1.12.0</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/servo/rust-smallvec/commit/f8136b8b208f028be1783f0842faea91c2f913fd"><code>f8136b8</code></a> Version 1.13.1</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/25f5d910349d7a07a7c60729af22365e17313632"><code>25f5d91</code></a> Revert &quot;Impl get_size::GetSize (behind feature flag)&quot;</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/ff0544479f24899c1ae41e39f987ac7cd824f7a3"><code>ff05444</code></a> Version 1.13.0</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/38863e83dbc84fc289a7f93c89e091ab454498c3"><code>38863e8</code></a> Impl get_size::GetSize (behind feature flag)</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/93b0e20e7cecff656ad4884b0a1dfecbff8f2b7a"><code>93b0e20</code></a> Version 1.12.0</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/c638a0b91faabdf78eb0c3dbb2bb826791f96be8"><code>c638a0b</code></a> Add <code>from_const_with_len_unchecked</code></li>
<li>See full diff in <a href="https://github.com/servo/rust-smallvec/compare/v1.11.2...v1.13.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=smallvec&package-manager=cargo&previous-version=1.11.2&new-version=1.13.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-22 08:15_

---

_Merged by @MichaReiser on 2024-01-22 08:31_

---

_Closed by @MichaReiser on 2024-01-22 08:31_

---

_Branch deleted on 2024-01-22 08:31_

---

_Comment by @github-actions[bot] on 2024-01-22 08:34_

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
