```yaml
number: 8346
title: Bump serde from 1.0.188 to 1.0.190
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/serde-1.0.190
created_at: 2023-10-30T08:30:41Z
updated_at: 2023-10-30T09:06:11Z
url: https://github.com/astral-sh/ruff/pull/8346
synced_at: 2026-01-10T23:40:55Z
```

# Bump serde from 1.0.188 to 1.0.190

---

_Pull request opened by @dependabot on 2023-10-30 08:30_

Bumps [serde](https://github.com/serde-rs/serde) from 1.0.188 to 1.0.190.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/serde-rs/serde/releases">serde's releases</a>.</em></p>
<blockquote>
<h2>v1.0.190</h2>
<ul>
<li>Preserve NaN sign when deserializing f32 from f64 or vice versa (<a href="https://redirect.github.com/serde-rs/serde/issues/2637">#2637</a>)</li>
</ul>
<h2>v1.0.189</h2>
<ul>
<li>Fix &quot;cannot infer type&quot; error when internally tagged enum contains untagged variant (<a href="https://redirect.github.com/serde-rs/serde/issues/2613">#2613</a>, thanks <a href="https://github.com/ahl"><code>@​ahl</code></a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/serde-rs/serde/commit/edb1a586d83ceaf4a61980ce890bc77a02e00787"><code>edb1a58</code></a> Release 1.0.190</li>
<li><a href="https://github.com/serde-rs/serde/commit/11c2917040057e91451df06a18109242926c1c9b"><code>11c2917</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/serde/issues/2637">#2637</a> from dtolnay/nansign</li>
<li><a href="https://github.com/serde-rs/serde/commit/6ba9c12ff6309114fd22d56a9f4acf32284352f4"><code>6ba9c12</code></a> Float copysign does not exist in libcore yet</li>
<li><a href="https://github.com/serde-rs/serde/commit/d2fcc346b9f8f8a61d05474b4e95198788c05ed0"><code>d2fcc34</code></a> Ensure f32 deserialized from f64 and vice versa preserve NaN sign</li>
<li><a href="https://github.com/serde-rs/serde/commit/a091a07aa2a70ce45e7c61146b4387630239eae8"><code>a091a07</code></a> Add float NaN tests</li>
<li><a href="https://github.com/serde-rs/serde/commit/bb4135cae830e27135c5ba0a3f23512634dfd7c6"><code>bb4135c</code></a> Fix unused imports</li>
<li><a href="https://github.com/serde-rs/serde/commit/8de84b7ca32afd5037326025ed8fcfd06ed9db5f"><code>8de84b7</code></a> Resolve get_first clippy lint</li>
<li><a href="https://github.com/serde-rs/serde/commit/9cdf33202977df68289a42b1ba30885b6b2abe44"><code>9cdf332</code></a> Remove 'remember to update' reminder from Cargo.toml</li>
<li><a href="https://github.com/serde-rs/serde/commit/e94fc65f01e60d91c6dd9bafc0601c4ae4a77739"><code>e94fc65</code></a> Release 1.0.189</li>
<li><a href="https://github.com/serde-rs/serde/commit/b908487476633a397685a5a5f5265d4e0a4cd3a4"><code>b908487</code></a> Remove double nesting of first_attempt</li>
<li>Additional commits viewable in <a href="https://github.com/serde-rs/serde/compare/v1.0.188...v1.0.190">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=serde&package-manager=cargo&previous-version=1.0.188&new-version=1.0.190)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-30 08:30_

---

_Comment by @github-actions[bot] on 2023-10-30 08:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_Merged by @MichaReiser on 2023-10-30 09:06_

---

_Closed by @MichaReiser on 2023-10-30 09:06_

---

_Branch deleted on 2023-10-30 09:06_

---
