```yaml
number: 7758
title: Bump memchr from 2.6.3 to 2.6.4
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/memchr-2.6.4
created_at: 2023-10-02T08:46:44Z
updated_at: 2023-10-02T13:50:23Z
url: https://github.com/astral-sh/ruff/pull/7758
synced_at: 2026-01-12T02:39:10Z
```

# Bump memchr from 2.6.3 to 2.6.4

---

_Pull request opened by @dependabot on 2023-10-02 08:46_

Bumps [memchr](https://github.com/BurntSushi/memchr) from 2.6.3 to 2.6.4.
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/BurntSushi/memchr/commit/ce7b8e606410f6c81a63f45abb24c5b5aab5d741"><code>ce7b8e6</code></a> 2.6.4</li>
<li><a href="https://github.com/BurntSushi/memchr/commit/ca9ea654198e50f6f7574bbc644aa100534e2a2b"><code>ca9ea65</code></a> cargo: update description</li>
<li>See full diff in <a href="https://github.com/BurntSushi/memchr/compare/2.6.3...2.6.4">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=memchr&package-manager=cargo&previous-version=2.6.3&new-version=2.6.4)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-02 08:46_

---

_Comment by @codspeed-hq[bot] on 2023-10-02 09:06_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/memchr-2.6.4)

### Merging #7758 will **degrade performances by 2.4%**

<sub>Comparing <code>dependabot/cargo/memchr-2.6.4</code> (9db9485) with <code>main</code> (f70e8a7)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/memchr-2.6.4)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/memchr-2.6.4` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 34.7 ms | 35.6 ms | -2.4% |


---

_Comment by @github-actions[bot] on 2023-10-02 09:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-10-02 13:50_

---

_Closed by @charliermarsh on 2023-10-02 13:50_

---

_Branch deleted on 2023-10-02 13:50_

---
