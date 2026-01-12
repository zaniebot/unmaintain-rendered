```yaml
number: 8130
title: Bump serde_with from 3.3.0 to 3.4.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/serde_with-3.4.0
created_at: 2023-10-23T08:53:41Z
updated_at: 2023-10-23T09:12:13Z
url: https://github.com/astral-sh/ruff/pull/8130
synced_at: 2026-01-12T02:32:41Z
```

# Bump serde_with from 3.3.0 to 3.4.0

---

_Pull request opened by @dependabot on 2023-10-23 08:53_

Bumps [serde_with](https://github.com/jonasbb/serde_with) from 3.3.0 to 3.4.0.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/jonasbb/serde_with/releases">serde_with's releases</a>.</em></p>
<blockquote>
<h2>serde_with v3.4.0</h2>
<ul>
<li>
<p>Lower minimum required serde version to 1.0.152 (<a href="https://redirect.github.com/jonasbb/serde_with/issues/653">#653</a>)
Thanks to <a href="https://github.com/banool"><code>@​banool</code></a> for submitting the PR.</p>
<p>This allows people that have a problem with 1.0.153 to still use <code>serde_with</code>.</p>
</li>
<li>
<p>Add support for <code>core::ops::Bound</code> (<a href="https://redirect.github.com/jonasbb/serde_with/issues/655">#655</a>)
Thanks to <a href="https://github.com/qsantos"><code>@​qsantos</code></a> for submitting the PR.</p>
</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/jonasbb/serde_with/commit/1e8e4e75398c6a9de29386473ae7c3157be031c2"><code>1e8e4e7</code></a> Merge <a href="https://redirect.github.com/jonasbb/serde_with/issues/657">#657</a></li>
<li><a href="https://github.com/jonasbb/serde_with/commit/151f2fbbb9522090743a87435dbfc05d6730b285"><code>151f2fb</code></a> Bump version to 3.4.0</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/e1657b4318eadd4d28e74607debdb02bcaf777bc"><code>e1657b4</code></a> Merge <a href="https://redirect.github.com/jonasbb/serde_with/issues/655">#655</a></li>
<li><a href="https://github.com/jonasbb/serde_with/commit/f55d41d89fda6daccfa064319624347ae32a8ebe"><code>f55d41d</code></a> Merge <a href="https://redirect.github.com/jonasbb/serde_with/issues/653">#653</a></li>
<li><a href="https://github.com/jonasbb/serde_with/commit/e1441335c3f18d7be1614844b49c87b0c0ce77a9"><code>e144133</code></a> Add support for core::ops::Bound</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/81507a2f81f8b5b9ccf2020f93cada8fcf970071"><code>81507a2</code></a> Decrease minimum serde version to 1.0.152</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/04bf8f35425dd81b4d6107ee9de9292ef128bc02"><code>04bf8f3</code></a> Merge <a href="https://redirect.github.com/jonasbb/serde_with/issues/652">#652</a></li>
<li><a href="https://github.com/jonasbb/serde_with/commit/0eb6b17f07c1c4caa62b430b17acdb1101a2181b"><code>0eb6b17</code></a> Mention serde_as in docstring for 'apply'</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/3813ed66b12f1bf33c2d93bbfb6373b71c05c977"><code>3813ed6</code></a> Merge <a href="https://redirect.github.com/jonasbb/serde_with/issues/646">#646</a></li>
<li><a href="https://github.com/jonasbb/serde_with/commit/14b6ea4ef0732eb903f9108c2f5a504f6070ce50"><code>14b6ea4</code></a> Tarpaulin now requires the --out argument in lowercase</li>
<li>Additional commits viewable in <a href="https://github.com/jonasbb/serde_with/compare/v3.3.0...v3.4.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=serde_with&package-manager=cargo&previous-version=3.3.0&new-version=3.4.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-23 08:53_

---

_Merged by @MichaReiser on 2023-10-23 09:05_

---

_Closed by @MichaReiser on 2023-10-23 09:05_

---

_Branch deleted on 2023-10-23 09:05_

---

_Comment by @github-actions[bot] on 2023-10-23 09:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
