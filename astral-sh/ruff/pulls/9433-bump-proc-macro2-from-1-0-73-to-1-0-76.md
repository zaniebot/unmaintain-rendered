```yaml
number: 9433
title: Bump proc-macro2 from 1.0.73 to 1.0.76
type: pull_request
state: closed
author: dependabot
labels:
  - internal
assignees: []
base: main
head: dependabot/cargo/proc-macro2-1.0.76
created_at: 2024-01-08T08:50:39Z
updated_at: 2024-01-08T09:10:00Z
url: https://github.com/astral-sh/ruff/pull/9433
synced_at: 2026-01-10T22:57:09Z
```

# Bump proc-macro2 from 1.0.73 to 1.0.76

---

_Pull request opened by @dependabot on 2024-01-08 08:50_

Bumps [proc-macro2](https://github.com/dtolnay/proc-macro2) from 1.0.73 to 1.0.76.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/proc-macro2/releases">proc-macro2's releases</a>.</em></p>
<blockquote>
<h2>1.0.76</h2>
<ul>
<li>Work around <code>dead_code</code> warning false positive (<a href="https://redirect.github.com/dtolnay/proc-macro2/issues/435">#435</a>)</li>
</ul>
<h2>1.0.75</h2>
<ul>
<li>Improve error messages related to proc_macro::LexError (<a href="https://redirect.github.com/dtolnay/proc-macro2/issues/434">#434</a>)</li>
</ul>
<h2>1.0.74</h2>
<ul>
<li>Work around improperly cached build script result by sccache (<a href="https://redirect.github.com/dtolnay/proc-macro2/issues/432">#432</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/ea778eb80729a01094b033c45633122052d96fd4"><code>ea778eb</code></a> Release 1.0.76</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/534bb8177f7a3ebc7bc9c265e4ae79e134711325"><code>534bb81</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/435">#435</a> from dtolnay/deadcode</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/436ce07eedae4db7e0138d3d8d55d8666b2ba3a4"><code>436ce07</code></a> Add link to dead_code autotrait bug</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/228503f96664cd47476e6a58efa1d3f4145674ed"><code>228503f</code></a> Work around new dead_code warning</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/2a9b955ec822f0483e796c103768f43c47600b71"><code>2a9b955</code></a> Release 1.0.75</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/6d2899c17e349937eff6db7a79387373af97a500"><code>6d2899c</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/434">#434</a> from dtolnay/mismatch</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/662426c587936e06dbe527caa9d941d23b684e4e"><code>662426c</code></a> Fix compiler/fallback mismatch when catching rustc lex error panic</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/4ae50dba501a349329c381345f56f5f0ceb969fb"><code>4ae50db</code></a> Distinguish which direction the mismatch occurs</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/cb94081585da1ceea3557ed93a39d723f5c89cc9"><code>cb94081</code></a> Add a way to get mismatch backtrace using cfg</li>
<li><a href="https://github.com/dtolnay/proc-macro2/commit/3eb18578d53228cc9f9e2514f1df94c4aebd8504"><code>3eb1857</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/proc-macro2/issues/433">#433</a> from dtolnay/mismatch</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/proc-macro2/compare/1.0.73...1.0.76">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=proc-macro2&package-manager=cargo&previous-version=1.0.73&new-version=1.0.76)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-08 08:50_

---

_Comment by @dependabot[bot] on 2024-01-08 08:57_

Looks like proc-macro2 is up-to-date now, so this is no longer needed.

---

_Closed by @dependabot[bot] on 2024-01-08 08:57_

---

_Branch deleted on 2024-01-08 08:57_

---

_Comment by @github-actions[bot] on 2024-01-08 09:09_

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
