```yaml
number: 9253
title: Bump toml from 0.8.2 to 0.8.8
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/toml-0.8.8
created_at: 2023-12-23T05:41:25Z
updated_at: 2023-12-23T12:35:25Z
url: https://github.com/astral-sh/ruff/pull/9253
synced_at: 2026-01-10T23:07:18Z
```

# Bump toml from 0.8.2 to 0.8.8

---

_Pull request opened by @dependabot on 2023-12-23 05:41_

Bumps [toml](https://github.com/toml-rs/toml) from 0.8.2 to 0.8.8.
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/toml-rs/toml/commit/2e99658cb84fdc897b66e95ba2d8bed0cce7a4e8"><code>2e99658</code></a> chore: Release</li>
<li><a href="https://github.com/toml-rs/toml/commit/61a68e5199fad60337cbca16d0da62f9200e3df3"><code>61a68e5</code></a> docs: Update changelog</li>
<li><a href="https://github.com/toml-rs/toml/commit/7b110a001fab182658429dad8998df0484502514"><code>7b110a0</code></a> Merge pull request <a href="https://redirect.github.com/toml-rs/toml/issues/649">#649</a> from epage/fix</li>
<li><a href="https://github.com/toml-rs/toml/commit/e162e9f6e18f7cb468e18e06803d0eafff23c4a1"><code>e162e9f</code></a> fix: Remove unused dependencies</li>
<li><a href="https://github.com/toml-rs/toml/commit/0026e2a520ef3d828d84486e0b748aba62767ab0"><code>0026e2a</code></a> chore: Release</li>
<li><a href="https://github.com/toml-rs/toml/commit/6b8e4aafc2b2cd1e61663519bcdd3f0a836dc6b5"><code>6b8e4aa</code></a> chore: Release</li>
<li><a href="https://github.com/toml-rs/toml/commit/36e3a02f4fbab13d2bbc43fc52d335257214ec2e"><code>36e3a02</code></a> docs: Update changelog</li>
<li><a href="https://github.com/toml-rs/toml/commit/7ba0932f13df81def43360e77181e24a942debfd"><code>7ba0932</code></a> Merge pull request <a href="https://redirect.github.com/toml-rs/toml/issues/648">#648</a> from epage/split</li>
<li><a href="https://github.com/toml-rs/toml/commit/ef9fd0a0bda34c2a1b9fc55baa2ec5bf79726d12"><code>ef9fd0a</code></a> fix(edit)!: Allow disabling parser or display</li>
<li><a href="https://github.com/toml-rs/toml/commit/5b53ff197b58099e5cae8ea4d3658804b1496294"><code>5b53ff1</code></a> refactor(edit): Remove 'use' for optional mods</li>
<li>Additional commits viewable in <a href="https://github.com/toml-rs/toml/compare/toml-v0.8.2...toml-v0.8.8">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=toml&package-manager=cargo&previous-version=0.8.2&new-version=0.8.8)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-23 05:41_

---

_Comment by @github-actions[bot] on 2023-12-23 05:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/dc37a6d035474c25932d805cfe50a47b04916a83/pandas/tests/window/test_rolling_functions.py#L120'>pandas/tests/window/test_rolling_functions.py:120:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/dc37a6d035474c25932d805cfe50a47b04916a83/pandas/tests/window/test_rolling_functions.py#L66'>pandas/tests/window/test_rolling_functions.py:66:5:</a> PLR0917 Too many positional arguments: (6/5)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2023-12-23 12:35_

---

_Closed by @charliermarsh on 2023-12-23 12:35_

---

_Branch deleted on 2023-12-23 12:35_

---
