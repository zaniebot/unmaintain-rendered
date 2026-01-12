```yaml
number: 7484
title: Bump test-case from 3.1.0 to 3.2.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/test-case-3.2.1
created_at: 2023-09-18T08:44:18Z
updated_at: 2023-09-18T09:31:42Z
url: https://github.com/astral-sh/ruff/pull/7484
synced_at: 2026-01-12T15:55:24Z
```

# Bump test-case from 3.1.0 to 3.2.1

---

_@dependabot_

Bumps [test-case](https://github.com/frondeus/test-case) from 3.1.0 to 3.2.1.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/frondeus/test-case/releases">test-case's releases</a>.</em></p>
<blockquote>
<h2>Test Case - v3.2.1</h2>
<ul>
<li>Update <code>syn</code> dependency</li>
<li>Ensure that <code>test-case</code> targets latest <code>core</code> and <code>macros</code> subcrates.</li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/frondeus/test-case/compare/v3.2.0...v3.2.1">https://github.com/frondeus/test-case/compare/v3.2.0...v3.2.1</a></p>
<h2>Test Case - v3.2.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Retain allow attribute in test_case macro by <a href="https://github.com/JohnPeel"><code>@​JohnPeel</code></a> in <a href="https://redirect.github.com/frondeus/test-case/pull/127">frondeus/test-case#127</a></li>
<li>Add test_matrix macro by <a href="https://github.com/overhacked"><code>@​overhacked</code></a> in <a href="https://redirect.github.com/frondeus/test-case/pull/128">frondeus/test-case#128</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/JohnPeel"><code>@​JohnPeel</code></a> made their first contribution in <a href="https://redirect.github.com/frondeus/test-case/pull/127">frondeus/test-case#127</a></li>
<li><a href="https://github.com/overhacked"><code>@​overhacked</code></a> made their first contribution in <a href="https://redirect.github.com/frondeus/test-case/pull/128">frondeus/test-case#128</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/frondeus/test-case/compare/v3.1.0...v3.2.0">https://github.com/frondeus/test-case/compare/v3.1.0...v3.2.0</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/frondeus/test-case/blob/master/CHANGELOG.md">test-case's changelog</a>.</em></p>
<blockquote>
<h2>3.2.1</h2>
<h3>Changes</h3>
<ul>
<li>Update <code>syn</code> dependency to 2.0</li>
<li>Ensure that <code>test-case</code> selects correct version of it's <code>core</code> and <code>macros</code> subcrates</li>
</ul>
<h2>3.2.0</h2>
<h3>New features</h3>
<ul>
<li>Add <code>test_matrix</code> macro: generates test cases from Cartesian product of possible test function argument values (<a href="https://redirect.github.com/frondeus/test-case/issues/128">#128</a>)</li>
</ul>
<h3>Changes</h3>
<ul>
<li>Retain <code>allow</code> attributes on test functions (<a href="https://redirect.github.com/frondeus/test-case/issues/127">#127</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/frondeus/test-case/commit/17db57af8d190e559533de5c5ee784d7ab811a05"><code>17db57a</code></a> Bump version to 3.2.1</li>
<li><a href="https://github.com/frondeus/test-case/commit/ca22d205a6ac2e1ea40eaf917e3c92f3aeace215"><code>ca22d20</code></a> chore: update syn dependency</li>
<li><a href="https://github.com/frondeus/test-case/commit/6f8ae5c6fcf12567010efe950151441ec09ff9a9"><code>6f8ae5c</code></a> Bump version to 3.2.0</li>
<li><a href="https://github.com/frondeus/test-case/commit/04935d4e320c3aef55bd4ffc71348cae25b5ce39"><code>04935d4</code></a> test: exclude some logs from Windows tests</li>
<li><a href="https://github.com/frondeus/test-case/commit/410d2c6dd839e66ec61e30bf3d12f04c819cfba9"><code>410d2c6</code></a> test: simplify acceptance tests</li>
<li><a href="https://github.com/frondeus/test-case/commit/f202f473ae42ade31cdd6b91a0e10b347cc2d6a6"><code>f202f47</code></a> chore: fix clippy and tests</li>
<li><a href="https://github.com/frondeus/test-case/commit/ada5d825c46f3bcd4ad2329bf18f66c85b48417c"><code>ada5d82</code></a> feat: Add test_matrix macro (<a href="https://redirect.github.com/frondeus/test-case/issues/128">#128</a>)</li>
<li><a href="https://github.com/frondeus/test-case/commit/dee411a3ff3aaf35b0b36fb4365b7144119c4d43"><code>dee411a</code></a> feat: retain allow attribute in test_case macro (<a href="https://redirect.github.com/frondeus/test-case/issues/127">#127</a>)</li>
<li><a href="https://github.com/frondeus/test-case/commit/dfef0a8cf7957888fedaaf3dcf592785cc4e2a78"><code>dfef0a8</code></a> chore: update tests with newest stable</li>
<li>See full diff in <a href="https://github.com/frondeus/test-case/compare/v3.1.0...v3.2.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=test-case&package-manager=cargo&previous-version=3.1.0&new-version=3.2.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-18 08:44_

---

_Comment by @github-actions[bot] on 2023-09-18 09:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @MichaReiser on 2023-09-18 09:31_

---

_Closed by @MichaReiser on 2023-09-18 09:31_

---

_Branch deleted on 2023-09-18 09:31_

---
