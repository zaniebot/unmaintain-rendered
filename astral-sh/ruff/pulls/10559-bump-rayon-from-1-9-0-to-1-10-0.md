```yaml
number: 10559
title: Bump rayon from 1.9.0 to 1.10.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/rayon-1.10.0
created_at: 2024-03-25T08:52:32Z
updated_at: 2024-03-25T09:16:53Z
url: https://github.com/astral-sh/ruff/pull/10559
synced_at: 2026-01-10T22:47:02Z
```

# Bump rayon from 1.9.0 to 1.10.0

---

_Pull request opened by @dependabot on 2024-03-25 08:52_

Bumps [rayon](https://github.com/rayon-rs/rayon) from 1.9.0 to 1.10.0.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rayon-rs/rayon/blob/main/RELEASES.md">rayon's changelog</a>.</em></p>
<blockquote>
<h1>Release rayon 1.10.0 (2024-03-23)</h1>
<ul>
<li>The new methods <code>ParallelSlice::par_chunk_by</code> and
<code>ParallelSliceMut::par_chunk_by_mut</code> work like the slice methods <code>chunk_by</code>
and <code>chunk_by_mut</code> added in Rust 1.77.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rayon-rs/rayon/commit/4a6e9bf6f348c213d780c5a0eff000c011ce055e"><code>4a6e9bf</code></a> Merge <a href="https://redirect.github.com/rayon-rs/rayon/issues/991">#991</a></li>
<li><a href="https://github.com/rayon-rs/rayon/commit/b0008f31b168a99e55d224a728ff2a4ddc2fe11a"><code>b0008f3</code></a> Release rayon 1.6.0 / rayon-core 1.10.0</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/c2dfa5c8684d88c20b0ba27a8a3bf762cf96af92"><code>c2dfa5c</code></a> Merge <a href="https://redirect.github.com/rayon-rs/rayon/issues/990">#990</a></li>
<li><a href="https://github.com/rayon-rs/rayon/commit/17f5b08bb3d6df7393b4e7eb8fc3b7829e501fb9"><code>17f5b08</code></a> fix typo</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/ca9b279d8316285aebef9f736edc35933de3f023"><code>ca9b279</code></a> Merge <a href="https://redirect.github.com/rayon-rs/rayon/issues/989">#989</a></li>
<li><a href="https://github.com/rayon-rs/rayon/commit/a119f2323aca7fbf9e74b4b632e63161026b5b52"><code>a119f23</code></a> Unify <code>chunks</code>, <code>fold_chunks</code>, and <code>fold_chunks_with</code></li>
<li><a href="https://github.com/rayon-rs/rayon/commit/911d6d098c385ed07a66be7402ba3319d119a9c1"><code>911d6d0</code></a> Merge <a href="https://redirect.github.com/rayon-rs/rayon/issues/492">#492</a></li>
<li><a href="https://github.com/rayon-rs/rayon/commit/9ef85cd5d84966bc332eaa408c38be141f52e0d6"><code>9ef85cd</code></a> Add some documentation about <em>when</em> broadcasts run</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/bd7b61ca8bf2ec472c74d221adfc4f8b22d2d090"><code>bd7b61c</code></a> Add more internal enforcement of static/scope lifetimes</li>
<li><a href="https://github.com/rayon-rs/rayon/commit/812ca025aedddea8a4c7d8477146527b71b33e19"><code>812ca02</code></a> Simplify calls that use the panic_handler</li>
<li>Additional commits viewable in <a href="https://github.com/rayon-rs/rayon/compare/rayon-core-v1.9.0...rayon-core-v1.10.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=rayon&package-manager=cargo&previous-version=1.9.0&new-version=1.10.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-03-25 08:52_

---

_Comment by @github-actions[bot] on 2024-03-25 09:11_

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

_Merged by @MichaReiser on 2024-03-25 09:16_

---

_Closed by @MichaReiser on 2024-03-25 09:16_

---

_Branch deleted on 2024-03-25 09:16_

---
