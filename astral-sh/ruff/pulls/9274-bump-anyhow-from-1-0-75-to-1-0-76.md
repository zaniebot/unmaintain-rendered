```yaml
number: 9274
title: Bump anyhow from 1.0.75 to 1.0.76
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/anyhow-1.0.76
created_at: 2023-12-25T08:23:12Z
updated_at: 2023-12-25T13:12:41Z
url: https://github.com/astral-sh/ruff/pull/9274
synced_at: 2026-01-10T23:07:18Z
```

# Bump anyhow from 1.0.75 to 1.0.76

---

_Pull request opened by @dependabot on 2023-12-25 08:23_

Bumps [anyhow](https://github.com/dtolnay/anyhow) from 1.0.75 to 1.0.76.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/dtolnay/anyhow/releases">anyhow's releases</a>.</em></p>
<blockquote>
<h2>1.0.76</h2>
<ul>
<li>Opt in to <code>unsafe_op_in_unsafe_fn</code> lint (<a href="https://redirect.github.com/dtolnay/anyhow/issues/329">#329</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/dtolnay/anyhow/commit/5cad3bf6a2d6198ae29067c9aadf2f0828fdb125"><code>5cad3bf</code></a> Release 1.0.76</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/d371a49395a35ec7c69e5e659da8e776b5830838"><code>d371a49</code></a> Merge pull request <a href="https://redirect.github.com/dtolnay/anyhow/issues/329">#329</a> from dtolnay/unsafeop</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/07aac819f6735b74f66a6672bc6947d5a4f90237"><code>07aac81</code></a> Fill in unsafe blocks inside unsafe functions</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/5ea720de15481650f22d0c30d40d7c8507ced12b"><code>5ea720d</code></a> Turn on deny(unsafe_op_in_unsafe_fn)</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/539e831ce2f8a8d779acd21200a55d1bf185e6ec"><code>539e831</code></a> Detect whether unsafe_op_in_unsafe_fn lint is available</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/afb298e33916b2f79844a6ae166588eff749bbc1"><code>afb298e</code></a> Label the compiler versions in build.rs with a comment and link</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/a205cc95bed47265a4335881d705bef6f7c29b0e"><code>a205cc9</code></a> Add a funding file</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/05e413219e97f101d8f39a90902e5c5d39f951fe"><code>05e4132</code></a> Ignore struct_field_names pedantic clippy lint</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/98921f8ec8696da7680b025483b181e1b5869f6f"><code>98921f8</code></a> Remove 'remember to update' reminder from Cargo.toml</li>
<li><a href="https://github.com/dtolnay/anyhow/commit/1ae99051ea4c982e5fdeb9730ba97e819a61337f"><code>1ae9905</code></a> Ignore needless_raw_string_hashes clippy lint</li>
<li>Additional commits viewable in <a href="https://github.com/dtolnay/anyhow/compare/1.0.75...1.0.76">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=anyhow&package-manager=cargo&previous-version=1.0.75&new-version=1.0.76)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-25 08:23_

---

_Comment by @github-actions[bot] on 2023-12-25 08:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2023-12-25 13:12_

---

_Closed by @charliermarsh on 2023-12-25 13:12_

---

_Branch deleted on 2023-12-25 13:12_

---
