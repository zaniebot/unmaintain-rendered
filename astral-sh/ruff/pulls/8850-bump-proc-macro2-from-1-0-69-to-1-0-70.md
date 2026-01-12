```yaml
number: 8850
title: Bump proc-macro2 from 1.0.69 to 1.0.70
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/proc-macro2-1.0.70
created_at: 2023-11-27T08:49:04Z
updated_at: 2023-11-27T10:24:02Z
url: https://github.com/astral-sh/ruff/pull/8850
synced_at: 2026-01-10T23:40:55Z
```

# Bump proc-macro2 from 1.0.69 to 1.0.70

---

_Pull request opened by @dependabot on 2023-11-27 08:49_

Bumps [proc-macro2](https://github.com/dtolnay/proc-macro2) from 1.0.69 to 1.0.70.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/proc-macro2/releases">proc-macro2's releases</a>.</em></p>
<blockquote>
<h2>1.0.70</h2>
<ul>
<li>Add #[track_caller] on <code>Ident::new</code> so that panics on invalid input are attributed to the caller (<a href="https://redirect.github.com/dtolnay/proc-macro2/issues/423">#423</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/cd31a69e9c6ece9e6a1bf366aae37ae5b00cef82"><code>cd31a69</code></a> Release 1.0.70</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/4482662ae5ef045a551dcc95b9506acdde69a8e3"><code>4482662</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/423">#423</a> from dtolnay/trackcaller</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/690ab11e042dbd9936c2a8fa98267e1df87a6002"><code>690ab11</code></a> Track caller for Ident validation panics</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/f15383f9f0165847e554b6232ba22b90c9315d7b"><code>f15383f</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/422">#422</a> from dtolnay/newchecked</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/ea2cd7f2e4ad199f1a04ccf9788b5dbed2ac8d59"><code>ea2cd7f</code></a> Rename internal Ident::new -&gt; Ident::new_checked</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/80591951011fba02b4296e35b06c4f1b41d5f9b5"><code>8059195</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/421">#421</a> from dtolnay/identunchecked</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/805a6adf92ba8da4b6d7ce5ff1def6b53f1f1cb5"><code>805a6ad</code></a> Bypass Ident validation on identifiers created by parser</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/c51346270ee53abaefd1231e418fd757252ab84d"><code>c513462</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/420">#420</a> from dtolnay/newraw</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/bd778e138376ba7ad7168335fb11afab86d158f9"><code>bd778e1</code></a> Delete Ident::_new_raw</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/0cb3649e019d8e2e913056c839c98d27fada6526"><code>0cb3649</code></a> Ignore checked_conversions pedantic clippy lint</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/proc-macro2/compare/1.0.69...1.0.70">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=proc-macro2&package-manager=cargo&previous-version=1.0.69&new-version=1.0.70)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-11-27 08:49_

---

_Comment by @github-actions[bot] on 2023-11-27 09:05_

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

_Merged by @MichaReiser on 2023-11-27 10:14_

---

_Closed by @MichaReiser on 2023-11-27 10:14_

---

_Branch deleted on 2023-11-27 10:14_

---
