```yaml
number: 9430
title: Bump serde from 1.0.193 to 1.0.195
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/serde-1.0.195
created_at: 2024-01-08T08:49:42Z
updated_at: 2024-01-08T09:09:06Z
url: https://github.com/astral-sh/ruff/pull/9430
synced_at: 2026-01-10T22:57:09Z
```

# Bump serde from 1.0.193 to 1.0.195

---

_Pull request opened by @dependabot on 2024-01-08 08:49_

Bumps [serde](https://github.com/serde-rs/serde) from 1.0.193 to 1.0.195.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/serde-rs/serde/releases">serde's releases</a>.</em></p>
<blockquote>
<h2>v1.0.195</h2>
<ul>
<li>Prevent remote definitions of tuple struct or tuple variant from triggering dead_code warning (<a href="https://redirect.github.com/serde-rs/serde/issues/2671">#2671</a>)</li>
</ul>
<h2>v1.0.194</h2>
<ul>
<li>Update proc-macro2 to fix caching issue when using a rustc-wrapper such as sccache</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/serde-rs/serde/commit/03eec42c3313b36da416be1486e9ecac345784d5"><code>03eec42</code></a> Release 1.0.195</li>
<li><a href="https://github.com/serde-rs/serde/commit/196f311ae2fd8ad94fe38a57830419859a4d3dbb"><code>196f311</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/serde/issues/2671">#2671</a> from dtolnay/deadremote</li>
<li><a href="https://github.com/serde-rs/serde/commit/38d9e0b2091e9b6150486c2c37367819b86bcc39"><code>38d9e0b</code></a> Revert &quot;Add FIXME to fix dead_code warning when using serde(remote)&quot;</li>
<li><a href="https://github.com/serde-rs/serde/commit/6502b3131697eff6420786ad71f87f29cfff3a13"><code>6502b31</code></a> Fix new dead_code warning in tuple struct and tuple variant remote defs</li>
<li><a href="https://github.com/serde-rs/serde/commit/6f1a8c3115c8d2502178c25d610fbaee2e82c46b"><code>6f1a8c3</code></a> Add FIXME to fix dead_code warning when using serde(remote)</li>
<li><a href="https://github.com/serde-rs/serde/commit/d883c94cc9fe72d0512dc7f4def7191a401595c9"><code>d883c94</code></a> Work around dead_code warning in tests</li>
<li><a href="https://github.com/serde-rs/serde/commit/961fa59a7469c5b5e323b9723323df412048d60d"><code>961fa59</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/serde/issues/2670">#2670</a> from serde-rs/exhaustive</li>
<li><a href="https://github.com/serde-rs/serde/commit/8bc71def551df190e6817d3311e5c76f751f53e6"><code>8bc71de</code></a> Fill in omitted patterns for GenericArguments match</li>
<li><a href="https://github.com/serde-rs/serde/commit/7c65a9dc0eab2d4d829b258a7b3549351bbe8dcd"><code>7c65a9d</code></a> Pick up changes to non_exhaustive_omitted_patterns lint</li>
<li><a href="https://github.com/serde-rs/serde/commit/d2d977a6c6dcff237ae956336d18b0c900c61aad"><code>d2d977a</code></a> Release 1.0.194</li>
<li>Additional commits viewable in <a href="https://github.com/serde-rs/serde/compare/v1.0.193...v1.0.195">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=serde&package-manager=cargo&previous-version=1.0.193&new-version=1.0.195)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-08 08:49_

---

_Merged by @MichaReiser on 2024-01-08 08:57_

---

_Closed by @MichaReiser on 2024-01-08 08:57_

---

_Branch deleted on 2024-01-08 08:57_

---

_Comment by @github-actions[bot] on 2024-01-08 09:09_

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
