```yaml
number: 9272
title: Bump pep440_rs from 0.3.12 to 0.4.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/pep440_rs-0.4.0
created_at: 2023-12-25T08:22:45Z
updated_at: 2023-12-25T13:12:21Z
url: https://github.com/astral-sh/ruff/pull/9272
synced_at: 2026-01-12T15:55:28Z
```

# Bump pep440_rs from 0.3.12 to 0.4.0

---

_@dependabot_

Bumps [pep440_rs](https://github.com/konstin/pep440-rs) from 0.3.12 to 0.4.0.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/konstin/pep440-rs/blob/main/Changelog.md">pep440_rs's changelog</a>.</em></p>
<blockquote>
<h2>0.4</h2>
<ul>
<li>segments are now <code>u64</code> instead of <code>usize</code>. This ensures consistency between platforms and <code>u64</code> are required
when timestamps are used as patch versions (e.g., <code>20230628214621</code>, the ISO 8601 &quot;basic format&quot;)</li>
<li>Faster version comparison</li>
<li>Added <code>VersionSpecifier::equals_version</code> constructor for <code>==&lt;version&gt;</code></li>
<li>Added <code>VersionSpecifier::any_prerelease</code>: Whether the version marker includes a prerelease</li>
<li>Updated to pyo3 0.20</li>
<li>once_cell instead of lazy_static</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/konstin/pep440-rs/commit/821b9bb7be1841f081085a2f2850a80de8b67231"><code>821b9bb</code></a> v0.4.0</li>
<li><a href="https://github.com/konstin/pep440-rs/commit/a8303b01ffef6fccfdce562a887f6b110d482ef3"><code>a8303b0</code></a> Add missing docstring</li>
<li><a href="https://github.com/konstin/pep440-rs/commit/c3933eaffbe44be4d20b57e17f9a12e30917b48d"><code>c3933ea</code></a> Add VersionSpecifiers::<strong>contains</strong> for pyo3</li>
<li>See full diff in <a href="https://github.com/konstin/pep440-rs/compare/v0.3.12...v0.4.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=pep440_rs&package-manager=cargo&previous-version=0.3.12&new-version=0.4.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2023-12-25 08:22_

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
