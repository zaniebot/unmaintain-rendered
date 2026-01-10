```yaml
number: 9275
title: Bump env_logger from 0.10.0 to 0.10.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/env_logger-0.10.1
created_at: 2023-12-25T08:23:25Z
updated_at: 2023-12-25T14:03:36Z
url: https://github.com/astral-sh/ruff/pull/9275
synced_at: 2026-01-10T23:07:18Z
```

# Bump env_logger from 0.10.0 to 0.10.1

---

_Pull request opened by @dependabot on 2023-12-25 08:23_

Bumps [env_logger](https://github.com/rust-cli/env_logger) from 0.10.0 to 0.10.1.
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/rust-cli/env_logger/blob/main/CHANGELOG.md">env_logger's changelog</a>.</em></p>
<blockquote>
<h2>[0.10.1] - 2023-11-10</h2>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/rust-cli/env_logger/commit/36623f573b13cc2ca7a041d052c858c2ac8da378"><code>36623f5</code></a> chore: Release env_logger version 0.10.1</li>
<li><a href="https://github.com/rust-cli/env_logger/commit/8a033d84385a27eb70665a697411827c7484705e"><code>8a033d8</code></a> chore: Fix packaging</li>
<li><a href="https://github.com/rust-cli/env_logger/commit/9df7e6c081d58edf86daf73666f0ad33fc44b0d4"><code>9df7e6c</code></a> Merge pull request <a href="https://redirect.github.com/rust-cli/env_logger/issues/241">#241</a> from ChrisDenton/simple-insert</li>
<li><a href="https://github.com/rust-cli/env_logger/commit/46ccdd94f532d125d6d2a95a9293f711d8399d5f"><code>46ccdd9</code></a> perf: Replace <code>HashMap</code> with a <code>Vec</code></li>
<li><a href="https://github.com/rust-cli/env_logger/commit/bdc96a421fe2027cb842e5732b04f8e62593f496"><code>bdc96a4</code></a> Merge pull request <a href="https://redirect.github.com/rust-cli/env_logger/issues/249">#249</a> from atouchet/v10</li>
<li><a href="https://github.com/rust-cli/env_logger/commit/983837c47b4c05a6103ff6a5a5f6dabc42a2007b"><code>983837c</code></a> Update links and remove broken badge</li>
<li><a href="https://github.com/rust-cli/env_logger/commit/dcd220dfaf1d731d7649d24807009a7229f5e2f2"><code>dcd220d</code></a> Update listed version number</li>
<li><a href="https://github.com/rust-cli/env_logger/commit/36b1508ea10e29bbb007d51a2f61b7f58d66c556"><code>36b1508</code></a> Merge pull request <a href="https://redirect.github.com/rust-cli/env_logger/issues/260">#260</a> from y-yagi/2018-edition</li>
<li><a href="https://github.com/rust-cli/env_logger/commit/6f64347c6a8645ef618f8b61dad37a53855c7fa3"><code>6f64347</code></a> Merge pull request <a href="https://redirect.github.com/rust-cli/env_logger/issues/282">#282</a> from epage/syntax</li>
<li><a href="https://github.com/rust-cli/env_logger/commit/b29735781a9f0d057a9cf13eb3cadf858a6c71de"><code>b297357</code></a> chore: Update docs and examples to 2018 edition</li>
<li>Additional commits viewable in <a href="https://github.com/rust-cli/env_logger/compare/v0.10.0...v0.10.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=env_logger&package-manager=cargo&previous-version=0.10.0&new-version=0.10.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Comment by @github-actions[bot] on 2023-12-25 08:40_

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

_Merged by @charliermarsh on 2023-12-25 14:03_

---

_Closed by @charliermarsh on 2023-12-25 14:03_

---

_Branch deleted on 2023-12-25 14:03_

---
