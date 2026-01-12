```yaml
number: 9342
title: Bump clap from 4.4.7 to 4.4.12
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/clap-4.4.12
created_at: 2024-01-01T08:46:47Z
updated_at: 2024-01-02T01:57:42Z
url: https://github.com/astral-sh/ruff/pull/9342
synced_at: 2026-01-12T15:55:28Z
```

# Bump clap from 4.4.7 to 4.4.12

---

_@dependabot_

Bumps [clap](https://github.com/clap-rs/clap) from 4.4.7 to 4.4.12.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/clap-rs/clap/releases">clap's releases</a>.</em></p>
<blockquote>
<h2>v4.4.12</h2>
<h2>[4.4.12] - 2023-12-28</h2>
<h3>Performance</h3>
<ul>
<li>Only ask <code>TypedValueParser</code> for possible values if needed</li>
</ul>
<h2>v4.4.11</h2>
<h2>[4.4.11] - 2023-12-04</h2>
<h3>Features</h3>
<ul>
<li>Add <code>Command::mut_group</code></li>
</ul>
<h2>v4.4.10</h2>
<h2>[4.4.10] - 2023-11-28</h2>
<h3>Documentation</h3>
<ul>
<li>Link out to changelog</li>
<li>Cross link derive's attribute reference to derive tutorial</li>
</ul>
<h2>v4.4.9</h2>
<h2>[4.4.9] - 2023-11-27</h2>
<h3>Fixes</h3>
<ul>
<li><em>(help)</em> Show correct <code>Command::about</code> under flattened headings</li>
<li><em>(help)</em> Respect <code>hide</code> when flattening subcommands</li>
</ul>
<h2>v4.4.8</h2>
<h2>[4.4.8] - 2023-11-10</h2>
<h3>Features</h3>
<ul>
<li>Add <code>Command::flatten_help</code> to allow <code>git stash -h</code> like help for subcommands</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/clap-rs/clap/blob/master/CHANGELOG.md">clap's changelog</a>.</em></p>
<blockquote>
<h2>[4.4.12] - 2023-12-28</h2>
<h3>Performance</h3>
<ul>
<li>Only ask <code>TypedValueParser</code> for possible values if needed</li>
</ul>
<h2>[4.4.11] - 2023-12-04</h2>
<h3>Features</h3>
<ul>
<li>Add <code>Command::mut_group</code></li>
</ul>
<h2>[4.4.10] - 2023-11-28</h2>
<h3>Documentation</h3>
<ul>
<li>Link out to changelog</li>
<li>Cross link derive's attribute reference to derive tutorial</li>
</ul>
<h2>[4.4.9] - 2023-11-27</h2>
<h3>Fixes</h3>
<ul>
<li><em>(help)</em> Show correct <code>Command::about</code> under flattened headings</li>
<li><em>(help)</em> Respect <code>hide</code> when flattening subcommands</li>
</ul>
<h2>[4.4.8] - 2023-11-10</h2>
<h3>Features</h3>
<ul>
<li>Add <code>Command::flatten_help</code> to allow <code>git stash -h</code> like help for subcommands</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/clap-rs/clap/commit/6d601e6f312857d78f47741268f1748840484d4a"><code>6d601e6</code></a> chore: Release</li>
<li><a href="https://github.com/clap-rs/clap/commit/048e7f0fbc4f8108894e4307af768c8d332a0576"><code>048e7f0</code></a> docs: Update changelog</li>
<li><a href="https://github.com/clap-rs/clap/commit/53f5b820988c1c03f0d2696fc144857ad461edc1"><code>53f5b82</code></a> Merge pull request <a href="https://redirect.github.com/clap-rs/clap/issues/5267">#5267</a> from vermiculus/sa/avoid-pv-expansion-in-help</li>
<li><a href="https://github.com/clap-rs/clap/commit/05cd057978db743a65fb5fde33213af752d064e7"><code>05cd057</code></a> perf: Avoid retrieving possible_values unless used</li>
<li><a href="https://github.com/clap-rs/clap/commit/29208083b0598ba7d1b80e79821c0ba3eb2342ce"><code>2920808</code></a> test: Update snapshots</li>
<li><a href="https://github.com/clap-rs/clap/commit/28763ebb6d8714f6ca588bf477729040e0e760b0"><code>28763eb</code></a> chore: Release</li>
<li><a href="https://github.com/clap-rs/clap/commit/ace7bb5b4570b030f7c2d0fa91e0afaaac1b0030"><code>ace7bb5</code></a> docs(complete): Update changelog</li>
<li><a href="https://github.com/clap-rs/clap/commit/76beca4d4d42a817bbb578f23553c64ded2aea97"><code>76beca4</code></a> docs(complete): Polish API reference for dynamic</li>
<li><a href="https://github.com/clap-rs/clap/commit/3630e582d3ec59912b8e9369c38687695ddb1f43"><code>3630e58</code></a> Merge pull request <a href="https://redirect.github.com/clap-rs/clap/issues/5273">#5273</a> from epage/docsrs</li>
<li><a href="https://github.com/clap-rs/clap/commit/3724b9e2e4c2a2e69337b6d809949b246d3fef39"><code>3724b9e</code></a> docs: Include more content on docs.rs</li>
<li>Additional commits viewable in <a href="https://github.com/clap-rs/clap/compare/v4.4.7...v4.4.12">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=clap&package-manager=cargo&previous-version=4.4.7&new-version=4.4.12)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-01 08:46_

---

_Comment by @github-actions[bot] on 2024-01-01 09:03_

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

_Merged by @charliermarsh on 2024-01-02 01:57_

---

_Closed by @charliermarsh on 2024-01-02 01:57_

---

_Branch deleted on 2024-01-02 01:57_

---
