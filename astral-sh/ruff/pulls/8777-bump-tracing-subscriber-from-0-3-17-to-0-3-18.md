```yaml
number: 8777
title: Bump tracing-subscriber from 0.3.17 to 0.3.18
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/tracing-subscriber-0.3.18
created_at: 2023-11-20T08:17:44Z
updated_at: 2023-11-20T08:55:57Z
url: https://github.com/astral-sh/ruff/pull/8777
synced_at: 2026-01-10T23:40:55Z
```

# Bump tracing-subscriber from 0.3.17 to 0.3.18

---

_Pull request opened by @dependabot on 2023-11-20 08:17_

Bumps [tracing-subscriber](https://github.com/tokio-rs/tracing) from 0.3.17 to 0.3.18.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/tokio-rs/tracing/releases">tracing-subscriber's releases</a>.</em></p>
<blockquote>
<h2>tracing-subscriber 0.3.18</h2>
<p>This release of <code>tracing-subscriber</code> adds support for the <a href="https://no-color.org/"><code>NO_COLOR</code></a> environment
variable (an informal standard to disable emitting ANSI color escape codes) in
<code>fmt::Layer</code>, reintroduces support for the <a href="https://github.com/chronotope/chrono"><code>chrono</code></a> crate, and increases the
minimum supported Rust version (MSRV) to Rust 1.63.0.</p>
<p>It also introduces several minor API improvements.</p>
<h3>Added</h3>
<ul>
<li><strong>chrono</strong>: Add <a href="https://github.com/chronotope/chrono"><code>chrono</code></a> implementations of <code>FormatTime</code> (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2690">#2690</a>)</li>
<li><strong>subscriber</strong>: Add support for the <a href="https://no-color.org/"><code>NO_COLOR</code></a> environment variable in
<code>fmt::Layer</code> (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2647">#2647</a>)</li>
<li><strong>fmt</strong>: make <code>format::Writer::new()</code> public (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2680">#2680</a>)</li>
<li><strong>filter</strong>: Implement <code>layer::Filter</code> for <code>Option&lt;Filter&gt;</code> (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2407">#2407</a>)</li>
</ul>
<h3>Changed</h3>
<ul>
<li><strong>log</strong>: bump version of <code>tracing-log</code> to 0.2 (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2772">#2772</a>)</li>
<li>Increased minimum supported Rust version (MSRV) to 1.63.0+.</li>
</ul>
<p><a href="https://redirect.github.com/tokio-rs/tracing/issues/2690">#2690</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2690">tokio-rs/tracing#2690</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2647">#2647</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2647">tokio-rs/tracing#2647</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2680">#2680</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2680">tokio-rs/tracing#2680</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2407">#2407</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2407">tokio-rs/tracing#2407</a>
<a href="https://redirect.github.com/tokio-rs/tracing/issues/2772">#2772</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2772">tokio-rs/tracing#2772</a></p>
<p>Thanks to <a href="https://github.com/shayne-fletcher"><code>@​shayne-fletcher</code></a>, <a href="https://github.com/dmlary"><code>@​dmlary</code></a>, <a href="https://github.com/kaifastromai"><code>@​kaifastromai</code></a>, and <a href="https://github.com/jsgf"><code>@​jsgf</code></a> for contributing!</p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/tokio-rs/tracing/commit/8b7a1dde69797b33ecfa20da71e72eb5e61f0b25"><code>8b7a1dd</code></a> chore: prepare tracing-subscriber 0.3.18 release (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2789">#2789</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/befb4de073acc8a228d6ad041088e6332253e8de"><code>befb4de</code></a> chore: fix <code>ahash</code>-caused build breakage (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2792">#2792</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/1dc1e6a302dd13e0c4de5e18c7a119970c9571a8"><code>1dc1e6a</code></a> chore: bump <code>ahash</code> to 0.7.7 (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2794">#2794</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/abb23931ef496902e241c39f67a8ed03e8ebbcca"><code>abb2393</code></a> chore: backport CI improvements (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2238">#2238</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/c6abc10c3a1b91a4c17d1fb09e081311a5f90eb3"><code>c6abc10</code></a> chore: bump MSRV to 1.63 (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2793">#2793</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/4529182e607d6165a496639e006a476edb6b7409"><code>4529182</code></a> attributes: added missing RecordTypes for instrument (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2781">#2781</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/7b594354cf0f7919be6975ac73f0ada9ed54aeee"><code>7b59435</code></a> subcriber: update docs for EnvFilter Builder (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2782">#2782</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/119f91a85cb6a8bc797fcf53d91c7719be0d0ee2"><code>119f91a</code></a> tracing: removed core imports in macros (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2762">#2762</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/346d6e6456aa9d62e2b3443b934c153589aa9444"><code>346d6e6</code></a> core: fix incorrect (incorrectly updated) docs for LevelFilter (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2767">#2767</a>)</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/8cdc9da2020d4c9e467dd094008dbbb96c87c402"><code>8cdc9da</code></a> appender: remove <code>Sync</code> bound from writer for <code>NonBlocking</code> (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2607">#2607</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/tokio-rs/tracing/compare/tracing-subscriber-0.3.17...tracing-subscriber-0.3.18">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=tracing-subscriber&package-manager=cargo&previous-version=0.3.17&new-version=0.3.18)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Comment by @github-actions[bot] on 2023-11-20 08:54_

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

_Merged by @konstin on 2023-11-20 08:55_

---

_Closed by @konstin on 2023-11-20 08:55_

---

_Branch deleted on 2023-11-20 08:55_

---
