```yaml
number: 8646
title: Bump annotate-snippets from 0.9.1 to 0.9.2
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/annotate-snippets-0.9.2
created_at: 2023-11-13T08:08:18Z
updated_at: 2023-11-13T15:05:40Z
url: https://github.com/astral-sh/ruff/pull/8646
synced_at: 2026-01-12T15:55:26Z
```

# Bump annotate-snippets from 0.9.1 to 0.9.2

---

_@dependabot_

Bumps [annotate-snippets](https://github.com/rust-lang/annotate-snippets-rs) from 0.9.1 to 0.9.2.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/rust-lang/annotate-snippets-rs/releases">annotate-snippets's releases</a>.</em></p>
<blockquote>
<h2>annotate-snippets 0.9.2 (October 30, 2023)</h2>
<ul>
<li>Remove parsing of __ in title strings, fixes (<a href="https://redirect.github.com/rust-lang/annotate-snippets-rs/issues/53">#53</a>)</li>
<li>Origin line number is not correct when using a slice with fold: true (<a href="https://redirect.github.com/rust-lang/annotate-snippets-rs/issues/52">#52</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rust-lang/annotate-snippets-rs/blob/master/CHANGELOG.md">annotate-snippets's changelog</a>.</em></p>
<blockquote>
<h2>[0.9.2] - October 30, 2023</h2>
<ul>
<li>Remove parsing of __ in title strings, fixes (<a href="https://redirect.github.com/rust-lang/annotate-snippets-rs/issues/53">#53</a>)</li>
<li>Origin line number is not correct when using a slice with fold: true (<a href="https://redirect.github.com/rust-lang/annotate-snippets-rs/issues/52">#52</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/bc504bc3ef6c856152d63d6304845014d046a2bd"><code>bc504bc</code></a> annotate-snippets 0.9.2</li>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/160ef451acded1f92e7ddc462f9e1d8a3f99dcce"><code>160ef45</code></a> Merge pull request <a href="https://redirect.github.com/rust-lang/annotate-snippets-rs/issues/54">#54</a> from ndmitchell/fix-53</li>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/a620165e7925fb8c15012efaa127e1eab4a23c9a"><code>a620165</code></a> Merge pull request <a href="https://redirect.github.com/rust-lang/annotate-snippets-rs/issues/55">#55</a> from Inky-developer/fix-52</li>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/08141133f0d84f81525f2b596ed89f23d8592a8e"><code>0814113</code></a> Fix <a href="https://redirect.github.com/rust-lang/annotate-snippets-rs/issues/52">#52</a></li>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/ee4687bf8d668f8cb6b76e5a518de7016a5ef7bd"><code>ee4687b</code></a> Remove parsing of __ in title strings, fixes <a href="https://redirect.github.com/rust-lang/annotate-snippets-rs/issues/53">#53</a></li>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/448b80421b5a9c39c3ed1a708636014aec82eac1"><code>448b804</code></a> Merge pull request <a href="https://redirect.github.com/rust-lang/annotate-snippets-rs/issues/50">#50</a> from jim4067/patch-2</li>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/ee58f7765489a403aedf7286dcd0f0c1d9aab42a"><code>ee58f77</code></a> update ci.yml</li>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/21b23ee4c333da7f899edcae47bd63db88246038"><code>21b23ee</code></a> update ci.yml</li>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/e2bf6e83745d25513e9954b978de5d5bf2523121"><code>e2bf6e8</code></a> remove travis ci badge and add github workflows badge</li>
<li><a href="https://github.com/rust-lang/annotate-snippets-rs/commit/c3fdb1c933e81f46419b4c3841c1242fab7f2744"><code>c3fdb1c</code></a> rename workflow to ci</li>
<li>Additional commits viewable in <a href="https://github.com/rust-lang/annotate-snippets-rs/compare/0.9.1...0.9.2">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=annotate-snippets&package-manager=cargo&previous-version=0.9.1&new-version=0.9.2)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Review requested from @charliermarsh by @charliermarsh on 2023-11-13 14:44_

---

_Merged by @charliermarsh on 2023-11-13 14:55_

---

_Closed by @charliermarsh on 2023-11-13 14:55_

---

_Branch deleted on 2023-11-13 14:55_

---

_Comment by @github-actions[bot] on 2023-11-13 15:05_

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
