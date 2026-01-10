```yaml
number: 9525
title: Bump semver from 1.0.20 to 1.0.21
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/semver-1.0.21
created_at: 2024-01-15T08:31:03Z
updated_at: 2024-01-15T09:28:05Z
url: https://github.com/astral-sh/ruff/pull/9525
synced_at: 2026-01-10T22:57:09Z
```

# Bump semver from 1.0.20 to 1.0.21

---

_Pull request opened by @dependabot on 2024-01-15 08:31_

Bumps [semver](https://github.com/dtolnay/semver) from 1.0.20 to 1.0.21.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/semver/releases">semver's releases</a>.</em></p>
<blockquote>
<h2>1.0.21</h2>
<ul>
<li>Update proc-macro2 to fix caching issue when using a rustc-wrapper such as sccache</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/semver/commit/f9cc2df9415c880bd3610c2cdb6785ac7cad31ea"><code>f9cc2df</code></a> Release 1.0.21</li>
<li><a href="https://github.com/dtolnay/semver/commit/87914b14dc19cc64f4428d78973fbfb7431a93f1"><code>87914b1</code></a> Pull in proc-macro2 sccache fix</li>
<li><a href="https://github.com/dtolnay/semver/commit/b6171889ac7e8f47ec6f12003571bdcc7f737b10"><code>b617188</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/semver/issues/309">#309</a> from dtolnay/optionifletelse</li>
<li><a href="https://github.com/dtolnay/semver/commit/5481cb9574a3980af0ad7fe84141998f327d39c1"><code>5481cb9</code></a> Remove option_if_let_else clippy suppression</li>
<li>See full diff in <a href="https://github.com/dtolnay/semver/compare/1.0.20...1.0.21">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=semver&package-manager=cargo&previous-version=1.0.20&new-version=1.0.21)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-15 08:31_

---

_Comment by @codspeed-hq[bot] on 2024-01-15 08:38_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/semver-1.0.21)

### Merging #9525 will **degrade performances by 4.35%**

<sub>Comparing <code>dependabot/cargo/semver-1.0.21</code> (cb726a5) with <code>main</code> (6183b8e)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/semver-1.0.21)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/semver-1.0.21` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @github-actions[bot] on 2024-01-15 08:49_

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

_Merged by @MichaReiser on 2024-01-15 09:28_

---

_Closed by @MichaReiser on 2024-01-15 09:28_

---

_Branch deleted on 2024-01-15 09:28_

---
