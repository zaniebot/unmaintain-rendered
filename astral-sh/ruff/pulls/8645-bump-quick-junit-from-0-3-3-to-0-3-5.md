```yaml
number: 8645
title: Bump quick-junit from 0.3.3 to 0.3.5
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/quick-junit-0.3.5
created_at: 2023-11-13T08:08:01Z
updated_at: 2023-11-13T14:38:33Z
url: https://github.com/astral-sh/ruff/pull/8645
synced_at: 2026-01-10T23:40:55Z
```

# Bump quick-junit from 0.3.3 to 0.3.5

---

_Pull request opened by @dependabot on 2023-11-13 08:08_

[//]: # (dependabot-start)
⚠️  **Dependabot is rebasing this PR** ⚠️ 

Rebasing might not happen immediately, so don't worry if this takes some time.

Note: if you make any changes to this PR yourself, they will take precedence over the rebase.

---

[//]: # (dependabot-end)

Bumps [quick-junit](https://github.com/nextest-rs/nextest) from 0.3.3 to 0.3.5.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/nextest-rs/nextest/releases">quick-junit's releases</a>.</em></p>
<blockquote>
<h2>quick-junit 0.3.5</h2>
<h3>Fixed</h3>
<ul>
<li>Corrected the <code>rust-version</code> field in <code>Cargo.toml</code> to <code>1.70</code>.</li>
</ul>
<h3>Changed</h3>
<ul>
<li>The <code>chrono</code> dependency no longer imports the <code>clock</code> feature. This helps cut down on the dependency tree. Thanks <a href="https://github.com/littledivy"><code>@​littledivy</code></a> for your first contribution!</li>
</ul>
<h2>quick-junit 0.3.4</h2>
<h3>Fixed</h3>
<ul>
<li><code>Output::new</code> now strips ANSI escapes as well. Thanks <a href="https://github.com/MaienM"><code>@​MaienM</code></a> for your first contribution!</li>
</ul>
<h3>Changed</h3>
<ul>
<li>MSRV updated to Rust 1.70.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/nextest-rs/nextest/commit/96b2d16bbe82dd14fe62ade603fa0d6a93e2c14f"><code>96b2d16</code></a> [quick-junit] version 0.3.5</li>
<li><a href="https://github.com/nextest-rs/nextest/commit/bf9a839f259789a05fc1891804db6402a49f44bd"><code>bf9a839</code></a> [quick-junit] prepare release</li>
<li><a href="https://github.com/nextest-rs/nextest/commit/0fd55ed351a3de822dd855bd4188db11a39a5cf6"><code>0fd55ed</code></a> Use bare minimum chrono features (<a href="https://redirect.github.com/nextest-rs/nextest/issues/1073">#1073</a>)</li>
<li><a href="https://github.com/nextest-rs/nextest/commit/beed18c0c3bc6996e6afd77fa3430f1c238e08b4"><code>beed18c</code></a> Update Rust crate toml_edit to 0.20.5</li>
<li><a href="https://github.com/nextest-rs/nextest/commit/c405796d2f0413de4d2716c3049a5740726f58c5"><code>c405796</code></a> Update Rust crate toml to 0.8.5</li>
<li><a href="https://github.com/nextest-rs/nextest/commit/664e87657d3e5c0551be98eeebbd4ed8f23e94f8"><code>664e876</code></a> Update Rust crate futures to 0.3.29</li>
<li><a href="https://github.com/nextest-rs/nextest/commit/15402ec5de9c94cd7e46b884c91a378f9dae85da"><code>15402ec</code></a> Update taiki-e/install-action digest to 0e17f1a</li>
<li><a href="https://github.com/nextest-rs/nextest/commit/14f9a5bf1460896cb92dcd06621674fe1599f4f7"><code>14f9a5b</code></a> Update slow-tests.md</li>
<li><a href="https://github.com/nextest-rs/nextest/commit/8d19a4dd795655af1f061cd293a9aa118fff8277"><code>8d19a4d</code></a> Update Rust crate serde to 1.0.190</li>
<li><a href="https://github.com/nextest-rs/nextest/commit/e4912b246ac83d4802813782956a0cc9bc5e4d26"><code>e4912b2</code></a> Update slow-tests.md</li>
<li>Additional commits viewable in <a href="https://github.com/nextest-rs/nextest/compare/quick-junit-0.3.3...quick-junit-0.3.5">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=quick-junit&package-manager=cargo&previous-version=0.3.3&new-version=0.3.5)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-11-13 08:08_

---

_Comment by @github-actions[bot] on 2023-11-13 08:27_

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

_Merged by @charliermarsh on 2023-11-13 14:38_

---

_Closed by @charliermarsh on 2023-11-13 14:38_

---

_Branch deleted on 2023-11-13 14:38_

---
