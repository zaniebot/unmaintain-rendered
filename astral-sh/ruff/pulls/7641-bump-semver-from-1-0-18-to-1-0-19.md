```yaml
number: 7641
title: Bump semver from 1.0.18 to 1.0.19
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/semver-1.0.19
created_at: 2023-09-25T08:55:13Z
updated_at: 2023-09-25T09:15:32Z
url: https://github.com/astral-sh/ruff/pull/7641
synced_at: 2026-01-12T15:55:24Z
```

# Bump semver from 1.0.18 to 1.0.19

---

_@dependabot_

Bumps [semver](https://github.com/dtolnay/semver) from 1.0.18 to 1.0.19.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/semver/releases">semver's releases</a>.</em></p>
<blockquote>
<h2>1.0.19</h2>
<ul>
<li>Improve test coverage (<a href="https://redirect.github.com/dtolnay/semver/issues/299">#299</a>, thanks <a href="https://github.com/CXWorks"><code>@​CXWorks</code></a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/semver/commit/72a6b5a722160befddbd210f4a3d41fdab60110f"><code>72a6b5a</code></a> Release 1.0.19</li>
<li><a href="https://github.com/dtolnay/semver/commit/83abc7f7b32fba9b369fdd984730206c1d89cc92"><code>83abc7f</code></a> Relocate comparator parse testing</li>
<li><a href="https://github.com/dtolnay/semver/commit/2d34e8fe60334d4525c70462b74e85508d2529e6"><code>2d34e8f</code></a> Touch up PR 299 test cases</li>
<li><a href="https://github.com/dtolnay/semver/commit/5e093296a46d982396cecad25c46c3c4e0889e38"><code>5e09329</code></a> More comprehensible excessive version comparator test</li>
<li><a href="https://github.com/dtolnay/semver/commit/473209fe33d2186906945a868a6801671c479479"><code>473209f</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/semver/issues/299">#299</a> from CXWorks/master</li>
<li><a href="https://github.com/dtolnay/semver/commit/cb0790143be4df9c389d90803ddb61af893696f6"><code>cb07901</code></a> Update actions/checkout@v3 -&gt; v4</li>
<li><a href="https://github.com/dtolnay/semver/commit/fff3f407577d48be83ac39a62aec7f9f76d5c741"><code>fff3f40</code></a> Revert &quot;Temporarily disable -Zrandomize-layout due to rustc ICE&quot;</li>
<li><a href="https://github.com/dtolnay/semver/commit/2399480676a3bbd6dda26b4632813071871c44a1"><code>2399480</code></a> Temporarily disable -Zrandomize-layout due to rustc ICE</li>
<li><a href="https://github.com/dtolnay/semver/commit/b074ea0465d7ff55beb10816d8b3aa8e8c976793"><code>b074ea0</code></a> Resolve incorrect_partial_ord_impl_on_ord_type clippy lint</li>
<li><a href="https://github.com/dtolnay/semver/commit/2d5031362f87b2427bea3fbe71b5cff054646739"><code>2d50313</code></a> Add missed test cases</li>
<li>See full diff in <a href="https://github.com/dtolnay/semver/compare/1.0.18...1.0.19">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=semver&package-manager=cargo&previous-version=1.0.18&new-version=1.0.19)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-25 08:55_

---

_Comment by @codspeed-hq[bot] on 2023-09-25 09:08_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/semver-1.0.19)

### Merging #7641 will **improve performances by 2.31%**

<sub>Comparing <code>dependabot/cargo/semver-1.0.19</code> (6710c9c) with <code>main</code> (39ddad7)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/semver-1.0.19` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 94.1 ms | 92 ms | +2.31% |


---

_Merged by @MichaReiser on 2023-09-25 09:09_

---

_Closed by @MichaReiser on 2023-09-25 09:09_

---

_Branch deleted on 2023-09-25 09:09_

---

_Comment by @github-actions[bot] on 2023-09-25 09:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
