```yaml
number: 7510
title: Bump schemars from 0.8.13 to 0.8.15
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/schemars-0.8.15
created_at: 2023-09-19T08:11:34Z
updated_at: 2023-09-19T09:08:23Z
url: https://github.com/astral-sh/ruff/pull/7510
synced_at: 2026-01-12T02:39:10Z
```

# Bump schemars from 0.8.13 to 0.8.15

---

_Pull request opened by @dependabot on 2023-09-19 08:11_

Bumps [schemars](https://github.com/GREsau/schemars) from 0.8.13 to 0.8.15.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/GREsau/schemars/releases">schemars's releases</a>.</em></p>
<blockquote>
<h2>v0.8.15</h2>
<h3>Added:</h3>
<ul>
<li>Implement <code>JsonSchema</code> for <code>BigDecimal</code> from <code>bigdecimal</code> 0.4 (<a href="https://redirect.github.com/GREsau/schemars/pull/237">GREsau/schemars#237</a>)</li>
</ul>
<h2>v0.8.14</h2>
<h3>Added:</h3>
<ul>
<li>Add <code>#[schemars(inner(...)]</code> attribute to specify schema for array items (<a href="https://redirect.github.com/GREsau/schemars/pull/234">GREsau/schemars#234</a>)</li>
</ul>
<h3>Changed:</h3>
<ul>
<li>New optional associated function on <code>JsonSchema</code> trait: <code>schema_id()</code>, which is similar to <code>schema_name()</code>, but does not have to be human-readable, and defaults to the type name including module path. This allows schemars to differentiate between types with the same name in different modules/crates (<a href="https://redirect.github.com/GREsau/schemars/issues/62">GREsau/schemars#62</a> / <a href="https://redirect.github.com/GREsau/schemars/pull/247">GREsau/schemars#247</a>)</li>
</ul>
<h3>Fixed:</h3>
<ul>
<li>Schemas for <code>rust_decimal::Decimal</code> and <code>bigdecimal::BigDecimal</code> now match how those types are serialized by default, i.e. as numeric strings (<a href="https://redirect.github.com/GREsau/schemars/pull/248">GREsau/schemars#248</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/GREsau/schemars/blob/master/CHANGELOG.md">schemars's changelog</a>.</em></p>
<blockquote>
<h2>[0.8.15] - 2023-09-17</h2>
<h3>Added:</h3>
<ul>
<li>Implement <code>JsonSchema</code> for <code>BigDecimal</code> from <code>bigdecimal</code> 0.4 (<a href="https://redirect.github.com/GREsau/schemars/pull/237">GREsau/schemars#237</a>)</li>
</ul>
<h2>[0.8.14] - 2023-09-17</h2>
<h3>Added:</h3>
<ul>
<li>Add <code>#[schemars(inner(...)]</code> attribute to specify schema for array items (<a href="https://redirect.github.com/GREsau/schemars/pull/234">GREsau/schemars#234</a>)</li>
</ul>
<h3>Changed:</h3>
<ul>
<li>New optional associated function on <code>JsonSchema</code> trait: <code>schema_id()</code>, which is similar to <code>schema_name()</code>, but does not have to be human-readable, and defaults to the type name including module path. This allows schemars to differentiate between types with the same name in different modules/crates (<a href="https://redirect.github.com/GREsau/schemars/issues/62">GREsau/schemars#62</a> / <a href="https://redirect.github.com/GREsau/schemars/pull/247">GREsau/schemars#247</a>)</li>
</ul>
<h3>Fixed:</h3>
<ul>
<li>Schemas for <code>rust_decimal::Decimal</code> and <code>bigdecimal::BigDecimal</code> now match how those types are serialized by default, i.e. as numeric strings (<a href="https://redirect.github.com/GREsau/schemars/pull/248">GREsau/schemars#248</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/GREsau/schemars/commit/9415fcb57b85f12e07afeb1dd16184bab0e26a84"><code>9415fcb</code></a> v0.8.15</li>
<li><a href="https://github.com/GREsau/schemars/commit/a8d723342fb9f215120619253ea3342cfb80e46b"><code>a8d7233</code></a> Cleanup and test updates for bigdecimal04</li>
<li><a href="https://github.com/GREsau/schemars/commit/cc28738f414619a2cae8b3ddd999c7f7d2a98992"><code>cc28738</code></a> Support bigdecimal 0.4 (<a href="https://redirect.github.com/GREsau/schemars/issues/237">#237</a>)</li>
<li><a href="https://github.com/GREsau/schemars/commit/0084f1a6551a9a6f45d25d06d07d0c74a9624d1f"><code>0084f1a</code></a> v0.8.14</li>
<li><a href="https://github.com/GREsau/schemars/commit/6e3248f83054c2544a9ed2ff5441e4fd0b322241"><code>6e3248f</code></a> Fix bad merge</li>
<li><a href="https://github.com/GREsau/schemars/commit/a136277f606fe61ffe7d050e8480cf8149d82cf5"><code>a136277</code></a> Update docs for <code>schema_id()</code></li>
<li><a href="https://github.com/GREsau/schemars/commit/28258ae99bd1cfa7af426fd30cec9256f0b2e58d"><code>28258ae</code></a> Update changelog</li>
<li><a href="https://github.com/GREsau/schemars/commit/342b2dff332f5d97ae47e2ff0f582e7a70c01f27"><code>342b2df</code></a> Add schema_id(), handles different types with the same name (<a href="https://redirect.github.com/GREsau/schemars/issues/247">#247</a>)</li>
<li><a href="https://github.com/GREsau/schemars/commit/53bb51cb2589fd18be6957797786896e359d483b"><code>53bb51c</code></a> Update changelog</li>
<li><a href="https://github.com/GREsau/schemars/commit/db1dd4703926ed91ea2435b41bb51fa2c72976ff"><code>db1dd47</code></a> Fix schemas for bigdecimal/rust_decimal (<a href="https://redirect.github.com/GREsau/schemars/issues/248">#248</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/GREsau/schemars/compare/v0.8.13...v0.8.15">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=schemars&package-manager=cargo&previous-version=0.8.13&new-version=0.8.15)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-19 08:11_

---

_Comment by @codspeed-hq[bot] on 2023-09-19 08:24_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/schemars-0.8.15)

### Merging #7510 will **degrade performances by 2.41%**

<sub>Comparing <code>dependabot/cargo/schemars-0.8.15</code> (34dee65) with <code>main</code> (37b7d0f)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/schemars-0.8.15)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/schemars-0.8.15` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 34.2 ms | 35.1 ms | -2.41% |


---

_Comment by @github-actions[bot] on 2023-09-19 08:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @MichaReiser on 2023-09-19 09:08_

---

_Closed by @MichaReiser on 2023-09-19 09:08_

---

_Branch deleted on 2023-09-19 09:08_

---
