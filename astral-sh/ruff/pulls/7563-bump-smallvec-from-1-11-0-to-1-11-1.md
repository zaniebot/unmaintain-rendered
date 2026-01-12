```yaml
number: 7563
title: Bump smallvec from 1.11.0 to 1.11.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/smallvec-1.11.1
created_at: 2023-09-21T08:56:38Z
updated_at: 2023-09-21T09:53:28Z
url: https://github.com/astral-sh/ruff/pull/7563
synced_at: 2026-01-12T15:55:24Z
```

# Bump smallvec from 1.11.0 to 1.11.1

---

_@dependabot_

Bumps [smallvec](https://github.com/servo/rust-smallvec) from 1.11.0 to 1.11.1.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/servo/rust-smallvec/releases">smallvec's releases</a>.</em></p>
<blockquote>
<h2>v1.11.1</h2>
<h2>What's Changed</h2>
<ul>
<li>Annotate SmallVec::insert_from_slice with <code>#[inline]</code> by <a href="https://github.com/sffc"><code>@​sffc</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/312">servo/rust-smallvec#312</a></li>
<li>Micro-optimize <code>push</code> and <code>insert</code> b <a href="https://github.com/emilio"><code>@​emilio</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/315">servo/rust-smallvec#315</a></li>
<li>Add <code>--generate-link-to-definition</code> option when building on docs.rs by <a href="https://github.com/GuillaumeGomez"><code>@​GuillaumeGomez</code></a> in <a href="https://redirect.github.com/servo/rust-smallvec/pull/304">servo/rust-smallvec#304</a></li>
<li>Testing fixes by <a href="https://github.com/mrobinson"><code>@​mrobinson</code></a> and <a href="https://github.com/waywardmonkeys"><code>@​waywardmonkeys</code></a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/mrobinson"><code>@​mrobinson</code></a> made their first contribution in <a href="https://redirect.github.com/servo/rust-smallvec/pull/303">servo/rust-smallvec#303</a></li>
<li><a href="https://github.com/GuillaumeGomez"><code>@​GuillaumeGomez</code></a> made their first contribution in <a href="https://redirect.github.com/servo/rust-smallvec/pull/304">servo/rust-smallvec#304</a></li>
<li><a href="https://github.com/sffc"><code>@​sffc</code></a> made their first contribution in <a href="https://redirect.github.com/servo/rust-smallvec/pull/312">servo/rust-smallvec#312</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/servo/rust-smallvec/compare/v1.11.0...v1.11.1">https://github.com/servo/rust-smallvec/compare/v1.11.0...v1.11.1</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/servo/rust-smallvec/commit/4f9cd3b17a63261b0f9344f5323e6f0d96767aee"><code>4f9cd3b</code></a> Bump version.</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/1d8f3d7175722c53b04d367e1a3c586daa7b35f5"><code>1d8f3d7</code></a> Micro-optimize push() and insert().</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/5d060d1042174128e32b2c4bf17ff6021e00fc08"><code>5d060d1</code></a> Merge pull request <a href="https://redirect.github.com/servo/rust-smallvec/issues/312">#312</a> from sffc/insert-inline</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/201b1e93b0d0f71993b61cddc781e4381c9513ed"><code>201b1e9</code></a> Add <code>#[inline]</code> on <code>insert_from_slice</code> to reduce code size</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/f5bd2af1c453012cbd0471173d3fe0024a108041"><code>f5bd2af</code></a> ci: Fix running fuzz.</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/65bf3c831bd57c937afe66e2e88630e3f081028d"><code>65bf3c8</code></a> ci: Run CI on push to <code>v1</code> branch.</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/21aec4e8e0ff3ef8d1a8cdd2b130fb0575ac1852"><code>21aec4e</code></a> ci: Update toolchain action.</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/8a49851620a1559311b0942f7a16e675a409c06b"><code>8a49851</code></a> Add <code>--generate-link-to-definition</code> option when building on docs.rs (<a href="https://redirect.github.com/servo/rust-smallvec/issues/304">#304</a>)</li>
<li><a href="https://github.com/servo/rust-smallvec/commit/4dda12d728440fc0cbad3d1bf380bd1b23b49bae"><code>4dda12d</code></a> Enable GitHub merge queue (<a href="https://redirect.github.com/servo/rust-smallvec/issues/303">#303</a>)</li>
<li>See full diff in <a href="https://github.com/servo/rust-smallvec/compare/v1.11.0...v1.11.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=smallvec&package-manager=cargo&previous-version=1.11.0&new-version=1.11.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-09-21 08:56_

---

_Comment by @codspeed-hq[bot] on 2023-09-21 09:07_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/smallvec-1.11.1)

### Merging #7563 will **degrade performances by 2.83%**

<sub>Comparing <code>dependabot/cargo/smallvec-1.11.1</code> (a8c29f1) with <code>main</code> (f8f1cd5)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dependabot/cargo/smallvec-1.11.1)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dependabot/cargo/smallvec-1.11.1` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[numpy/ctypeslib.py]` | 18.2 ms | 18.7 ms | -2.83% |


---

_Comment by @github-actions[bot] on 2023-09-21 09:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @MichaReiser on 2023-09-21 09:53_

---

_Closed by @MichaReiser on 2023-09-21 09:53_

---

_Branch deleted on 2023-09-21 09:53_

---
