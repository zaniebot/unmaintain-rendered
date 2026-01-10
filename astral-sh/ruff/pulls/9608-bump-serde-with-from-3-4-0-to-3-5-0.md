```yaml
number: 9608
title: Bump serde_with from 3.4.0 to 3.5.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/serde_with-3.5.0
created_at: 2024-01-22T08:16:02Z
updated_at: 2024-01-22T08:52:53Z
url: https://github.com/astral-sh/ruff/pull/9608
synced_at: 2026-01-10T22:57:09Z
```

# Bump serde_with from 3.4.0 to 3.5.0

---

_Pull request opened by @dependabot on 2024-01-22 08:16_

Bumps [serde_with](https://github.com/jonasbb/serde_with) from 3.4.0 to 3.5.0.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/jonasbb/serde_with/releases">serde_with's releases</a>.</em></p>
<blockquote>
<h2>serde_with v3.5.0</h2>
<h3>Added</h3>
<ul>
<li>Support for <code>schemars</code> integration added by <a href="https://github.com/swlynch99"><code>@​swlynch99</code></a> (<a href="https://redirect.github.com/jonasbb/serde_with/issues/666">#666</a>)
The support uses a new <code>Schema</code> top-level item which implements <code>JsonSchema</code>
The <code>serde_as</code> macro can now detect <code>schemars</code> usage and emits matching annotations for all fields with <code>serde_as</code> attribute.
Many types of this crate come already with support for the <code>schemars</code>, but support is not complete and will be extended over time.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/jonasbb/serde_with/commit/6ecde3cb98e187d4e0be68f3fa17cd2c02230c92"><code>6ecde3c</code></a> Merge pull request <a href="https://redirect.github.com/jonasbb/serde_with/issues/681">#681</a> from jonasbb:bump-version</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/dda736721f30ef159cab1318f2e1684ffc55080c"><code>dda7367</code></a> Bump to v3.5.0</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/cc06ba25238e61a0ae5e56ccce52b649e009af9f"><code>cc06ba2</code></a> Merge pull request <a href="https://redirect.github.com/jonasbb/serde_with/issues/680">#680</a> from jonasbb/cleanups</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/4f1a6195e285e6e18906c3b587a1b926787ecc15"><code>4f1a619</code></a> Light spell checking</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/442beff82ca137035f2db3d73bdd3c277ff4c15a"><code>442beff</code></a> More rustdoc cleanups</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/0f5ad6d840097d43825454e428f06b60ac6c32d3"><code>0f5ad6d</code></a> Sort entries in Cargo.toml</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/2a59333a398079af1b40af5e3dfe869b8ac04cba"><code>2a59333</code></a> Fix some rustdoc warnings</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/655380425233fb0ac6c69819835c9123f9e45f71"><code>6553804</code></a> Merge pull request <a href="https://redirect.github.com/jonasbb/serde_with/issues/679">#679</a> from swlynch99/schemars-ext</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/b0ee3ab712333792676d3ede1be0367ad57fe4d1"><code>b0ee3ab</code></a> Make serde_with schemars support extensible</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/170b6e1b703f480ca159a834d4e845c39bb76a34"><code>170b6e1</code></a> Merge pull request <a href="https://redirect.github.com/jonasbb/serde_with/issues/676">#676</a> from swlynch99/schemars-fwd</li>
<li>Additional commits viewable in <a href="https://github.com/jonasbb/serde_with/compare/v3.4.0...v3.5.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=serde_with&package-manager=cargo&previous-version=3.4.0&new-version=3.5.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-22 08:16_

---

_Merged by @MichaReiser on 2024-01-22 08:39_

---

_Closed by @MichaReiser on 2024-01-22 08:39_

---

_Branch deleted on 2024-01-22 08:39_

---

_Comment by @github-actions[bot] on 2024-01-22 08:52_

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
