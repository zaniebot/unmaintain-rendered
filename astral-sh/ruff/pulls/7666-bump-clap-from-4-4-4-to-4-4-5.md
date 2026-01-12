```yaml
number: 7666
title: Bump clap from 4.4.4 to 4.4.5
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/clap-4.4.5
created_at: 2023-09-26T08:42:51Z
updated_at: 2023-09-26T11:02:18Z
url: https://github.com/astral-sh/ruff/pull/7666
synced_at: 2026-01-12T15:55:24Z
```

# Bump clap from 4.4.4 to 4.4.5

---

_@dependabot_

Bumps [clap](https://github.com/clap-rs/clap) from 4.4.4 to 4.4.5.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/clap-rs/clap/releases">clap's releases</a>.</em></p>
<blockquote>
<h2>v4.4.5</h2>
<h2>[4.4.5] - 2023-09-25</h2>
<h3>Fixes</h3>
<ul>
<li><em>(parser)</em> When inferring subcommand <code>name</code> or <code>long_flag</code>, allow ambiguous-looking matches that unambiguously map back to the same command</li>
<li><em>(parser)</em> When inferring subcommand <code>long_flag</code>, don't panic</li>
<li><em>(assert)</em> Clarify what action is causing a positional that doesn't set values which is especially useful for derive users</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/clap-rs/clap/blob/master/CHANGELOG.md">clap's changelog</a>.</em></p>
<blockquote>
<h2>[4.4.5] - 2023-09-25</h2>
<h3>Fixes</h3>
<ul>
<li><em>(parser)</em> When inferring subcommand <code>name</code> or <code>long_flag</code>, allow ambiguous-looking matches that unambiguously map back to the same command</li>
<li><em>(parser)</em> When inferring subcommand <code>long_flag</code>, don't panic</li>
<li><em>(assert)</em> Clarify what action is causing a positional that doesn't set values which is especially useful for derive users</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/clap-rs/clap/commit/c298f6a52c0095df95a664807cff1f51e6a5b0f9"><code>c298f6a</code></a> chore: Release</li>
<li><a href="https://github.com/clap-rs/clap/commit/463d6c52af9a4d0acddea669bedc0f4ea6ae2258"><code>463d6c5</code></a> docs: Update changelog</li>
<li><a href="https://github.com/clap-rs/clap/commit/3ac44040efeaa6e2b738bad9c2b3ecf46e8017d2"><code>3ac4404</code></a> Merge pull request <a href="https://redirect.github.com/clap-rs/clap/issues/5025">#5025</a> from SUPERCILEX/resolve-alias-conflicts</li>
<li><a href="https://github.com/clap-rs/clap/commit/a76789eb8bb1f07dc8d63414ef0e39e6f0abab29"><code>a76789e</code></a> fix: Make long subcommand flag inference consistent</li>
<li><a href="https://github.com/clap-rs/clap/commit/c2b8ec3bd3f0a99ef1ed723deac12cf1dfb781ca"><code>c2b8ec3</code></a> fix: Resolve conflicting name inference if from aliases</li>
<li><a href="https://github.com/clap-rs/clap/commit/e5c6993cca8398ed16514e1a065b832ff04d6120"><code>e5c6993</code></a> test: Long flags inference</li>
<li><a href="https://github.com/clap-rs/clap/commit/0d9b14fa6e9ccb09fea5d7c93477b7fca8d51bdc"><code>0d9b14f</code></a> Merge pull request <a href="https://redirect.github.com/clap-rs/clap/issues/5136">#5136</a> from epage/panic</li>
<li><a href="https://github.com/clap-rs/clap/commit/221177b9cbd8fc711536e6dcb8f948dae5c1b172"><code>221177b</code></a> fix(assert): Call out the action in positional assert</li>
<li><a href="https://github.com/clap-rs/clap/commit/cb2d2bcf0753b4538b0ca08789dff47b1e445ba0"><code>cb2d2bc</code></a> chore: Update from '_rust/main'</li>
<li><a href="https://github.com/clap-rs/clap/commit/4173c8f4767296f76a6eb96d70b7ca6c13bb38bd"><code>4173c8f</code></a> chore(ci): Don't set patch for MSRV</li>
<li>Additional commits viewable in <a href="https://github.com/clap-rs/clap/compare/v4.4.4...v4.4.5">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=clap&package-manager=cargo&previous-version=4.4.4&new-version=4.4.5)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-26 08:42_

---

_Comment by @github-actions[bot] on 2023-09-26 09:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @MichaReiser on 2023-09-26 11:02_

---

_Closed by @MichaReiser on 2023-09-26 11:02_

---

_Branch deleted on 2023-09-26 11:02_

---
