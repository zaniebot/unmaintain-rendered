```yaml
number: 7565
title: Bump insta from 1.31.0 to 1.32.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/insta-1.32.0
created_at: 2023-09-21T08:58:30Z
updated_at: 2023-09-21T09:53:16Z
url: https://github.com/astral-sh/ruff/pull/7565
synced_at: 2026-01-12T15:55:24Z
```

# Bump insta from 1.31.0 to 1.32.0

---

_@dependabot_

Bumps [insta](https://github.com/mitsuhiko/insta) from 1.31.0 to 1.32.0.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/mitsuhiko/insta/blob/master/CHANGELOG.md">insta's changelog</a>.</em></p>
<blockquote>
<h2>1.32.0</h2>
<ul>
<li>Added <code>--profile</code> parameter support to <code>cargo insta test</code>.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/mitsuhiko/insta/commit/845ee36cf4fc425d3e84cf51dba86117ae0175b8"><code>845ee36</code></a> 1.32.0</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/5b34e7eaade37332ddd6b6e2cd5bd1926a7c9c91"><code>5b34e7e</code></a> Added changlog entry</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/bee0ff41afd70846b7671bf4eda421b8f47f9f7b"><code>bee0ff4</code></a> cargo-insta: reduce visibility of all items (<a href="https://redirect.github.com/mitsuhiko/insta/issues/407">#407</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/6e22d670924f7357d27baf59b2b6ce3064668bbe"><code>6e22d67</code></a> cargo-insta: edit reject message (<a href="https://redirect.github.com/mitsuhiko/insta/issues/404">#404</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/7e9315ede74393e32567a72d5b8f1f3e808521cf"><code>7e9315e</code></a> test-stable requires 1.61</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/84cddf9c7045a2769c6b06dd0259640c0ba2df5c"><code>84cddf9</code></a> Fix incorrect --profile command</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/20656ad9027c0f30450947e771d16c02d34a2fef"><code>20656ad</code></a> cargo-insta: allow passing --profile (<a href="https://redirect.github.com/mitsuhiko/insta/issues/402">#402</a>)</li>
<li>See full diff in <a href="https://github.com/mitsuhiko/insta/compare/1.31.0...1.32.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=insta&package-manager=cargo&previous-version=1.31.0&new-version=1.32.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-21 08:58_

---

_Comment by @codspeed-hq[bot] on 2023-09-21 09:10_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/insta-1.32.0)

### Merging #7565 will **degrade performances by 2.82%**

<sub>Comparing <code>dependabot/cargo/insta-1.32.0</code> (b3bd35f) with <code>main</code> (f8f1cd5)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/insta-1.32.0)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/insta-1.32.0` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[numpy/ctypeslib.py]` | 18.2 ms | 18.7 ms | -2.82% |


---

_Comment by @github-actions[bot] on 2023-09-21 09:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @MichaReiser on 2023-09-21 09:53_

---

_Closed by @MichaReiser on 2023-09-21 09:53_

---

_Branch deleted on 2023-09-21 09:53_

---
