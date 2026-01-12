```yaml
number: 8127
title: Bump tracing from 0.1.39 to 0.1.40
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/tracing-0.1.40
created_at: 2023-10-23T08:53:05Z
updated_at: 2023-10-23T09:08:48Z
url: https://github.com/astral-sh/ruff/pull/8127
synced_at: 2026-01-12T02:32:41Z
```

# Bump tracing from 0.1.39 to 0.1.40

---

_Pull request opened by @dependabot on 2023-10-23 08:53_

Bumps [tracing](https://github.com/tokio-rs/tracing) from 0.1.39 to 0.1.40.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/tokio-rs/tracing/releases">tracing's releases</a>.</em></p>
<blockquote>
<h2>tracing 0.1.40</h2>
<p>This release fixes a potential stack use-after-free in the
<code>Instrument::into_inner</code> method. Only uses of this method are affected by this
bug.</p>
<h3>Fixed</h3>
<ul>
<li>Use <code>mem::ManuallyDrop</code> instead of <code>mem::forget</code> in <code>Instrument::into_inner</code>
(<a href="https://redirect.github.com/tokio-rs/tracing/issues/2765">#2765</a>)</li>
</ul>
<p><a href="https://redirect.github.com/tokio-rs/tracing/issues/2765">#2765</a>: <a href="https://redirect.github.com/tokio-rs/tracing/pull/2765">tokio-rs/tracing#2765</a></p>
<p>Thanks to <a href="https://github.com/cramertj"><code>@​cramertj</code></a> and <a href="https://github.com/manishearth"><code>@​manishearth</code></a> for finding and fixing this issue!</p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/tokio-rs/tracing/commit/15600a3a67c418f53cb80ff21da57d89a5de0486"><code>15600a3</code></a> tracing: prepare to release v0.1.40</li>
<li><a href="https://github.com/tokio-rs/tracing/commit/20a1762b3fd5f1fafead198fd18e469c68683721"><code>20a1762</code></a> tracing: use ManuallyDrop instead of mem::forget (<a href="https://redirect.github.com/tokio-rs/tracing/issues/2765">#2765</a>)</li>
<li>See full diff in <a href="https://github.com/tokio-rs/tracing/compare/tracing-0.1.39...tracing-0.1.40">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=tracing&package-manager=cargo&previous-version=0.1.39&new-version=0.1.40)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-23 08:53_

---

_Merged by @MichaReiser on 2023-10-23 09:04_

---

_Closed by @MichaReiser on 2023-10-23 09:04_

---

_Branch deleted on 2023-10-23 09:04_

---

_Comment by @github-actions[bot] on 2023-10-23 09:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
