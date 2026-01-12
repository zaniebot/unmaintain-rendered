```yaml
number: 9524
title: Bump js-sys from 0.3.66 to 0.3.67
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/js-sys-0.3.67
created_at: 2024-01-15T08:29:42Z
updated_at: 2024-01-15T09:29:19Z
url: https://github.com/astral-sh/ruff/pull/9524
synced_at: 2026-01-12T15:55:29Z
```

# Bump js-sys from 0.3.66 to 0.3.67

---

_@dependabot_

Bumps [js-sys](https://github.com/rustwasm/wasm-bindgen) from 0.3.66 to 0.3.67.
<details>
<summary>Commits</summary>
<ul>
<li>See full diff in <a href="https://github.com/rustwasm/wasm-bindgen/commits">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=js-sys&package-manager=cargo&previous-version=0.3.66&new-version=0.3.67)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-15 08:29_

---

_Comment by @codspeed-hq[bot] on 2024-01-15 08:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/js-sys-0.3.67)

### Merging #9524 will **degrade performances by 4.35%**

<sub>Comparing <code>dependabot/cargo/js-sys-0.3.67</code> (cc6e29b) with <code>main</code> (6183b8e)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/js-sys-0.3.67` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @github-actions[bot] on 2024-01-15 08:48_

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

_Merged by @MichaReiser on 2024-01-15 09:29_

---

_Closed by @MichaReiser on 2024-01-15 09:29_

---

_Branch deleted on 2024-01-15 09:29_

---
