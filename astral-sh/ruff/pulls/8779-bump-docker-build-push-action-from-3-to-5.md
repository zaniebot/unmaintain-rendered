```yaml
number: 8779
title: Bump docker/build-push-action from 3 to 5
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/github_actions/docker/build-push-action-5
created_at: 2023-11-20T08:17:57Z
updated_at: 2023-11-20T08:32:23Z
url: https://github.com/astral-sh/ruff/pull/8779
synced_at: 2026-01-12T15:55:27Z
```

# Bump docker/build-push-action from 3 to 5

---

_@dependabot_

Bumps [docker/build-push-action](https://github.com/docker/build-push-action) from 3 to 5.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/docker/build-push-action/releases">docker/build-push-action's releases</a>.</em></p>
<blockquote>
<h2>v5.0.0</h2>
<ul>
<li>Node 20 as default runtime (requires <a href="https://github.com/actions/runner/releases/tag/v2.308.0">Actions Runner v2.308.0</a> or later) by <a href="https://github.com/crazy-max"><code>@​crazy-max</code></a> in <a href="https://redirect.github.com/docker/build-push-action/pull/954">docker/build-push-action#954</a></li>
<li>Bump <code>@​actions/core</code> from 1.10.0 to 1.10.1 in <a href="https://redirect.github.com/docker/build-push-action/pull/959">docker/build-push-action#959</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/docker/build-push-action/compare/v4.2.1...v5.0.0">https://github.com/docker/build-push-action/compare/v4.2.1...v5.0.0</a></p>
<h2>v4.2.1</h2>
<blockquote>
<p><strong>Note</strong></p>
<p>Buildx v0.10 enables support for a minimal <a href="https://slsa.dev/provenance/">SLSA Provenance</a> attestation, which requires support for <a href="https://github.com/opencontainers/image-spec">OCI-compliant</a> multi-platform images. This may introduce issues with registry and runtime support (e.g. <a href="https://redirect.github.com/docker/buildx/issues/1533">Google Cloud Run and AWS Lambda</a>). You can optionally disable the default provenance attestation functionality using <code>provenance: false</code>.</p>
</blockquote>
<ul>
<li>warn if docker config can't be parsed by <a href="https://github.com/crazy-max"><code>@​crazy-max</code></a> in <a href="https://redirect.github.com/docker/build-push-action/pull/957">docker/build-push-action#957</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/docker/build-push-action/compare/v4.2.0...v4.2.1">https://github.com/docker/build-push-action/compare/v4.2.0...v4.2.1</a></p>
<h2>v4.2.0</h2>
<blockquote>
<p><strong>Note</strong></p>
<p>Buildx v0.10 enables support for a minimal <a href="https://slsa.dev/provenance/">SLSA Provenance</a> attestation, which requires support for <a href="https://github.com/opencontainers/image-spec">OCI-compliant</a> multi-platform images. This may introduce issues with registry and runtime support (e.g. <a href="https://redirect.github.com/docker/buildx/issues/1533">Google Cloud Run and AWS Lambda</a>). You can optionally disable the default provenance attestation functionality using <code>provenance: false</code>.</p>
</blockquote>
<ul>
<li>display proxy configuration  by <a href="https://github.com/crazy-max"><code>@​crazy-max</code></a> in <a href="https://redirect.github.com/docker/build-push-action/pull/872">docker/build-push-action#872</a></li>
<li>chore(deps): Bump <code>@​docker/actions-toolkit</code> from 0.6.0 to 0.8.0 in <a href="https://redirect.github.com/docker/build-push-action/pull/930">docker/build-push-action#930</a></li>
<li>chore(deps): Bump word-wrap from 1.2.3 to 1.2.5 in <a href="https://redirect.github.com/docker/build-push-action/pull/925">docker/build-push-action#925</a></li>
<li>chore(deps): Bump semver from 6.3.0 to 6.3.1 in <a href="https://redirect.github.com/docker/build-push-action/pull/902">docker/build-push-action#902</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/docker/build-push-action/compare/v4.1.1...v4.2.0">https://github.com/docker/build-push-action/compare/v4.1.1...v4.2.0</a></p>
<h2>v4.1.1</h2>
<blockquote>
<p><strong>Note</strong></p>
<p>Buildx v0.10 enables support for a minimal <a href="https://slsa.dev/provenance/">SLSA Provenance</a> attestation, which requires support for <a href="https://github.com/opencontainers/image-spec">OCI-compliant</a> multi-platform images. This may introduce issues with registry and runtime support (e.g. <a href="https://redirect.github.com/docker/buildx/issues/1533">Google Cloud Run and AWS Lambda</a>). You can optionally disable the default provenance attestation functionality using <code>provenance: false</code>.</p>
</blockquote>
<ul>
<li>Bump <code>@​docker/actions-toolkit</code> from 0.3.0 to 0.5.0 by <a href="https://github.com/crazy-max"><code>@​crazy-max</code></a> in <a href="https://redirect.github.com/docker/build-push-action/pull/880">docker/build-push-action#880</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/docker/build-push-action/compare/v4.1.0...v4.1.1">https://github.com/docker/build-push-action/compare/v4.1.0...v4.1.1</a></p>
<h2>v4.1.0</h2>
<blockquote>
<p><strong>Note</strong></p>
<p>Buildx v0.10 enables support for a minimal <a href="https://slsa.dev/provenance/">SLSA Provenance</a> attestation, which requires support for <a href="https://github.com/opencontainers/image-spec">OCI-compliant</a> multi-platform images. This may introduce issues with registry and runtime support (e.g. <a href="https://redirect.github.com/docker/buildx/issues/1533">Google Cloud Run and AWS Lambda</a>). You can optionally disable the default provenance attestation functionality using <code>provenance: false</code>.</p>
</blockquote>
<ul>
<li>Switch to actions-toolkit implementation by <a href="https://github.com/crazy-max"><code>@​crazy-max</code></a> in <a href="https://redirect.github.com/docker/build-push-action/pull/811">docker/build-push-action#811</a>  <a href="https://redirect.github.com/docker/build-push-action/pull/838">docker/build-push-action#838</a> <a href="https://redirect.github.com/docker/build-push-action/pull/855">docker/build-push-action#855</a> <a href="https://redirect.github.com/docker/build-push-action/pull/860">docker/build-push-action#860</a> <a href="https://redirect.github.com/docker/build-push-action/pull/875">docker/build-push-action#875</a></li>
<li>e2e: quay.io by <a href="https://github.com/crazy-max"><code>@​crazy-max</code></a> in <a href="https://redirect.github.com/docker/build-push-action/pull/799">docker/build-push-action#799</a> <a href="https://redirect.github.com/docker/build-push-action/pull/805">docker/build-push-action#805</a></li>
<li>e2e: local harbor and nexus by <a href="https://github.com/crazy-max"><code>@​crazy-max</code></a> in <a href="https://redirect.github.com/docker/build-push-action/pull/800">docker/build-push-action#800</a></li>
<li>e2e: add artifactory container registry to test against by <a href="https://github.com/jedevc"><code>@​jedevc</code></a> in <a href="https://redirect.github.com/docker/build-push-action/pull/804">docker/build-push-action#804</a></li>
<li>e2e: add distribution tests by <a href="https://github.com/jedevc"><code>@​jedevc</code></a> in <a href="https://redirect.github.com/docker/build-push-action/pull/814">docker/build-push-action#814</a> <a href="https://redirect.github.com/docker/build-push-action/pull/815">docker/build-push-action#815</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/docker/build-push-action/compare/v4.0.0...v4.1.0">https://github.com/docker/build-push-action/compare/v4.0.0...v4.1.0</a></p>
<h2>v4.0.0</h2>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/docker/build-push-action/commit/4a13e500e55cf31b7a5d59a38ab2040ab0f42f56"><code>4a13e50</code></a> Merge pull request <a href="https://redirect.github.com/docker/build-push-action/issues/1006">#1006</a> from docker/dependabot/npm_and_yarn/docker/actions-t...</li>
<li><a href="https://github.com/docker/build-push-action/commit/74166686865cdc289a02f214871fb53447b73447"><code>7416668</code></a> chore: update generated content</li>
<li><a href="https://github.com/docker/build-push-action/commit/b4f76a5dc6a67282180eddc6d460f23bc97bfcbc"><code>b4f76a5</code></a> chore(deps): Bump <code>@​docker/actions-toolkit</code> from 0.13.0 to 0.14.0</li>
<li><a href="https://github.com/docker/build-push-action/commit/b7feb766fae338d85274c87a9d0f24c09690dbe2"><code>b7feb76</code></a> Merge pull request <a href="https://redirect.github.com/docker/build-push-action/issues/1005">#1005</a> from crazy-max/ci-inspect</li>
<li><a href="https://github.com/docker/build-push-action/commit/fae8018297c67066fff64a6e9c319c86f89b8982"><code>fae8018</code></a> ci: inspect sbom and provenance</li>
<li><a href="https://github.com/docker/build-push-action/commit/b625868b13c3feb675cabbf9bfeb52ae94166606"><code>b625868</code></a> Merge pull request <a href="https://redirect.github.com/docker/build-push-action/issues/1004">#1004</a> from crazy-max/ci-update-buildx</li>
<li><a href="https://github.com/docker/build-push-action/commit/5193ef1da6ea0d66de97d22817c258b203ade96a"><code>5193ef1</code></a> ci: update buildx to latest</li>
<li><a href="https://github.com/docker/build-push-action/commit/d3afd779e409ac26db5374fb27fe4aae9f6adb42"><code>d3afd77</code></a> Merge pull request <a href="https://redirect.github.com/docker/build-push-action/issues/991">#991</a> from docker/dependabot/npm_and_yarn/babel/traverse-7....</li>
<li><a href="https://github.com/docker/build-push-action/commit/7a786bb2b9408f7f997564f677248fabd4b886d5"><code>7a786bb</code></a> Merge pull request <a href="https://redirect.github.com/docker/build-push-action/issues/992">#992</a> from crazy-max/annotations</li>
<li><a href="https://github.com/docker/build-push-action/commit/c66ae3adcfbf698ecd851c6bb782654a0c6ffcae"><code>c66ae3a</code></a> chore: update generated content</li>
<li>Additional commits viewable in <a href="https://github.com/docker/build-push-action/compare/v3...v5">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=docker/build-push-action&package-manager=github_actions&previous-version=3&new-version=5)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-11-20 08:17_

---

_Merged by @konstin on 2023-11-20 08:32_

---

_Closed by @konstin on 2023-11-20 08:32_

---

_Branch deleted on 2023-11-20 08:32_

---
