```yaml
number: 8511
title: Bump bitflags from 2.4.0 to 2.4.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/bitflags-2.4.1
created_at: 2023-11-06T08:30:44Z
updated_at: 2023-11-06T14:20:40Z
url: https://github.com/astral-sh/ruff/pull/8511
synced_at: 2026-01-12T15:55:26Z
```

# Bump bitflags from 2.4.0 to 2.4.1

---

_@dependabot_

Bumps [bitflags](https://github.com/bitflags/bitflags) from 2.4.0 to 2.4.1.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/bitflags/bitflags/releases">bitflags's releases</a>.</em></p>
<blockquote>
<h2>2.4.1</h2>
<h2>What's Changed</h2>
<ul>
<li>Allow some new pedantic clippy lints by <a href="https://github.com/KodrAus"><code>@​KodrAus</code></a> in <a href="https://redirect.github.com/bitflags/bitflags/pull/380">bitflags/bitflags#380</a></li>
<li>Prepare for 2.4.1 release by <a href="https://github.com/KodrAus"><code>@​KodrAus</code></a> in <a href="https://redirect.github.com/bitflags/bitflags/pull/381">bitflags/bitflags#381</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/bitflags/bitflags/compare/2.4.0...2.4.1">https://github.com/bitflags/bitflags/compare/2.4.0...2.4.1</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/bitflags/bitflags/blob/main/CHANGELOG.md">bitflags's changelog</a>.</em></p>
<blockquote>
<h1>2.4.1</h1>
<h2>What's Changed</h2>
<ul>
<li>Allow some new pedantic clippy lints by <a href="https://github.com/KodrAus"><code>@​KodrAus</code></a> in <a href="https://redirect.github.com/bitflags/bitflags/pull/380">bitflags/bitflags#380</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/bitflags/bitflags/compare/2.4.0...2.4.1">https://github.com/bitflags/bitflags/compare/2.4.0...2.4.1</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/bitflags/bitflags/commit/6c67922300d5abae779ca147bac00f6ff9c87f8a"><code>6c67922</code></a> Merge pull request <a href="https://redirect.github.com/bitflags/bitflags/issues/381">#381</a> from KodrAus/cargo/2.4.1</li>
<li><a href="https://github.com/bitflags/bitflags/commit/96ba9cf3a9f9fdaef752c51a1478ad89ddfe9134"><code>96ba9cf</code></a> prepare for 2.4.1 release</li>
<li><a href="https://github.com/bitflags/bitflags/commit/dd91b825b2574fed995df882f4c963723fb358f7"><code>dd91b82</code></a> Merge pull request <a href="https://redirect.github.com/bitflags/bitflags/issues/380">#380</a> from KodrAus/ci/compile-pass-clippy</li>
<li><a href="https://github.com/bitflags/bitflags/commit/03d14274dfd7cf1b46cb07cd2b3a558b6023ccba"><code>03d1427</code></a> ignore a few tests that run slowly in miri</li>
<li><a href="https://github.com/bitflags/bitflags/commit/3f05853276bcf8b3c1077a3f3a8b3e82a587a705"><code>3f05853</code></a> replace MIPS with Miri for cross endian tests</li>
<li><a href="https://github.com/bitflags/bitflags/commit/dfe6f21e5947046553476be51973a9aaec6675bd"><code>dfe6f21</code></a> just rely on smoke test for clippy</li>
<li><a href="https://github.com/bitflags/bitflags/commit/ea75f339841e490f9e1adb5dfef20df52462c5a6"><code>ea75f33</code></a> explicitly test on beta</li>
<li><a href="https://github.com/bitflags/bitflags/commit/60ef5ee91485b2af9fae00106925319143dd70ec"><code>60ef5ee</code></a> run clippy in compile-pass tests if available</li>
<li>See full diff in <a href="https://github.com/bitflags/bitflags/compare/2.4.0...2.4.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=bitflags&package-manager=cargo&previous-version=2.4.0&new-version=2.4.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-11-06 08:30_

---

_Comment by @github-actions[bot] on 2023-11-06 08:59_

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

_Merged by @charliermarsh on 2023-11-06 14:20_

---

_Closed by @charliermarsh on 2023-11-06 14:20_

---

_Branch deleted on 2023-11-06 14:20_

---
