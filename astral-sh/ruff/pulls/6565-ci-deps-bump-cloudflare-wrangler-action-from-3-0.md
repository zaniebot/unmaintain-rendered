```yaml
number: 6565
title: "ci(deps): bump cloudflare/wrangler-action from 3.0.0 to 3.0.2"
type: pull_request
state: merged
author: dependabot
labels:
  - dependencies
assignees: []
merged: true
base: main
head: dependabot/github_actions/cloudflare/wrangler-action-3.0.2
created_at: 2023-08-14T16:38:42Z
updated_at: 2023-08-17T14:11:05Z
url: https://github.com/astral-sh/ruff/pull/6565
synced_at: 2026-01-12T15:55:21Z
```

# ci(deps): bump cloudflare/wrangler-action from 3.0.0 to 3.0.2

---

_@dependabot_

Bumps [cloudflare/wrangler-action](https://github.com/cloudflare/wrangler-action) from 3.0.0 to 3.0.2.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/cloudflare/wrangler-action/releases">cloudflare/wrangler-action's releases</a>.</em></p>
<blockquote>
<h2>v3.0.2</h2>
<h3>Patch Changes</h3>
<ul>
<li>
<p><a href="https://redirect.github.com/cloudflare/wrangler-action/pull/147">#147</a> <a href="https://github.com/cloudflare/wrangler-action/commit/58f274b9f70867447519c6fa983c813e2b167b85"><code>58f274b</code></a> Thanks <a href="https://github.com/JacobMGEvans"><code>@​JacobMGEvans</code></a>! - Added more error logging when a command fails to execute
Previously, we prevented any error logs from propagating too far to prevent leaking of any potentially sensitive information. However, this made it difficult for developers to debug their code.</p>
<p>In this release, we have updated our error handling to allow for more error messaging from pre/post and custom commands. We still discourage the use of these commands for secrets or other sensitive information, but we believe this change will make it easier for developers to debug their code.</p>
<p>Relates to <a href="https://redirect.github.com/cloudflare/wrangler-action/issues/137">#137</a></p>
</li>
<li>
<p><a href="https://redirect.github.com/cloudflare/wrangler-action/pull/147">#147</a> <a href="https://github.com/cloudflare/wrangler-action/commit/58f274b9f70867447519c6fa983c813e2b167b85"><code>58f274b</code></a> Thanks <a href="https://github.com/JacobMGEvans"><code>@​JacobMGEvans</code></a>! - Adding Changesets</p>
</li>
<li>
<p><a href="https://github.com/cloudflare/wrangler-action/blob/HEAD/#version-300">Version 3.0.0</a></p>
</li>
<li>
<p><a href="https://github.com/cloudflare/wrangler-action/blob/HEAD/#version-200">Version 2.0.0</a></p>
</li>
</ul>
<h2>v3.0.1</h2>
<p>Automating Build &amp; Release</p>
<h2>What's Changed</h2>
<ul>
<li>Artifacts are now ESM supported with <code>.mjs</code></li>
<li>Update publish to deploy by <a href="https://github.com/lrapoport-cf"><code>@​lrapoport-cf</code></a> in <a href="https://redirect.github.com/cloudflare/wrangler-action/pull/124">cloudflare/wrangler-action#124</a></li>
<li>Automatically add issues to workers-sdk GH project by <a href="https://github.com/lrapoport-cf"><code>@​lrapoport-cf</code></a> in <a href="https://redirect.github.com/cloudflare/wrangler-action/pull/127">cloudflare/wrangler-action#127</a></li>
<li>Automate Action Release by <a href="https://github.com/JacobMGEvans"><code>@​JacobMGEvans</code></a> in <a href="https://redirect.github.com/cloudflare/wrangler-action/pull/128">cloudflare/wrangler-action#128</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/lrapoport-cf"><code>@​lrapoport-cf</code></a> made their first contribution in <a href="https://redirect.github.com/cloudflare/wrangler-action/pull/124">cloudflare/wrangler-action#124</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/cloudflare/wrangler-action/compare/3.0.0...3.0.1">https://github.com/cloudflare/wrangler-action/compare/3.0.0...3.0.1</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/cloudflare/wrangler-action/blob/main/CHANGELOG.md">cloudflare/wrangler-action's changelog</a>.</em></p>
<blockquote>
<h2>3.0.2</h2>
<h3>Patch Changes</h3>
<ul>
<li>
<p><a href="https://redirect.github.com/cloudflare/wrangler-action/pull/147">#147</a> <a href="https://github.com/cloudflare/wrangler-action/commit/58f274b9f70867447519c6fa983c813e2b167b85"><code>58f274b</code></a> Thanks <a href="https://github.com/JacobMGEvans"><code>@​JacobMGEvans</code></a>! - Added more error logging when a command fails to execute
Previously, we prevented any error logs from propagating too far to prevent leaking of any potentially sensitive information. However, this made it difficult for developers to debug their code.</p>
<p>In this release, we have updated our error handling to allow for more error messaging from pre/post and custom commands. We still discourage the use of these commands for secrets or other sensitive information, but we believe this change will make it easier for developers to debug their code.</p>
<p>Relates to <a href="https://redirect.github.com/cloudflare/wrangler-action/issues/137">#137</a></p>
</li>
<li>
<p><a href="https://redirect.github.com/cloudflare/wrangler-action/pull/147">#147</a> <a href="https://github.com/cloudflare/wrangler-action/commit/58f274b9f70867447519c6fa983c813e2b167b85"><code>58f274b</code></a> Thanks <a href="https://github.com/JacobMGEvans"><code>@​JacobMGEvans</code></a>! - Adding Changesets</p>
</li>
<li>
<p><a href="https://github.com/cloudflare/wrangler-action/blob/main/#version-300">Version 3.0.0</a></p>
</li>
<li>
<p><a href="https://github.com/cloudflare/wrangler-action/blob/main/#version-200">Version 2.0.0</a></p>
</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/80501a7b4d229a89df2bf50ac1bc99bc0f305ae9"><code>80501a7</code></a> Automatic compilation</li>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/32527114049e402a74f9b440bc36518398585dbb"><code>3252711</code></a> hotfix</li>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/2faabf36a28803498aedb7811da1ed1782827398"><code>2faabf3</code></a> Merge pull request <a href="https://redirect.github.com/cloudflare/wrangler-action/issues/150">#150</a> from cloudflare/jacobmgevans/changesets-tag-cli</li>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/b734d85d748861f15b2e96f747f397a1e3a092bf"><code>b734d85</code></a> Changesets needs a tag created for release</li>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/8ca2ff161218573ed25fffb12aeef6e0f555c20c"><code>8ca2ff1</code></a> Merge pull request <a href="https://redirect.github.com/cloudflare/wrangler-action/issues/148">#148</a> from cloudflare/changeset-release/main</li>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/7d08a8657e7551c59cd13d347e6a8472bf05d4f6"><code>7d08a86</code></a> Merge pull request <a href="https://redirect.github.com/cloudflare/wrangler-action/issues/149">#149</a> from cloudflare/jacobmgevans/changesets-needs-publish</li>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/e193627f19b8f57672ef173216b84304a4564824"><code>e193627</code></a> Spoofing publish for Changeset Action</li>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/55b80c5f62ea9cb7a91f120dae709ff7af1eef9d"><code>55b80c5</code></a> Version Packages</li>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/f61dc4d5a97cf9a3584f4e14b91922db7ee0723d"><code>f61dc4d</code></a> Merge pull request <a href="https://redirect.github.com/cloudflare/wrangler-action/issues/147">#147</a> from cloudflare/revert-146-changeset-release/main</li>
<li><a href="https://github.com/cloudflare/wrangler-action/commit/58f274b9f70867447519c6fa983c813e2b167b85"><code>58f274b</code></a> Revert &quot;Version Packages&quot;</li>
<li>Additional commits viewable in <a href="https://github.com/cloudflare/wrangler-action/compare/3.0.0...v3.0.2">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=cloudflare/wrangler-action&package-manager=github_actions&previous-version=3.0.0&new-version=3.0.2)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `dependencies` added by @dependabot[bot] on 2023-08-14 16:38_

---

_Merged by @zanieb on 2023-08-17 14:11_

---

_Closed by @zanieb on 2023-08-17 14:11_

---

_Branch deleted on 2023-08-17 14:11_

---
