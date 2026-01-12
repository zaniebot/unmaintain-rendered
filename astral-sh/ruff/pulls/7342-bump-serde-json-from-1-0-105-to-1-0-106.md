```yaml
number: 7342
title: Bump serde_json from 1.0.105 to 1.0.106
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/serde_json-1.0.106
created_at: 2023-09-13T15:56:25Z
updated_at: 2023-09-13T16:41:57Z
url: https://github.com/astral-sh/ruff/pull/7342
synced_at: 2026-01-12T15:55:23Z
```

# Bump serde_json from 1.0.105 to 1.0.106

---

_@dependabot_

Bumps [serde_json](https://github.com/serde-rs/json) from 1.0.105 to 1.0.106.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/serde-rs/json/releases">serde_json's releases</a>.</em></p>
<blockquote>
<h2>v1.0.106</h2>
<ul>
<li>Add <code>Value::as_number</code> accessor (<a href="https://redirect.github.com/serde-rs/json/issues/1069">#1069</a>, thanks <a href="https://github.com/chanced"><code>@​chanced</code></a>)</li>
<li>Add <code>Number::as_str</code> accessor under &quot;arbitrary_precision&quot; feature (<a href="https://redirect.github.com/serde-rs/json/issues/1067">#1067</a>, thanks <a href="https://github.com/chanced"><code>@​chanced</code></a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/serde-rs/json/commit/45f10ec816e3f2765ac08f7ca73752326b0475d7"><code>45f10ec</code></a> Release 1.0.106</li>
<li><a href="https://github.com/serde-rs/json/commit/f346308cda4cd5a7dae0327a757fe41f064a2161"><code>f346308</code></a> Elaborate on documentation of Number::as_str</li>
<li><a href="https://github.com/serde-rs/json/commit/f16cad635d2aadb2ef6610e765f49eaabc1c9bf1"><code>f16cad6</code></a> Add cfg banner to documentation of Number::as_str</li>
<li><a href="https://github.com/serde-rs/json/commit/fc8dd13aa284d255b79504ed4ee15a27aba6228f"><code>fc8dd13</code></a> Touch up PR 1067</li>
<li><a href="https://github.com/serde-rs/json/commit/028b6439309e1fa910e380a24194c273313d0ba3"><code>028b643</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/json/issues/1067">#1067</a> from chanced/add-as_str-to-number</li>
<li><a href="https://github.com/serde-rs/json/commit/db75c22990f58c77d380c9dc02caa43e42b5b098"><code>db75c22</code></a> Fix unintended u8 link inferred by intra doc link</li>
<li><a href="https://github.com/serde-rs/json/commit/11b603cf07b6298cdec9803ef0b32085e43c335c"><code>11b603c</code></a> Resolve rustdoc::redundant_explicit_links lint</li>
<li><a href="https://github.com/serde-rs/json/commit/95c5d6c8be64634d9bf408ef9679003e6b6f0220"><code>95c5d6c</code></a> Fix documentation typo from PR 1069</li>
<li><a href="https://github.com/serde-rs/json/commit/5a39516161ea9efd7957e2b6c6c8fa76792026a9"><code>5a39516</code></a> Reorder Value::as_number after is_number</li>
<li><a href="https://github.com/serde-rs/json/commit/6a5fef919092c11a9a4eb827ac51001761795fe0"><code>6a5fef9</code></a> Wrap as_number documentation to 80 columns</li>
<li>Additional commits viewable in <a href="https://github.com/serde-rs/json/compare/v1.0.105...v1.0.106">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=serde_json&package-manager=cargo&previous-version=1.0.105&new-version=1.0.106)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-13 15:56_

---

_Comment by @zanieb on 2023-09-13 15:57_

Weird title given

```diff
- serde_json = { version = "1.0.93" }
+ serde_json = { version = "1.0.106" }
```

---

_Comment by @MichaReiser on 2023-09-13 16:08_

> Weird title given
> 
> ```diff
> - serde_json = { version = "1.0.93" }
> + serde_json = { version = "1.0.106" }
> ```

It's because 1.0.105 is in the lockfile

---

_Merged by @MichaReiser on 2023-09-13 16:13_

---

_Closed by @MichaReiser on 2023-09-13 16:13_

---

_Branch deleted on 2023-09-13 16:13_

---

_Comment by @github-actions[bot] on 2023-09-13 16:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
