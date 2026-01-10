```yaml
number: 10562
title: Bump smallvec from 1.13.1 to 1.13.2
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/smallvec-1.13.2
created_at: 2024-03-25T08:57:09Z
updated_at: 2024-03-25T09:16:09Z
url: https://github.com/astral-sh/ruff/pull/10562
synced_at: 2026-01-10T22:47:02Z
```

# Bump smallvec from 1.13.1 to 1.13.2

---

_Pull request opened by @dependabot on 2024-03-25 08:57_

Bumps [smallvec](https://github.com/servo/rust-smallvec) from 1.13.1 to 1.13.2.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/servo/rust-smallvec/releases">smallvec's releases</a>.</em></p>
<blockquote>
<h2>v1.13.2</h2>
<h2>What's Changed</h2>
<ul>
<li>Add more tests for UB by <a href="https://github.com/workingjubilee"><code>@​workingjubilee</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/346">servo/rust-smallvec#346</a></li>
<li>Fix UB on out-of-bounds insert() by <a href="https://github.com/mbrubeck"><code>@​mbrubeck</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/345">servo/rust-smallvec#345</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/servo/rust-smallvec/compare/v1.13.1...v1.13.2">https://github.com/servo/rust-smallvec/compare/v1.13.1...v1.13.2</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/servo/rust-smallvec/commit/0089d0a15bee4792a575b9c51e9d7ace39b3957c"><code>0089d0a</code></a> Bump version to 1.13.2</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/b1d2814a3d717d79c06159e66dcd14a186d88d71"><code>b1d2814</code></a> Fix UB on out-of-bounds insert()</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/30573623ed8d40800098ecad415e8c23e38abe67"><code>3057362</code></a> Stop passing tag-raw-ptrs to MIRIFLAGS</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/f6665a5e7e30bf9b684f8cd3054cba7e8b18b5f2"><code>f6665a5</code></a> Add more tests for UB</li>
<li>See full diff in <a href="https://github.com/servo/rust-smallvec/compare/v1.13.1...v1.13.2">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=smallvec&package-manager=cargo&previous-version=1.13.1&new-version=1.13.2)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-03-25 08:57_

---

_Comment by @codspeed-hq[bot] on 2024-03-25 09:02_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/smallvec-1.13.2)

### Merging #10562 will **degrade performances by 5.41%**

<sub>Comparing <code>dependabot/cargo/smallvec-1.13.2</code> (831aa46) with <code>main</code> (e9115b8)</sub>



### Summary

`❌ 2 (👁 2)` regressions
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/smallvec-1.13.2` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/default-rules[large/dataset.py]` | 19.3 ms | 20.4 ms | -5.41% |
| 👁 | `linter/default-rules[unicode/pypinyin.py]` | 1.5 ms | 1.6 ms | -4.94% |


---

_Comment by @github-actions[bot] on 2024-03-25 09:15_

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

_Merged by @MichaReiser on 2024-03-25 09:16_

---

_Closed by @MichaReiser on 2024-03-25 09:16_

---

_Branch deleted on 2024-03-25 09:16_

---
