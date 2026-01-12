```yaml
number: 7640
title: Bump wild from 2.1.0 to 2.2.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/wild-2.2.0
created_at: 2023-09-25T08:54:42Z
updated_at: 2023-09-25T09:12:32Z
url: https://github.com/astral-sh/ruff/pull/7640
synced_at: 2026-01-12T15:55:24Z
```

# Bump wild from 2.1.0 to 2.2.0

---

_@dependabot_

Bumps [wild](https://gitlab.com/kornelski/wild) from 2.1.0 to 2.2.0.
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://gitlab.com/kornelski/wild/commit/646db43ed766a60b833cdd2de2de980d0d6f8994"><code>646db43</code></a> Avoid allocating pattern</li>
<li><a href="https://gitlab.com/kornelski/wild/commit/2207deaecec84d8fc9f9576d7110ef6052105040"><code>2207dea</code></a> Optimize parser</li>
<li><a href="https://gitlab.com/kornelski/wild/commit/1d3b8d7997fc7956e4a022adb7c30082bebe91c7"><code>1d3b8d7</code></a> Add option to always glob args</li>
<li><a href="https://gitlab.com/kornelski/wild/commit/5a423c9e69542b9e73e523eed9af8c876e91cc9c"><code>5a423c9</code></a> Bump</li>
<li><a href="https://gitlab.com/kornelski/wild/commit/952567474c3d35c1e2f34fa575e242e71bde8d36"><code>9525674</code></a> Cold fmt</li>
<li>See full diff in <a href="https://gitlab.com/kornelski/wild/compare/v2.1.0...v2.2.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=wild&package-manager=cargo&previous-version=2.1.0&new-version=2.2.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-25 08:54_

---

_Merged by @MichaReiser on 2023-09-25 09:06_

---

_Closed by @MichaReiser on 2023-09-25 09:06_

---

_Branch deleted on 2023-09-25 09:06_

---

_Comment by @codspeed-hq[bot] on 2023-09-25 09:08_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/wild-2.2.0)

### Merging #7640 will **improve performances by 2.31%**

<sub>Comparing <code>dependabot/cargo/wild-2.2.0</code> (9735f00) with <code>main</code> (39ddad7)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/wild-2.2.0` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 94.1 ms | 92 ms | +2.31% |


---

_Comment by @github-actions[bot] on 2023-09-25 09:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
