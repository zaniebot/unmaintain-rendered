```yaml
number: 9941
title: Bump thiserror from 1.0.56 to 1.0.57
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/thiserror-1.0.57
created_at: 2024-02-12T08:46:13Z
updated_at: 2024-02-12T09:24:41Z
url: https://github.com/astral-sh/ruff/pull/9941
synced_at: 2026-01-10T22:57:09Z
```

# Bump thiserror from 1.0.56 to 1.0.57

---

_Pull request opened by @dependabot on 2024-02-12 08:46_

Bumps [thiserror](https://github.com/dtolnay/thiserror) from 1.0.56 to 1.0.57.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/thiserror/releases">thiserror's releases</a>.</em></p>
<blockquote>
<h2>1.0.57</h2>
<ul>
<li>Generate more efficient <code>Display</code> impl for error message which do not contain any interpolated value (<a href="https://redirect.github.com/dtolnay/thiserror/issues/286">#286</a>, thanks <a href="https://github.com/nyurik"><code>@​nyurik</code></a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/thiserror/commit/1d106b169c1ba328bcd64d70d06687413906d751"><code>1d106b1</code></a> Release 1.0.57</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/8a5c4d1b76eaa667a71dfaeb1373bca36fda4e78"><code>8a5c4d1</code></a> Use write_str when args only consists of trailing comma</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/f790bee2a401d71ac6e5492c7d1f8bb3a18a0e1c"><code>f790bee</code></a> Phrase flag in terms of whether core::fmt machinery is required</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/d43b759e3aa02d54dcad59c5eadfc78a8e96536f"><code>d43b759</code></a> Ignore needless_raw_string_hashes pedantic clippy lint in test</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/d09c4182955b459a4699adaf9b045077308a1d1a"><code>d09c418</code></a> Touch up PR 286</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/097251d2f538c123c850e1873cd1e0172bf4c151"><code>097251d</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/thiserror/issues/286">#286</a> from nyurik/litstr</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/cd79876fe8f2eead51a1d9efa0b0f42467b9bef8"><code>cd79876</code></a> optimize by avoiding second fmt.value() call</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/d7e738e1d8e339d35a1ea0c4c252b055c66c3526"><code>d7e738e</code></a> Optimize simple literals for Display::fmt</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/0717de3f507293f6faa7a32d987febb5c39a7048"><code>0717de3</code></a> Update ui test suite to nightly-2024-02-08</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/c7c75470ec80c253a197b365f4571569ab53a8d9"><code>c7c7547</code></a> Update ui test suite to nightly-2024-01-31</li>
<li>See full diff in <a href="https://github.com/dtolnay/thiserror/compare/1.0.56...1.0.57">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=thiserror&package-manager=cargo&previous-version=1.0.56&new-version=1.0.57)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-02-12 08:46_

---

_Comment by @github-actions[bot] on 2024-02-12 09:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/4e40afd9d9a6206a1cab83b4fdb0365cd739f576/pandas/_libs/window/indexers.pyi#L6'>pandas/_libs/window/indexers.pyi:6:5:</a> PLR0917 Too many positional arguments (6/5)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2024-02-12 09:24_

---

_Closed by @MichaReiser on 2024-02-12 09:24_

---

_Branch deleted on 2024-02-12 09:24_

---
