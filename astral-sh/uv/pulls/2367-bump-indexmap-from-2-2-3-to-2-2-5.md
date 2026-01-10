```yaml
number: 2367
title: Bump indexmap from 2.2.3 to 2.2.5
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/indexmap-2.2.5
created_at: 2024-03-11T21:33:43Z
updated_at: 2024-03-11T21:43:35Z
url: https://github.com/astral-sh/uv/pull/2367
synced_at: 2026-01-10T14:49:08Z
```

# Bump indexmap from 2.2.3 to 2.2.5

---

_Pull request opened by @dependabot on 2024-03-11 21:33_

Bumps [indexmap](https://github.com/indexmap-rs/indexmap) from 2.2.3 to 2.2.5.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/indexmap-rs/indexmap/blob/master/RELEASES.md">indexmap's changelog</a>.</em></p>
<blockquote>
<h2>2.2.5</h2>
<ul>
<li>Added optional <code>borsh</code> serialization support.</li>
</ul>
<h2>2.2.4</h2>
<ul>
<li>Added an <code>insert_sorted</code> method on <code>IndexMap</code>, <code>IndexSet</code>, and <code>VacantEntry</code>.</li>
<li>Avoid hashing for lookups in single-entry maps.</li>
<li>Limit preallocated memory in <code>serde</code> deserializers.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/184fe4bfcba20e21242c924220170b98be157d45"><code>184fe4b</code></a> Merge pull request <a href="https://redirect.github.com/indexmap-rs/indexmap/issues/320">#320</a> from cuviper/release-2.2.5</li>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/5d7bd5e6e3946678375ead7e72714162f3ad9a5e"><code>5d7bd5e</code></a> Release 2.2.5</li>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/c934ace47e4eb8a9cbd2f09a9d70e4e123b5cf0f"><code>c934ace</code></a> Merge pull request <a href="https://redirect.github.com/indexmap-rs/indexmap/issues/313">#313</a> from heliaxdev/heliax/borsh-support</li>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/b81a4d25e1703dfd574aee4551460ba571e1c870"><code>b81a4d2</code></a> Use S for the BuildHasher parameter</li>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/32793f1211fdf88b475e909259470011edfba152"><code>32793f1</code></a> Don't require BuildHasher in BorshSerialize</li>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/b8b1f52599d7fee3458bad36110d8a495ddee443"><code>b8b1f52</code></a> ci: reduce features on MSRV</li>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/6ad3e42dc34a0f902090aa69e0aa0ce82508a4db"><code>6ad3e42</code></a> Include <code>borsh</code> in CI workflow</li>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/c610e14ea059dc8e99359edf5fa4a1f53274a73b"><code>c610e14</code></a> Add <code>borsh</code> serialization roundtrip tests</li>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/0804a16e24d2c8e0ed5e466df195fc358bb91b48"><code>0804a16</code></a> Implement <code>borsh</code> serialization routines</li>
<li><a href="https://github.com/indexmap-rs/indexmap/commit/ae38b913022251230c6b5fbe3df3706e300449ad"><code>ae38b91</code></a> Add <code>borsh</code> dep to Cargo manifest</li>
<li>Additional commits viewable in <a href="https://github.com/indexmap-rs/indexmap/compare/2.2.3...2.2.5">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=indexmap&package-manager=cargo&previous-version=2.2.3&new-version=2.2.5)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-03-11 21:33_

---

_Merged by @zanieb on 2024-03-11 21:43_

---

_Closed by @zanieb on 2024-03-11 21:43_

---

_Branch deleted on 2024-03-11 21:43_

---
