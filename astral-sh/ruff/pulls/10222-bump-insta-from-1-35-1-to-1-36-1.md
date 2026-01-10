```yaml
number: 10222
title: Bump insta from 1.35.1 to 1.36.1
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/insta-1.36.1
created_at: 2024-03-04T08:27:38Z
updated_at: 2024-03-04T09:10:50Z
url: https://github.com/astral-sh/ruff/pull/10222
synced_at: 2026-01-10T22:47:01Z
```

# Bump insta from 1.35.1 to 1.36.1

---

_Pull request opened by @dependabot on 2024-03-04 08:27_

Bumps [insta](https://github.com/mitsuhiko/insta) from 1.35.1 to 1.36.1.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/mitsuhiko/insta/blob/master/CHANGELOG.md">insta's changelog</a>.</em></p>
<blockquote>
<h2>1.36.1</h2>
<ul>
<li>Fix an ownership issue introduced in 1.36 with snapshot assertions.  <a href="https://redirect.github.com/mitsuhiko/insta/issues/453">#453</a></li>
</ul>
<h2>1.36.0</h2>
<ul>
<li>
<p>Deprecate <code>INSTA_FORCE_UPDATE_SNAPSHOTS</code> env-var for <code>INSTA_FORCE_UPDATE</code>.
The latter was documented, the former was implemented.  <a href="https://redirect.github.com/mitsuhiko/insta/issues/449">#449</a></p>
</li>
<li>
<p>Add <code>require_full_match</code> option.  <a href="https://redirect.github.com/mitsuhiko/insta/issues/448">#448</a></p>
</li>
<li>
<p>Deprecate <code>assert_display_snapshot!</code>.  <a href="https://redirect.github.com/mitsuhiko/insta/issues/385">#385</a></p>
</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/mitsuhiko/insta/commit/3cb9934aa30d16ba0dc9355413a01d299e340015"><code>3cb9934</code></a> 1.36.1</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/28bc0d57aa03494e49f91b089520ef0f161cbbbb"><code>28bc0d5</code></a> Fix ownership issue (<a href="https://redirect.github.com/mitsuhiko/insta/issues/453">#453</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/23c045d277665d0fa03cb8953ef3c30799d7ae65"><code>23c045d</code></a> 1.36.0</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/1fc3983fbcf400c018bd633149d1615ae0fbe5e4"><code>1fc3983</code></a> Better missing file errors (<a href="https://redirect.github.com/mitsuhiko/insta/issues/451">#451</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/efa031cf307c26c5293c2a0e712429d899140e58"><code>efa031c</code></a> Ensure cargo insta sets old envvar</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/780c343f5e8a0891ac75d01307b7ca55f7988b8c"><code>780c343</code></a> Restore cargo-insta compat with older insta (<a href="https://redirect.github.com/mitsuhiko/insta/issues/452">#452</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/0d4bab1fcd1d17890ee43f825c0280506f4b8d4b"><code>0d4bab1</code></a> Update changelog</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/8221931101fcade23feb6033c0955c6d26103992"><code>8221931</code></a> Deprecate <code>assert_display_snapshot</code>, refactor macros (<a href="https://redirect.github.com/mitsuhiko/insta/issues/385">#385</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/686de754aa613ccc820541ce1d7f95ab1a0095ee"><code>686de75</code></a> Add <code>require_full_match</code> option (<a href="https://redirect.github.com/mitsuhiko/insta/issues/448">#448</a>)</li>
<li><a href="https://github.com/mitsuhiko/insta/commit/189290fbb15d6898cdc502d7505b158ed093eeef"><code>189290f</code></a> Align env var with docs (<a href="https://redirect.github.com/mitsuhiko/insta/issues/449">#449</a>)</li>
<li>See full diff in <a href="https://github.com/mitsuhiko/insta/compare/1.35.1...1.36.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=insta&package-manager=cargo&previous-version=1.35.1&new-version=1.36.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-03-04 08:27_

---

_Closed by @MichaReiser on 2024-03-04 09:10_

---

_Comment by @dependabot[bot] on 2024-03-04 09:10_

OK, I won't notify you again about this release, but will get in touch when a new version is available. If you'd rather skip all updates until the next major or minor version, let me know by commenting `@dependabot ignore this major version` or `@dependabot ignore this minor version`. You can also ignore all major, minor, or patch releases for a dependency by adding an [`ignore` condition](https://docs.github.com/en/code-security/supply-chain-security/configuration-options-for-dependency-updates#ignore) with the desired `update_types` to your config file.

If you change your mind, just re-open this PR and I'll resolve any conflicts on it.

---

_Branch deleted on 2024-03-04 09:10_

---
