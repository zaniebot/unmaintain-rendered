```yaml
number: 9179
title: Bump actions/download-artifact from 3 to 4
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/github_actions/actions/download-artifact-4
created_at: 2023-12-18T08:14:12Z
updated_at: 2023-12-23T05:43:10Z
url: https://github.com/astral-sh/ruff/pull/9179
synced_at: 2026-01-10T23:07:18Z
```

# Bump actions/download-artifact from 3 to 4

---

_Pull request opened by @dependabot on 2023-12-18 08:14_

Bumps [actions/download-artifact](https://github.com/actions/download-artifact) from 3 to 4.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/download-artifact/releases">actions/download-artifact's releases</a>.</em></p>
<blockquote>
<h2>v4.0.0</h2>
<h2>What's Changed</h2>
<p>The release of upload-artifact@v4 and download-artifact@v4 are major changes to the backend architecture of Artifacts. They have numerous performance and behavioral improvements.</p>
<p>For more information, see the <a href="https://github.com/actions/toolkit/tree/main/packages/artifact"><code>@​actions/artifact</code></a> documentation.</p>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/bflad"><code>@​bflad</code></a> made their first contribution in <a href="https://redirect.github.com/actions/download-artifact/pull/194">actions/download-artifact#194</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/download-artifact/compare/v3...v4.0.0">https://github.com/actions/download-artifact/compare/v3...v4.0.0</a></p>
<h2>v3.0.2</h2>
<ul>
<li>Bump <code>@actions/artifact</code> to v1.1.1 - <a href="https://redirect.github.com/actions/download-artifact/pull/195">actions/download-artifact#195</a></li>
<li>Fixed a bug in Node16 where if an HTTP download finished too quickly (&lt;1ms, e.g. when it's mocked) we attempt to delete a temp file that has not been created yet <a href="hhttps://redirect.github.com/actions/toolkit/pull/1278">actions/toolkit#1278</a></li>
</ul>
<h2>v3.0.1</h2>
<ul>
<li><a href="https://redirect.github.com/actions/download-artifact/pull/178">Bump <code>@​actions/core</code> to 1.10.0</a></li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/actions/download-artifact/commit/7a1cd3216ca9260cd8022db641d960b1db4d1be4"><code>7a1cd32</code></a> Merge pull request <a href="https://redirect.github.com/actions/download-artifact/issues/246">#246</a> from actions/v4-beta</li>
<li><a href="https://github.com/actions/download-artifact/commit/8f32874a49903ea488c5e7d476a9173e8706f409"><code>8f32874</code></a> licensed cache</li>
<li><a href="https://github.com/actions/download-artifact/commit/b5ff8444b1c4fcec8131f3cb1ddade813ddfacb1"><code>b5ff844</code></a> Merge pull request <a href="https://redirect.github.com/actions/download-artifact/issues/245">#245</a> from actions/robherley/v4-documentation</li>
<li><a href="https://github.com/actions/download-artifact/commit/f07a0f73f51b3f1d41667c782c821b9667da9d19"><code>f07a0f7</code></a> Update README.md</li>
<li><a href="https://github.com/actions/download-artifact/commit/7226129829bb686fdff47bd63bbd0d1373993a84"><code>7226129</code></a> update test workflow to use different artifact names for matrix</li>
<li><a href="https://github.com/actions/download-artifact/commit/ada9446619b84dd8a557aaaec3b79b58c4986cdf"><code>ada9446</code></a> update docs and bump <code>@​actions/artifact</code></li>
<li><a href="https://github.com/actions/download-artifact/commit/7eafc8b729ba790ce8f2cee54be8ad6257af4c7c"><code>7eafc8b</code></a> Merge pull request <a href="https://redirect.github.com/actions/download-artifact/issues/244">#244</a> from actions/robherley/bump-toolkit</li>
<li><a href="https://github.com/actions/download-artifact/commit/3132d12662b5915f20cdbf449465896962101abf"><code>3132d12</code></a> consume latest toolkit</li>
<li><a href="https://github.com/actions/download-artifact/commit/5be1d3867182a382bc59f2775e002595f487aa88"><code>5be1d38</code></a> Merge pull request <a href="https://redirect.github.com/actions/download-artifact/issues/243">#243</a> from actions/robherley/v4-beta-updates</li>
<li><a href="https://github.com/actions/download-artifact/commit/465b526e63559575a64716cdbb755bc78dfb263b"><code>465b526</code></a> consume latest <code>@​actions/toolkit</code></li>
<li>Additional commits viewable in <a href="https://github.com/actions/download-artifact/compare/v3...v4">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=actions/download-artifact&package-manager=github_actions&previous-version=3&new-version=4)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-18 08:14_

---

_Comment by @dependabot[bot] on 2023-12-23 05:43_

Superseded by #9256.

---

_Closed by @dependabot[bot] on 2023-12-23 05:43_

---

_Branch deleted on 2023-12-23 05:43_

---
