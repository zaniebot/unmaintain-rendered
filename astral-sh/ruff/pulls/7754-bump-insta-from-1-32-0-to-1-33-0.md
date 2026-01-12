```yaml
number: 7754
title: Bump insta from 1.32.0 to 1.33.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/insta-1.33.0
created_at: 2023-10-02T08:43:18Z
updated_at: 2023-10-02T13:49:44Z
url: https://github.com/astral-sh/ruff/pull/7754
synced_at: 2026-01-12T15:55:24Z
```

# Bump insta from 1.32.0 to 1.33.0

---

_@dependabot_

Bumps [insta](https://github.com/mitsuhiko/insta) from 1.32.0 to 1.33.0.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/mitsuhiko/insta/blob/master/CHANGELOG.md">insta's changelog</a>.</em></p>
<blockquote>
<h2>1.33.0</h2>
<ul>
<li>Added <code>--all-targets</code> parameter support to <code>cargo insta test</code>.  (<a href="https://redirect.github.com/mitsuhiko/insta/issues/408">#408</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/mitsuhiko/insta/commit/9623b3a99ad14075086f93531528e465d2288bf0"><code>9623b3a</code></a> 1.33.0</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/c09cd866a7e97094757e8751b35e7318d3dfb217"><code>c09cd86</code></a> Added changelog</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/58a225f98fdd5cb4cc9e33907fd67caf2c377591"><code>58a225f</code></a> cargo-insta: support --all-targets (<a href="https://redirect.github.com/mitsuhiko/insta/issues/408">#408</a>)</li>
<li>See full diff in <a href="https://github.com/mitsuhiko/insta/compare/1.32.0...1.33.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=insta&package-manager=cargo&previous-version=1.32.0&new-version=1.33.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-02 08:43_

---

_Comment by @codspeed-hq[bot] on 2023-10-02 08:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/insta-1.33.0)

### Merging #7754 will **improve performances by 2.02%**

<sub>Comparing <code>dependabot/cargo/insta-1.33.0</code> (6134d72) with <code>main</code> (1df8101)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/insta-1.33.0` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 19 ms | 18.6 ms | +2.02% |


---

_Comment by @github-actions[bot] on 2023-10-02 09:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-10-02 13:49_

---

_Closed by @charliermarsh on 2023-10-02 13:49_

---

_Branch deleted on 2023-10-02 13:49_

---
