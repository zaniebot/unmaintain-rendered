```yaml
number: 8126
title: Bump thiserror from 1.0.49 to 1.0.50
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/thiserror-1.0.50
created_at: 2023-10-23T08:52:51Z
updated_at: 2023-10-23T09:09:18Z
url: https://github.com/astral-sh/ruff/pull/8126
synced_at: 2026-01-12T02:32:41Z
```

# Bump thiserror from 1.0.49 to 1.0.50

---

_Pull request opened by @dependabot on 2023-10-23 08:52_

Bumps [thiserror](https://github.com/dtolnay/thiserror) from 1.0.49 to 1.0.50.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/thiserror/releases">thiserror's releases</a>.</em></p>
<blockquote>
<h2>1.0.50</h2>
<ul>
<li>Improve diagnostic when a #[source], #[from], or #[transparant] attribute refers to a type that has no std::error::Error impl (<a href="https://redirect.github.com/dtolnay/thiserror/issues/258">#258</a>, thanks <a href="https://github.com/de-vri-es"><code>@​de-vri-es</code></a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/thiserror/commit/a7d220d7915fb888413aa7978efd70f7006bda9d"><code>a7d220d</code></a> Release 1.0.50</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/4088d169edce8fe029935273bf8cbbc6f08f3a7d"><code>4088d16</code></a> Ignore module_name_repetitions pedantic clippy lint</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/ebebf77fe0a130769f92306d402e266ca30c512d"><code>ebebf77</code></a> Format ui tests with rustfmt</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/ff0a0a58590e9c12c1a2fe7d4dcb1d96e06eba26"><code>ff0a0a5</code></a> Source and From attributes only have single-ident path</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/7cec716420b0f0bfbec8a827955d810b0f9804ab"><code>7cec716</code></a> Remove reliance on Spanned for Member</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/c9fe739272dee48b0dedc6114780e1ca63c42ad3"><code>c9fe739</code></a> Touch up PR 258</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/4850c6f80fb9290c14ebf35051280f7ca4cf4de1"><code>4850c6f</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/thiserror/issues/258">#258</a> from de-vri-es/as-dyn-error-span</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/a49f7c603ddff7ecbfff1bf02dea100772621753"><code>a49f7c6</code></a> Change span of <code>as_dyn_error()</code> to point compile error at attribute.</li>
<li><a href="https://github.com/dtolnay/thiserror/commit/f4eac7ef7be36981b314d098049f6874af090e48"><code>f4eac7e</code></a> Ignore needless_raw_string_hashes clippy lint</li>
<li>See full diff in <a href="https://github.com/dtolnay/thiserror/compare/1.0.49...1.0.50">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=thiserror&package-manager=cargo&previous-version=1.0.49&new-version=1.0.50)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-10-23 08:52_

---

_Merged by @MichaReiser on 2023-10-23 09:03_

---

_Closed by @MichaReiser on 2023-10-23 09:03_

---

_Branch deleted on 2023-10-23 09:03_

---

_Comment by @github-actions[bot] on 2023-10-23 09:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
