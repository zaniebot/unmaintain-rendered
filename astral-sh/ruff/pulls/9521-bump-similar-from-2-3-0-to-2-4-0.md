```yaml
number: 9521
title: Bump similar from 2.3.0 to 2.4.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/similar-2.4.0
created_at: 2024-01-15T08:25:51Z
updated_at: 2024-01-15T09:30:59Z
url: https://github.com/astral-sh/ruff/pull/9521
synced_at: 2026-01-10T22:57:09Z
```

# Bump similar from 2.3.0 to 2.4.0

---

_Pull request opened by @dependabot on 2024-01-15 08:25_

Bumps [similar](https://github.com/mitsuhiko/similar) from 2.3.0 to 2.4.0.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/mitsuhiko/similar/blob/main/CHANGELOG.md">similar's changelog</a>.</em></p>
<blockquote>
<h2>2.4.0</h2>
<ul>
<li>Fixed a bug where the LCS diff algorithm didn't always call <code>D::finish</code>.  (<a href="https://redirect.github.com/mitsuhiko/similar/issues/58">#58</a>)</li>
<li>Fixed a bug in LCS that caused a panic if the common prefix and the
common suffix overlapped.  (<a href="https://redirect.github.com/mitsuhiko/similar/issues/59">#59</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/mitsuhiko/similar/commit/ace8f34a27c465a97380da2683ba4979005925d2"><code>ace8f34</code></a> 2.4.0</li>
<li><a href="https://github.com/mitsuhiko/similar/commit/e9a05ed6fa1668e76699e360b300f828d52747dd"><code>e9a05ed</code></a> Fix overlap bug in LCS (<a href="https://redirect.github.com/mitsuhiko/similar/issues/59">#59</a>)</li>
<li><a href="https://github.com/mitsuhiko/similar/commit/18712783da6fec2658773286445f14af1fb999ea"><code>1871278</code></a> Always call finish (<a href="https://redirect.github.com/mitsuhiko/similar/issues/58">#58</a>)</li>
<li><a href="https://github.com/mitsuhiko/similar/commit/f5c1afa8f47578009629ada4613d6f2342735c79"><code>f5c1afa</code></a> Use unwrap_or (<a href="https://redirect.github.com/mitsuhiko/similar/issues/56">#56</a>)</li>
<li><a href="https://github.com/mitsuhiko/similar/commit/2b31f65445df9093ba007ca5a5ae6a71b899d491"><code>2b31f65</code></a> doc(inline/iter_strings_lossy): describe different behaviors (<a href="https://redirect.github.com/mitsuhiko/similar/issues/52">#52</a>)</li>
<li>See full diff in <a href="https://github.com/mitsuhiko/similar/compare/2.3.0...2.4.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=similar&package-manager=cargo&previous-version=2.3.0&new-version=2.4.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-15 08:25_

---

_Comment by @codspeed-hq[bot] on 2024-01-15 08:33_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/similar-2.4.0)

### Merging #9521 will **degrade performances by 4.35%**

<sub>Comparing <code>dependabot/cargo/similar-2.4.0</code> (4dc43e1) with <code>main</code> (6183b8e)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/similar-2.4.0)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/similar-2.4.0` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @github-actions[bot] on 2024-01-15 08:44_

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

_Merged by @MichaReiser on 2024-01-15 09:30_

---

_Closed by @MichaReiser on 2024-01-15 09:30_

---

_Branch deleted on 2024-01-15 09:30_

---
