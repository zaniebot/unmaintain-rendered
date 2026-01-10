```yaml
number: 9432
title: Bump anyhow from 1.0.76 to 1.0.79
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/anyhow-1.0.79
created_at: 2024-01-08T08:50:25Z
updated_at: 2024-01-08T09:09:11Z
url: https://github.com/astral-sh/ruff/pull/9432
synced_at: 2026-01-10T22:57:09Z
```

# Bump anyhow from 1.0.76 to 1.0.79

---

_Pull request opened by @dependabot on 2024-01-08 08:50_

Bumps [anyhow](https://github.com/dtolnay/anyhow) from 1.0.76 to 1.0.79.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/anyhow/releases">anyhow's releases</a>.</em></p>
<blockquote>
<h2>1.0.79</h2>
<ul>
<li>Work around improperly cached build script result by sccache (<a href="https://redirect.github.com/dtolnay/anyhow/issues/340">#340</a>)</li>
</ul>
<h2>1.0.78</h2>
<ul>
<li>Reduce spurious rebuilds under RustRover IDE when using a nightly toolchain (<a href="https://redirect.github.com/dtolnay/anyhow/issues/337">#337</a>)</li>
</ul>
<h2>1.0.77</h2>
<ul>
<li>Make <code>anyhow::Error::backtrace</code> available on stable Rust compilers 1.65+ (<a href="https://redirect.github.com/dtolnay/anyhow/issues/293">#293</a>, thanks <a href="https://github.com/LukasKalbertodt"><code>@​LukasKalbertodt</code></a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/anyhow/commit/71ab53dd2e89ff816bebaa452ad5a968f4c4105d"><code>71ab53d</code></a> Release 1.0.79</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/60705a53cefdc387b72c20cbab88aa4ffcf4b38e"><code>60705a5</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/anyhow/issues/340">#340</a> from dtolnay/depinfo</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/17e252bfdf4f7a2dff8e788ef7deab326682bbba"><code>17e252b</code></a> Include env-dep:RUSTC_BOOTSTRAP in dep-info for sccache</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/04774c089409597e4859ed256ef68eeb60ba81a7"><code>04774c0</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/anyhow/issues/338">#338</a> from dtolnay/nightlyci</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/1fd290c222f28ade7f5ee6f049a47b93a51b1f97"><code>1fd290c</code></a> Make CI verify that error_generic_member_access works in latest nightly</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/ee414707be0d4b6f0f109d0a85e4d10651e4aa2c"><code>ee41470</code></a> RUSTC must be set by Cargo for build script</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/38c79ef242fdfb124e499a58d917286ed1d4cad5"><code>38c79ef</code></a> Release 1.0.78</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/ded2295ff5cd6e759b30b12cd127ffe0bc658f0f"><code>ded2295</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/anyhow/issues/337">#337</a> from dtolnay/bootstrap</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/ae45b672c944e87cec570cf29389cfe0c0c2599a"><code>ae45b67</code></a> Do not rebuild on RUSTC_BOOTSTRAP changes on nightly compiler</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/2d32366f581f82256208181fa2c02285888fd4df"><code>2d32366</code></a> Update crate name used for build script probe</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/anyhow/compare/1.0.76...1.0.79">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=anyhow&package-manager=cargo&previous-version=1.0.76&new-version=1.0.79)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-08 08:50_

---

_Merged by @MichaReiser on 2024-01-08 08:56_

---

_Closed by @MichaReiser on 2024-01-08 08:56_

---

_Branch deleted on 2024-01-08 08:56_

---

_Comment by @github-actions[bot] on 2024-01-08 09:09_

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
