```yaml
number: 8993
title: Bump ureq from 2.8.0 to 2.9.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/ureq-2.9.1
created_at: 2023-12-04T08:41:20Z
updated_at: 2023-12-04T15:53:27Z
url: https://github.com/astral-sh/ruff/pull/8993
synced_at: 2026-01-12T15:55:27Z
```

# Bump ureq from 2.8.0 to 2.9.1

---

_@dependabot_

Bumps [ureq](https://github.com/algesten/ureq) from 2.8.0 to 2.9.1.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/algesten/ureq/blob/main/CHANGELOG.md">ureq's changelog</a>.</em></p>
<blockquote>
<h1>2.9.1</h1>
<h2>Fixed</h2>
<ul>
<li>Unbreak feature <code>http-interop</code>. This feature is version locked to http crate 0.2</li>
<li>New feature <code>http-crate</code>. This feature is for http crate 1.0</li>
<li>New feature <code>proxy-from-env</code> to detect proxy settings for global Agent (ureq::get).</li>
</ul>
<h1>2.9.0</h1>
<h2>Fixed</h2>
<ul>
<li>Broken rustls dep (introduced new function in patch version) (<a href="https://redirect.github.com/algesten/ureq/issues/677">#677</a>)</li>
<li>Doc and test fixes (<a href="https://redirect.github.com/algesten/ureq/issues/670">#670</a>, <a href="https://redirect.github.com/algesten/ureq/issues/673">#673</a>, <a href="https://redirect.github.com/algesten/ureq/issues/674">#674</a>)</li>
</ul>
<h2>Added</h2>
<ul>
<li>Upgraded http dep to 1.0</li>
<li>http_interop to not require utf-8 headers (<a href="https://redirect.github.com/algesten/ureq/issues/672">#672</a>)</li>
<li>http_interop implement conversion for <code>http::request::Parts</code> (<a href="https://redirect.github.com/algesten/ureq/issues/669">#669</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/algesten/ureq/commit/9092222fbd89f4ab73f6be8cb969069df72f7500"><code>9092222</code></a> 2.9.1</li>
<li><a href="https://github.com/algesten/ureq/commit/73690436cbff2bd48b2cfcc8738bbda8dfbeeda1"><code>7369043</code></a> Rename feature http -&gt; http-crate</li>
<li><a href="https://github.com/algesten/ureq/commit/87108e006d0a5d124e47aca0098b8f96ce4f2e24"><code>87108e0</code></a> Support both http 0.2 and 1.0</li>
<li><a href="https://github.com/algesten/ureq/commit/89a69a58344cf19e57d618fa4df29062c18e4b7e"><code>89a69a5</code></a> docs: update doc comment on <code>try_proxy_from_env</code> method to reflect the `proxy...</li>
<li><a href="https://github.com/algesten/ureq/commit/395218de3902ada1f4da16ede781c4b84cb25110"><code>395218d</code></a> feat: add a feature flag &quot;proxy-from-env&quot; to control whether or not to detect...</li>
<li><a href="https://github.com/algesten/ureq/commit/260213b3e2a55e40f7132374034520455d497789"><code>260213b</code></a> 2.9.0</li>
<li><a href="https://github.com/algesten/ureq/commit/c83ba95ac41e8b53942c9ebd914a48c2ac8df142"><code>c83ba95</code></a> Update changelog</li>
<li><a href="https://github.com/algesten/ureq/commit/07b8925f3b917e9bf6ffec5f3a9222c9797e06d6"><code>07b8925</code></a> Simpler version expression for rustls dep</li>
<li><a href="https://github.com/algesten/ureq/commit/c1bc86ad5844101231c6fc8eef7431bd1f5619b3"><code>c1bc86a</code></a> http 1.0</li>
<li><a href="https://github.com/algesten/ureq/commit/916ffbffbe098d28d9ac142054484fd71423d9a6"><code>916ffbf</code></a> Fix missing add_trust_anchors method due to lax rustls versioning</li>
<li>Additional commits viewable in <a href="https://github.com/algesten/ureq/compare/2.8.0...2.9.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=ureq&package-manager=cargo&previous-version=2.8.0&new-version=2.9.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-04 08:41_

---

_Comment by @github-actions[bot] on 2023-12-04 08:57_

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

_Merged by @zanieb on 2023-12-04 15:53_

---

_Closed by @zanieb on 2023-12-04 15:53_

---

_Branch deleted on 2023-12-04 15:53_

---
