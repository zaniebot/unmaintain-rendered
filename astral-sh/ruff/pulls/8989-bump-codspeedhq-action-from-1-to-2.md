```yaml
number: 8989
title: Bump CodSpeedHQ/action from 1 to 2
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/github_actions/CodSpeedHQ/action-2
created_at: 2023-12-04T08:22:34Z
updated_at: 2023-12-05T00:00:41Z
url: https://github.com/astral-sh/ruff/pull/8989
synced_at: 2026-01-10T23:40:55Z
```

# Bump CodSpeedHQ/action from 1 to 2

---

_Pull request opened by @dependabot on 2023-12-04 08:22_

Bumps [CodSpeedHQ/action](https://github.com/codspeedhq/action) from 1 to 2.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/codspeedhq/action/releases">CodSpeedHQ/action's releases</a>.</em></p>
<blockquote>
<h2>v2.0.0</h2>
<p><strong>Full Changelog</strong>: <a href="https://github.com/CodSpeedHQ/action/compare/v1...v2.0.0">https://github.com/CodSpeedHQ/action/compare/v1...v2.0.0</a></p>
<p>This major update greatly simplifies the action and moves all the core logic to the <a href="https://github.com/CodSpeedHQ/runner">https://github.com/CodSpeedHQ/runner</a> repo.</p>
<p>Updating is as easy as changing <code>CodSpeedHQ/action@v1</code> to <code>CodSpeedHQ/action@v2</code>.</p>
<h2>What's changed</h2>
<ul>
<li>feat: integrate the native runner with a composite action by <a href="https://github.com/art049"><code>@​art049</code></a> and <a href="https://github.com/adriencaccia"><code>@​adriencaccia</code></a> in <a href="https://redirect.github.com/CodSpeedHQ/action/pull/86">CodSpeedHQ/action#86</a></li>
</ul>
<h3>Breaking changes</h3>
<ul>
<li><code>upload_url</code> input is now <code>upload-url</code></li>
<li><code>pytest-codspeed</code> is no longer installed automatically by this action</li>
</ul>
<h2>v1.8.1</h2>
<h2>What's Changed</h2>
<ul>
<li>fix: update apt registry before installing valgrind by <a href="https://github.com/adriencaccia"><code>@​adriencaccia</code></a> in <a href="https://redirect.github.com/CodSpeedHQ/action/pull/80">CodSpeedHQ/action#80</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/CodSpeedHQ/action/compare/v1.8.0...v1.8.1">https://github.com/CodSpeedHQ/action/compare/v1.8.0...v1.8.1</a></p>
<h2>v1.8.0</h2>
<h2>What's Changed</h2>
<h3>Added</h3>
<ul>
<li>Add the <code>working-directory</code> input parameter by <a href="https://github.com/art049"><code>@​art049</code></a> in <a href="https://redirect.github.com/CodSpeedHQ/action/pull/77">CodSpeedHQ/action#77</a></li>
</ul>
<h3>Internals</h3>
<ul>
<li>Update documentation examples by <a href="https://github.com/adriencaccia"><code>@​adriencaccia</code></a> in <a href="https://redirect.github.com/CodSpeedHQ/action/pull/76">CodSpeedHQ/action#76</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/CodSpeedHQ/action/compare/v1.7.1...v1.8.0">https://github.com/CodSpeedHQ/action/compare/v1.7.1...v1.8.0</a></p>
<h2>v1.7.1</h2>
<h2>🎉 What's Changed</h2>
<ul>
<li>Prepare for release on GitHub Action Marketplace</li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/CodSpeedHQ/action/compare/v1.7.0...v1.7.1">https://github.com/CodSpeedHQ/action/compare/v1.7.0...v1.7.1</a></p>
<h2>v1.7.0</h2>
<h2>🎉 What's Changed</h2>
<ul>
<li>
<p>A <a href="https://github.com/CodSpeedHQ/valgrind-codspeed">fork of valgrind</a> is now used, allowing us to <a href="https://docs.codspeed.io/features/trace-generation/">generate execution traces</a> with Node and Python.</p>
</li>
<li>
<p>V8 flags previously set as an environment variable <code>CODSPEED_V8_FLAGS</code> are now handled directly by the integration library and injected to Nodejs through our <a href="https://github.com/CodSpeedHQ/action/blob/main/dist/bin/node">custom node executable</a>.</p>
</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/CodSpeedHQ/action/blob/main/CHANGELOG.md">CodSpeedHQ/action's changelog</a>.</em></p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/CodSpeedHQ/action/commit/3cb272459e1691ee9a28a6628ab6c5cbf4219399"><code>3cb2724</code></a> Release v2.0.1 🚀</li>
<li><a href="https://github.com/CodSpeedHQ/action/commit/6790ba286f399d331fcae56bb0ca07439e79c3c4"><code>6790ba2</code></a> feat: simplify action output</li>
<li><a href="https://github.com/CodSpeedHQ/action/commit/0c6eabe26db40afca825bd704dfb236d948bf6a4"><code>0c6eabe</code></a> fix: quiet the codspeed-runner installation</li>
<li><a href="https://github.com/CodSpeedHQ/action/commit/4f1d7b4ec1478d38adad51b6ef3be817585564e6"><code>4f1d7b4</code></a> Release v2.0.0 🚀</li>
<li><a href="https://github.com/CodSpeedHQ/action/commit/de30d2b7e6b709ad7f06bee5a7bf2b4d5533ba09"><code>de30d2b</code></a> chore: use full version in script and stop relying on pnpm</li>
<li><a href="https://github.com/CodSpeedHQ/action/commit/5a53587953615b98bc13084f5249a3de7054b371"><code>5a53587</code></a> chore(ci): add multiline check</li>
<li><a href="https://github.com/CodSpeedHQ/action/commit/e2bcafd32c14de3b9e8ccb3f4816a842f60dc3a3"><code>e2bcafd</code></a> feat: integrate the native runner with a composite action</li>
<li><a href="https://github.com/CodSpeedHQ/action/commit/e8372ebb5fc64d267ef35e92cb4ceb4494ca7994"><code>e8372eb</code></a> chore: update readme with vitest</li>
<li>See full diff in <a href="https://github.com/codspeedhq/action/compare/v1...v2">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=CodSpeedHQ/action&package-manager=github_actions&previous-version=1&new-version=2)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-04 08:22_

---

_Comment by @codspeed-hq[bot] on 2023-12-04 08:33_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dependabot/github_actions/CodSpeedHQ/action-2)

### Merging #8989 will **not alter performance**

<sub>Comparing <code>dependabot/github_actions/CodSpeedHQ/action-2</code> (ad4427a) with <code>main</code> (f5d4676)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-12-04 08:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

```
warning: The following rules may cause conflicts when used with the formatter: `ISC001`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
warning: Detected debug build without --no-cache.
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
error: Encountered error: No such file or directory (os error 2)
```

</p>
</details>




---

_Comment by @MichaReiser on 2023-12-04 09:24_

I asked in [Codspeed's discord](https://discord.com/channels/1065233827569598464/1065266698862739516/1181163859730497586) for help to understand why this is regressing performance.

---

_Comment by @adriencaccia on 2023-12-04 15:36_

> I asked in [Codspeed's discord](https://discord.com/channels/1065233827569598464/1065266698862739516/1181163859730497586) for help to understand why this is regressing performance.

Thanks a lot for raising the issue, a fix is available in https://github.com/CodSpeedHQ/action/releases/tag/v2.0.2 😃 
Simply re-running the benchmark job should pick the newer version of the action and fix the issue!

---

_Comment by @konstin on 2023-12-04 16:23_

@dependabot rebase

---

_Merged by @MichaReiser on 2023-12-05 00:00_

---

_Closed by @MichaReiser on 2023-12-05 00:00_

---

_Branch deleted on 2023-12-05 00:00_

---
