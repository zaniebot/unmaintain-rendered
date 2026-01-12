```yaml
number: 8991
title: Bump fs-err from 2.10.0 to 2.11.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/fs-err-2.11.0
created_at: 2023-12-04T08:40:52Z
updated_at: 2023-12-04T10:49:35Z
url: https://github.com/astral-sh/ruff/pull/8991
synced_at: 2026-01-10T23:40:55Z
```

# Bump fs-err from 2.10.0 to 2.11.0

---

_Pull request opened by @dependabot on 2023-12-04 08:40_

Bumps [fs-err](https://github.com/andrewhickman/fs-err) from 2.10.0 to 2.11.0.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/andrewhickman/fs-err/blob/main/CHANGELOG.md">fs-err's changelog</a>.</em></p>
<blockquote>
<h2>2.11.0</h2>
<ul>
<li>Added the first line of the standard library documentation to each function's rustdocs, to make them more useful in IDEs (<a href="https://redirect.github.com/andrewhickman/fs-err/issues/45">#50</a>)</li>
<li>Fixed the wrapper for <code>tokio::fs::symlink_dir()</code> on Windows being incorrectly named <code>symlink</code>. The old function is now deprecated and will be removed in the next breaking release.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/andrewhickman/fs-err/commit/ba9888793c517a82499c8df1ad013529d8578777"><code>ba98887</code></a> chore: Release fs-err version 2.11.0</li>
<li><a href="https://github.com/andrewhickman/fs-err/commit/1747eca1aa6152ff7008cb1f3de4ebf5a35a2eb1"><code>1747eca</code></a> Update changelog</li>
<li><a href="https://github.com/andrewhickman/fs-err/commit/ef915218dfb89baca2b47e89a0f87bf4bd64bf11"><code>ef91521</code></a> Merge pull request <a href="https://redirect.github.com/andrewhickman/fs-err/issues/50">#50</a> from hniksic/main</li>
<li><a href="https://github.com/andrewhickman/fs-err/commit/5c0922b51880126b4d106a1f8971fe0d77912277"><code>5c0922b</code></a> Preserve (and deprecate) the old name fs_err::tokio::symlink() on Windows for...</li>
<li><a href="https://github.com/andrewhickman/fs-err/commit/f4965bd804ac0a49a2a6db4a3202fdd9b2d4d3ea"><code>f4965bd</code></a> Fix name of tokio::symlink_dir()</li>
<li><a href="https://github.com/andrewhickman/fs-err/commit/b81b8d43ee69862af2a851f7c3d5f5047f24c125"><code>b81b8d4</code></a> Add the initial paragraph of the original rustdoc to each wrapped function (<a href="https://redirect.github.com/andrewhickman/fs-err/issues/45">#45</a>)</li>
<li>See full diff in <a href="https://github.com/andrewhickman/fs-err/compare/2.10.0...2.11.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=fs-err&package-manager=cargo&previous-version=2.10.0&new-version=2.11.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-04 08:40_

---

_Comment by @github-actions[bot] on 2023-12-04 08:59_

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

_Merged by @MichaReiser on 2023-12-04 10:49_

---

_Closed by @MichaReiser on 2023-12-04 10:49_

---

_Branch deleted on 2023-12-04 10:49_

---
