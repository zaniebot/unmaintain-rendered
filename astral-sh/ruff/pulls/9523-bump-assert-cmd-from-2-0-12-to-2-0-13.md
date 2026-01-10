```yaml
number: 9523
title: Bump assert_cmd from 2.0.12 to 2.0.13
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/assert_cmd-2.0.13
created_at: 2024-01-15T08:28:34Z
updated_at: 2024-01-15T09:30:22Z
url: https://github.com/astral-sh/ruff/pull/9523
synced_at: 2026-01-10T22:57:09Z
```

# Bump assert_cmd from 2.0.12 to 2.0.13

---

_Pull request opened by @dependabot on 2024-01-15 08:28_

Bumps [assert_cmd](https://github.com/assert-rs/assert_cmd) from 2.0.12 to 2.0.13.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/assert-rs/assert_cmd/blob/master/CHANGELOG.md">assert_cmd's changelog</a>.</em></p>
<blockquote>
<h2>[2.0.13] - 2024-01-12</h2>
<h3>Internal</h3>
<ul>
<li>Dependency update</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/19da72b81c789f9c06817b99c1ecebfe7083dbfb"><code>19da72b</code></a> chore: Release assert_cmd version 2.0.13</li>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/db5ee325aafffb5b8e042cda1cc946e36079302a"><code>db5ee32</code></a> docs: Update changelog</li>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/86d96ed348c63ac7eeac332e292bf80f18d06ed9"><code>86d96ed</code></a> chore: Update anstream</li>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/8e3cb3f154ad9ecd0f326871d388a0eb61b14a50"><code>8e3cb3f</code></a> Merge pull request <a href="https://redirect.github.com/assert-rs/assert_cmd/issues/191">#191</a> from assert-rs/renovate/github-codeql-action-3.x</li>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/8159e3d3e377406e967e3afe6af5cd43bc4b73f8"><code>8159e3d</code></a> Merge pull request <a href="https://redirect.github.com/assert-rs/assert_cmd/issues/190">#190</a> from assert-rs/renovate/actions-setup-python-5.x</li>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/48c7f1dfb771b0f8dada7e8eaac860905a7cc4c8"><code>48c7f1d</code></a> chore(deps): update github/codeql-action action to v3</li>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/c5291f822fe3db0c0fb47046d04e428d81fa8b6c"><code>c5291f8</code></a> chore(deps): update actions/setup-python action to v5</li>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/a909b08504ab16170f2eb7ab30b2c5b53c69ebd0"><code>a909b08</code></a> chore: Update from '_rust/main'</li>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/8c836eaa9d9279df467991a3b8463d748b515a0a"><code>8c836ea</code></a> Merge pull request <a href="https://redirect.github.com/assert-rs/assert_cmd/issues/10">#10</a> from epage/renovate/migrate-config</li>
<li><a href="https://github.com/assert-rs/assert_cmd/commit/598c6244983fb392457f3fbec9badf25fab6d051"><code>598c624</code></a> chore(config): migrate config .github/renovate.json5</li>
<li>Additional commits viewable in <a href="https://github.com/assert-rs/assert_cmd/compare/v2.0.12...v2.0.13">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=assert_cmd&package-manager=cargo&previous-version=2.0.12&new-version=2.0.13)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-15 08:28_

---

_Comment by @codspeed-hq[bot] on 2024-01-15 08:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/assert_cmd-2.0.13)

### Merging #9523 will **degrade performances by 4.35%**

<sub>Comparing <code>dependabot/cargo/assert_cmd-2.0.13</code> (6f789d0) with <code>main</code> (6183b8e)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/assert_cmd-2.0.13)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/assert_cmd-2.0.13` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @github-actions[bot] on 2024-01-15 08:47_

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
