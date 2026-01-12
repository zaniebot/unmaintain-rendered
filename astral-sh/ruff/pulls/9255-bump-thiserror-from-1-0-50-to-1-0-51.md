```yaml
number: 9255
title: Bump thiserror from 1.0.50 to 1.0.51
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/thiserror-1.0.51
created_at: 2023-12-23T05:41:50Z
updated_at: 2023-12-23T12:54:37Z
url: https://github.com/astral-sh/ruff/pull/9255
synced_at: 2026-01-12T15:55:28Z
```

# Bump thiserror from 1.0.50 to 1.0.51

---

_@dependabot_

Bumps [thiserror](https://github.com/dtolnay/thiserror) from 1.0.50 to 1.0.51.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/thiserror/releases">thiserror's releases</a>.</em></p>
<blockquote>
<h2>1.0.51</h2>
<ul>
<li>Improve diagnostics when an invalid attribute previously caused thiserror to generate no <code>Error</code> impl (<a href="https://redirect.github.com/dtolnay/thiserror/issues/266">#266</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/thiserror/commit/0555b805916067d898356fd67a5384606fbf8414"><code>0555b80</code></a> Release 1.0.51</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/b94add8c9ba7c01c5c109413cc3fb00021a66792"><code>b94add8</code></a> Add ui test where fallback impl conflicts with handwritten Display</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/02c6a5548072646d27e850d782f76c2473f4fb25"><code>02c6a55</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/thiserror/issues/266">#266</a> from dtolnay/fallback</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/1754825c24f51a1deec46a273b37c0fea32881f3"><code>1754825</code></a> Work around trivial bounds being unstable</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/1567f40ec3c00d2f228bd3b71e0f583ef5e52e88"><code>1567f40</code></a> Try to remove &quot;doesn't implement Debug&quot; in fallback expansion</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/d7e3bdd980520731862d8bcd83ad1955355b1bd3"><code>d7e3bdd</code></a> Fix redundant &quot;Error doesn't implement Display&quot; in fallback</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/7e5ff62806e75a89f92980420d64c4a460cb6751"><code>7e5ff62</code></a> Emit an Error impl even in the presence of bad attributes</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/0444cd5e66bd3fcdb2aefe9baeda8beb4c21178e"><code>0444cd5</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/thiserror/issues/265">#265</a> from dtolnay/fallbackfixme</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/b010e52359f3e68d29eff4b08a2890bac3fc1644"><code>b010e52</code></a> Add test looking for invalid input to still generate an impl</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/1c6c4bb593f15e79c65fc45dcdf78ce20da0cbbe"><code>1c6c4bb</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/thiserror/issues/264">#264</a> from dtolnay/errortraitpath</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/thiserror/compare/1.0.50...1.0.51">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=thiserror&package-manager=cargo&previous-version=1.0.50&new-version=1.0.51)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-23 05:41_

---

_Comment by @github-actions[bot] on 2023-12-23 05:59_

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

_Merged by @charliermarsh on 2023-12-23 12:44_

---

_Closed by @charliermarsh on 2023-12-23 12:44_

---

_Branch deleted on 2023-12-23 12:44_

---
