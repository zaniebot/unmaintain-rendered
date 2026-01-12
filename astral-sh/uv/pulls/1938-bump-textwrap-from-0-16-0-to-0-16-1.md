```yaml
number: 1938
title: Bump textwrap from 0.16.0 to 0.16.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/textwrap-0.16.1
created_at: 2024-02-23T19:46:35Z
updated_at: 2024-02-23T20:01:06Z
url: https://github.com/astral-sh/uv/pull/1938
synced_at: 2026-01-12T16:04:47Z
```

# Bump textwrap from 0.16.0 to 0.16.1

---

_@dependabot_

Bumps [textwrap](https://github.com/mgeisler/textwrap) from 0.16.0 to 0.16.1.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/mgeisler/textwrap/releases">textwrap's releases</a>.</em></p>
<blockquote>
<h2>textwrap-0.16.1</h2>
<h2>Version 0.16.1 (2024-02-17)</h2>
<p>This release fixes <code>display_width</code> to ignore inline-hyperlinks. The minimum
supported version of Rust is now documented to be 1.56.</p>
<ul>
<li><a href="https://redirect.github.com/mgeisler/textwrap/pull/526">#526</a>: Ignore ANSI hyperlinks
in <code>display_width</code>: calculations.</li>
<li><a href="https://redirect.github.com/mgeisler/textwrap/pull/521">#521</a>: Add <code>Options::width</code>
setter method.</li>
<li><a href="https://redirect.github.com/mgeisler/textwrap/pull/520">#520</a>: Clarify that
<code>WordSeparator</code> is an enum rather than a trait.</li>
<li><a href="https://redirect.github.com/mgeisler/textwrap/pull/518">#518</a>: Test with latest stable
and nightly Rust, but check that we can build with Rust 1.56.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/mgeisler/textwrap/blob/master/CHANGELOG.md">textwrap's changelog</a>.</em></p>
<blockquote>
<h2>Version 0.16.1 (2024-02-17)</h2>
<p>This release fixes <code>display_width</code> to ignore inline-hyperlinks. The minimum
supported version of Rust is now documented to be 1.56.</p>
<ul>
<li><a href="https://redirect.github.com/mgeisler/textwrap/pull/526">#526</a>: Ignore ANSI hyperlinks
in <code>display_width</code>: calculations.</li>
<li><a href="https://redirect.github.com/mgeisler/textwrap/pull/521">#521</a>: Add <code>Options::width</code>
setter method.</li>
<li><a href="https://redirect.github.com/mgeisler/textwrap/pull/520">#520</a>: Clarify that
<code>WordSeparator</code> is an enum rather than a trait.</li>
<li><a href="https://redirect.github.com/mgeisler/textwrap/pull/518">#518</a>: Test with latest stable
and nightly Rust, but check that we can build with Rust 1.56.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/mgeisler/textwrap/commit/39914e0fd7e9af79908c5b9a5cfbb303fe6778e8"><code>39914e0</code></a> Merge pull request <a href="https://redirect.github.com/mgeisler/textwrap/issues/533">#533</a> from mgeisler/release-0.16.1</li>
<li><a href="https://github.com/mgeisler/textwrap/commit/8f84d66737578a9575d0f3c57d8e51e11ccc0d79"><code>8f84d66</code></a> Bump version to 0.16.1</li>
<li><a href="https://github.com/mgeisler/textwrap/commit/a87c39548665a4eb6c36a8f9f6299376bcffe6a7"><code>a87c395</code></a> Update changelog for version 0.16.1</li>
<li><a href="https://github.com/mgeisler/textwrap/commit/3dd0774cc3ead0a441ab08d26aa4fb28d99cac86"><code>3dd0774</code></a> Add dependency graph for version 0.16.1</li>
<li><a href="https://github.com/mgeisler/textwrap/commit/b656c0719a2cd25348a1e43861fbf2fa7e58c2ad"><code>b656c07</code></a> Merge pull request <a href="https://redirect.github.com/mgeisler/textwrap/issues/532">#532</a> from mgeisler/skip-ansi-escape-sequence-early-return</li>
<li><a href="https://github.com/mgeisler/textwrap/commit/599b78a3154abc103085c5b7767a0944a7a98ac4"><code>599b78a</code></a> Early return in <code>skip_ansi_escape_sequence</code></li>
<li><a href="https://github.com/mgeisler/textwrap/commit/e7a20efd3342c06323bf90c8e3e562bb47c1c48d"><code>e7a20ef</code></a> Merge pull request <a href="https://redirect.github.com/mgeisler/textwrap/issues/526">#526</a> from tertsdiepraam/ansi-hyperlink</li>
<li><a href="https://github.com/mgeisler/textwrap/commit/5d28004b830150c59be20e4921b61d502a3f376b"><code>5d28004</code></a> display_width: support BEL as a separator in hyperlinks</li>
<li><a href="https://github.com/mgeisler/textwrap/commit/aa097981e5b8fde7bf7a05e14d1dd60b90d02be8"><code>aa09798</code></a> Merge pull request <a href="https://redirect.github.com/mgeisler/textwrap/issues/529">#529</a> from mgeisler/dependabot/npm_and_yarn/examples/wasm/w...</li>
<li><a href="https://github.com/mgeisler/textwrap/commit/12feb68780eb8cfdabdfbc398cd3fb94f3c3a19c"><code>12feb68</code></a> Bump follow-redirects from 1.15.2 to 1.15.4 in /examples/wasm/www</li>
<li>Additional commits viewable in <a href="https://github.com/mgeisler/textwrap/compare/0.16.0...0.16.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=textwrap&package-manager=cargo&previous-version=0.16.0&new-version=0.16.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-02-23 19:46_

---

_Merged by @zanieb on 2024-02-23 20:01_

---

_Closed by @zanieb on 2024-02-23 20:01_

---

_Branch deleted on 2024-02-23 20:01_

---
