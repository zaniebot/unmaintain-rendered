```yaml
number: 9672
title: Bump serde_with from 3.5.0 to 3.5.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/serde_with-3.5.1
created_at: 2024-01-29T09:08:37Z
updated_at: 2024-01-29T10:23:25Z
url: https://github.com/astral-sh/ruff/pull/9672
synced_at: 2026-01-10T22:57:09Z
```

# Bump serde_with from 3.5.0 to 3.5.1

---

_Pull request opened by @dependabot on 2024-01-29 09:08_

Bumps [serde_with](https://github.com/jonasbb/serde_with) from 3.5.0 to 3.5.1.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/jonasbb/serde_with/releases">serde_with's releases</a>.</em></p>
<blockquote>
<h2>serde_with v3.5.1</h2>
<h3>Fixed</h3>
<ul>
<li>The <code>serde_as</code> macro now better detects existing <code>schemars</code> attributes on fields and incorporates them by <a href="https://github.com/swlynch99"><code>@​swlynch99</code></a> (<a href="https://redirect.github.com/jonasbb/serde_with/issues/682">#682</a>)
This avoids errors on existing <code>#[schemars(with = ...)]</code> annotations.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/jonasbb/serde_with/commit/88e98795dc164b1ca8a1e8978402f0656ff4ca07"><code>88e9879</code></a> Merge pull request <a href="https://redirect.github.com/jonasbb/serde_with/issues/682">#682</a> from swlynch99/schemars-custom-with</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/1c9131ff64d52c47e260f898ab9ef7051f10085c"><code>1c9131f</code></a> Bump version to 3.5.1</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/bceb16e27fcac603ddc1712c4065dc45c43ef6c3"><code>bceb16e</code></a> Add changelog</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/215d77a5fe7b82735514a420be1e9e7a44edc9e4"><code>215d77a</code></a> Use #[serde_as] from import without crate name</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/b2afd766b761d016a68bc49968b4b114b90821a9"><code>b2afd76</code></a> Extend the test with an always existing condition</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/c09f33b394979c9370c7fffced3379e3652ac1f2"><code>c09f33b</code></a> Avoid emitting a #[schemars] annotation when one already exists</li>
<li>See full diff in <a href="https://github.com/jonasbb/serde_with/compare/v3.5.0...v3.5.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=serde_with&package-manager=cargo&previous-version=3.5.0&new-version=3.5.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-29 09:08_

---

_Comment by @github-actions[bot] on 2024-01-29 09:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_Merged by @MichaReiser on 2024-01-29 10:10_

---

_Closed by @MichaReiser on 2024-01-29 10:10_

---

_Branch deleted on 2024-01-29 10:10_

---
