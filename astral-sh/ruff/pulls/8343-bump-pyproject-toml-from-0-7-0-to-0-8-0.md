```yaml
number: 8343
title: Bump pyproject-toml from 0.7.0 to 0.8.0
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/pyproject-toml-0.8.0
created_at: 2023-10-30T08:29:51Z
updated_at: 2023-10-30T10:05:49Z
url: https://github.com/astral-sh/ruff/pull/8343
synced_at: 2026-01-10T23:40:55Z
```

# Bump pyproject-toml from 0.7.0 to 0.8.0

---

_Pull request opened by @dependabot on 2023-10-30 08:29_

Bumps [pyproject-toml](https://github.com/PyO3/pyproject-toml-rs) from 0.7.0 to 0.8.0.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/PyO3/pyproject-toml-rs/blob/main/Changelog.md">pyproject-toml's changelog</a>.</em></p>
<blockquote>
<h2>0.8.0</h2>
<ul>
<li>The <code>build_system</code> table is now optional. There are many projects that use pyproject.toml for tool configuration without specifying a build backend, which this change reflects.</li>
</ul>
<h2>0.6.0</h2>
<ul>
<li>Update to latest <a href="https://peps.python.org/pep-0639">PEP 639</a> draft. The <code>license</code> key is now an enum that can either be an SPDX identifier or the previous table form, which accepting PEP 639 would deprecate. The previous implementation of a <code>project.license-expression</code> key in <code>pyproject.toml</code> has been <a href="https://peps.python.org/pep-0639/#define-a-new-top-level-license-expression-key">removed</a>.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/PyO3/pyproject-toml-rs/commit/06b924d59334d25237a8f1e5a2563490d0857fad"><code>06b924d</code></a> Merge pull request <a href="https://redirect.github.com/PyO3/pyproject-toml-rs/issues/15">#15</a> from PyO3/optional-build_system</li>
<li><a href="https://github.com/PyO3/pyproject-toml-rs/commit/458b4236cef0324310323b7a514e30b42f551c82"><code>458b423</code></a> Make the <code>build_system</code> table optional</li>
<li><a href="https://github.com/PyO3/pyproject-toml-rs/commit/f0134caabc94525ac9ad775fcc5546a5d870efb2"><code>f0134ca</code></a> Merge pull request <a href="https://redirect.github.com/PyO3/pyproject-toml-rs/issues/14">#14</a> from PyO3/dependabot/github_actions/actions/checkout-4</li>
<li><a href="https://github.com/PyO3/pyproject-toml-rs/commit/b4b958b02fcafd62309c97c6544e4a181b5296e7"><code>b4b958b</code></a> Bump actions/checkout from 3 to 4</li>
<li>See full diff in <a href="https://github.com/PyO3/pyproject-toml-rs/compare/v0.7.0...v0.8.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=pyproject-toml&package-manager=cargo&previous-version=0.7.0&new-version=0.8.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-30 08:29_

---

_Comment by @github-actions[bot] on 2023-10-30 08:48_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a39f783c1026aacbbfbaa9767c153bf7b197b9d9/pandas/io/parquet.py#L466'>pandas/io/parquet.py:466:1:</a> E999 SyntaxError: unexpected EOF while parsing
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

✅ ecosystem check detected no format changes.



---

_Review requested from @konstin by @MichaReiser on 2023-10-30 09:06_

---

_Closed by @konstin on 2023-10-30 10:05_

---

_Comment by @dependabot[bot] on 2023-10-30 10:05_

OK, I won't notify you again about this release, but will get in touch when a new version is available. If you'd rather skip all updates until the next major or minor version, let me know by commenting `@dependabot ignore this major version` or `@dependabot ignore this minor version`. You can also ignore all major, minor, or patch releases for a dependency by adding an [`ignore` condition](https://docs.github.com/en/code-security/supply-chain-security/configuration-options-for-dependency-updates#ignore) with the desired `update_types` to your config file.

If you change your mind, just re-open this PR and I'll resolve any conflicts on it.

---

_Branch deleted on 2023-10-30 10:05_

---
