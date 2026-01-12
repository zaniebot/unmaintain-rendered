```yaml
number: 9087
title: Bump serde from 1.0.190 to 1.0.193
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/serde-1.0.193
created_at: 2023-12-11T08:44:16Z
updated_at: 2023-12-11T09:01:13Z
url: https://github.com/astral-sh/ruff/pull/9087
synced_at: 2026-01-12T15:55:27Z
```

# Bump serde from 1.0.190 to 1.0.193

---

_@dependabot_

Bumps [serde](https://github.com/serde-rs/serde) from 1.0.190 to 1.0.193.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/serde-rs/serde/releases">serde's releases</a>.</em></p>
<blockquote>
<h2>v1.0.193</h2>
<ul>
<li>Fix field names used for the deserialization of <code>RangeFrom</code> and <code>RangeTo</code> (<a href="https://redirect.github.com/serde-rs/serde/issues/2653">#2653</a>, <a href="https://redirect.github.com/serde-rs/serde/issues/2654">#2654</a>, <a href="https://redirect.github.com/serde-rs/serde/issues/2655">#2655</a>, thanks <a href="https://github.com/emilbonnek"><code>@​emilbonnek</code></a>)</li>
</ul>
<h2>v1.0.192</h2>
<ul>
<li>Allow internal tag field in untagged variant (<a href="https://redirect.github.com/serde-rs/serde/issues/2646">#2646</a>, thanks <a href="https://github.com/robsdedude"><code>@​robsdedude</code></a>)</li>
</ul>
<h2>v1.0.191</h2>
<ul>
<li>Documentation improvements</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/serde-rs/serde/commit/44613c7d0190dbb5ecd2d5ec19c636f45b7488cc"><code>44613c7</code></a> Release 1.0.193</li>
<li><a href="https://github.com/serde-rs/serde/commit/c706281df3c8d50dba1763f19c856df2746eba6c"><code>c706281</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/serde/issues/2655">#2655</a> from dtolnay/rangestartend</li>
<li><a href="https://github.com/serde-rs/serde/commit/65d75b8fe3105f00ab2e01537d568d4587167582"><code>65d75b8</code></a> Add RangeFrom and RangeTo tests</li>
<li><a href="https://github.com/serde-rs/serde/commit/332b0cba40bcbcc7a6b23a9706277c54791a9856"><code>332b0cb</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/serde/issues/2654">#2654</a> from dtolnay/rangestartend</li>
<li><a href="https://github.com/serde-rs/serde/commit/8c4af412969086bc8f54fdc2a079d373632e0a03"><code>8c4af41</code></a> Fix more RangeFrom / RangeEnd mixups</li>
<li><a href="https://github.com/serde-rs/serde/commit/24a78f071b22ae491eec4127be696ac255b9b5d3"><code>24a78f0</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/serde/issues/2653">#2653</a> from emilbonnek/fix/range-to-from-de-mixup</li>
<li><a href="https://github.com/serde-rs/serde/commit/c91c33436d7aaef7472ebc18b734ddc9b5bd11fa"><code>c91c334</code></a> Fix Range{From,To} deserialize mixup</li>
<li><a href="https://github.com/serde-rs/serde/commit/2083f43a287cac8302009fda5bbe41518dd83209"><code>2083f43</code></a> Update ui test suite to nightly-2023-11-19</li>
<li><a href="https://github.com/serde-rs/serde/commit/4676abdc9e6bbbddfb33a00ce8d7e81e92f01120"><code>4676abd</code></a> Release 1.0.192</li>
<li><a href="https://github.com/serde-rs/serde/commit/35700eb23e21d8cb198ef4a422ddad13b855ce3b"><code>35700eb</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/serde/issues/2646">#2646</a> from robsdedude/fix/2643/allow-tag-field-in-untagged</li>
<li>Additional commits viewable in <a href="https://github.com/serde-rs/serde/compare/v1.0.190...v1.0.193">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=serde&package-manager=cargo&previous-version=1.0.190&new-version=1.0.193)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-11 08:44_

---

_Merged by @konstin on 2023-12-11 08:50_

---

_Closed by @konstin on 2023-12-11 08:50_

---

_Branch deleted on 2023-12-11 08:50_

---

_Comment by @github-actions[bot] on 2023-12-11 09:01_

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
