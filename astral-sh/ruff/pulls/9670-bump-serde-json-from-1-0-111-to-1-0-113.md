```yaml
number: 9670
title: Bump serde_json from 1.0.111 to 1.0.113
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/serde_json-1.0.113
created_at: 2024-01-29T09:06:21Z
updated_at: 2024-01-29T10:00:56Z
url: https://github.com/astral-sh/ruff/pull/9670
synced_at: 2026-01-10T22:57:09Z
```

# Bump serde_json from 1.0.111 to 1.0.113

---

_Pull request opened by @dependabot on 2024-01-29 09:06_

Bumps [serde_json](https://github.com/serde-rs/json) from 1.0.111 to 1.0.113.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/serde-rs/json/releases">serde_json's releases</a>.</em></p>
<blockquote>
<h2>v1.0.113</h2>
<ul>
<li>Add <code>swap_remove</code> and <code>shift_remove</code> methods on Map (<a href="https://redirect.github.com/serde-rs/json/issues/1109">#1109</a>)</li>
</ul>
<h2>v1.0.112</h2>
<ul>
<li>Improve formatting of &quot;invalid type&quot; error messages involving floats (<a href="https://redirect.github.com/serde-rs/json/issues/1107">#1107</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/serde-rs/json/commit/09d865b34b9701be52764dc9bf571b1a16e9d3dc"><code>09d865b</code></a> Release 1.0.113</li>
<li><a href="https://github.com/serde-rs/json/commit/5aeab4eaf69d7959f013f8081865c264d6c00551"><code>5aeab4e</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/json/issues/1109">#1109</a> from serde-rs/remove</li>
<li><a href="https://github.com/serde-rs/json/commit/ca3c2ca3696cab79b8b279be7569ee1647250f1e"><code>ca3c2ca</code></a> Add swap_remove and shift_remove methods on Map</li>
<li><a href="https://github.com/serde-rs/json/commit/7fece969e3b480ec620419d65c2aeb08776bebcb"><code>7fece96</code></a> Release 1.0.112</li>
<li><a href="https://github.com/serde-rs/json/commit/6a6d2bbd9e8b8bd72573b863f12a4ec991f33232"><code>6a6d2bb</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/json/issues/1107">#1107</a> from serde-rs/unexpectedfloat</li>
<li><a href="https://github.com/serde-rs/json/commit/83d7bad54ba5db3a44198d6df0ff2e81621683fa"><code>83d7bad</code></a> Format f64 in error messages using ryu</li>
<li><a href="https://github.com/serde-rs/json/commit/107c2d1c42817f0d71f07a4d5b0ea2f29dbce8b8"><code>107c2d1</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/json/issues/1106">#1106</a> from serde-rs/invalidvalue</li>
<li><a href="https://github.com/serde-rs/json/commit/62ca3e4c01c2e62cd5c2a32e9104f386e5ce7808"><code>62ca3e4</code></a> Handle Unexpected::Unit in Error::invalid_value</li>
<li><a href="https://github.com/serde-rs/json/commit/296fafb8f32e8442ef8e4d5725c15ffca726b288"><code>296fafb</code></a> Factor out JSON-specific Display impl for serde::de::Unexpected</li>
<li><a href="https://github.com/serde-rs/json/commit/e56cc696bd7c112e5dd4ccfa23d094c3a1c1c1ff"><code>e56cc69</code></a> Merge pull request <a href="https://redirect.github.com/serde-rs/json/issues/1105">#1105</a> from keienWang/master</li>
<li>Additional commits viewable in <a href="https://github.com/serde-rs/json/compare/v1.0.111...v1.0.113">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=serde_json&package-manager=cargo&previous-version=1.0.111&new-version=1.0.113)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-29 09:06_

---

_Comment by @github-actions[bot] on 2024-01-29 09:25_

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

_Merged by @MichaReiser on 2024-01-29 10:00_

---

_Closed by @MichaReiser on 2024-01-29 10:00_

---

_Branch deleted on 2024-01-29 10:00_

---
