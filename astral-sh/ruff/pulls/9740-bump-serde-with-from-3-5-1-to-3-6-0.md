```yaml
number: 9740
title: Bump serde_with from 3.5.1 to 3.6.0
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/serde_with-3.6.0
created_at: 2024-01-31T15:30:45Z
updated_at: 2024-01-31T15:49:30Z
url: https://github.com/astral-sh/ruff/pull/9740
synced_at: 2026-01-12T15:55:30Z
```

# Bump serde_with from 3.5.1 to 3.6.0

---

_@dependabot_

Bumps [serde_with](https://github.com/jonasbb/serde_with) from 3.5.1 to 3.6.0.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/jonasbb/serde_with/releases">serde_with's releases</a>.</em></p>
<blockquote>
<h2>serde_with v3.6.0</h2>
<h3>Added</h3>
<ul>
<li>Add <code>IfIsHumanReadable</code> for conditional implementation by <a href="https://github.com/irriden"><code>@​irriden</code></a> (<a href="https://redirect.github.com/jonasbb/serde_with/issues/690">#690</a>)
Used to specify different transformations for text-based and binary formats.</li>
<li>Add more <code>JsonSchemaAs</code> impls for all <code>Duration*</code> and <code>Timestamp*</code> adaptors by <a href="https://github.com/swlynch99"><code>@​swlynch99</code></a> (<a href="https://redirect.github.com/jonasbb/serde_with/issues/685">#685</a>)</li>
</ul>
<h3>Changed</h3>
<ul>
<li>Bump MSRV to 1.65, since that is required for the <code>regex</code> dependency.</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/jonasbb/serde_with/commit/1be8fa3f81c98e0331c0e6186e5846ba6179836a"><code>1be8fa3</code></a> Bump version to 3.6.0 (<a href="https://redirect.github.com/jonasbb/serde_with/issues/692">#692</a>)</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/8d38d10dca3a50907611796167b6273cf61d4293"><code>8d38d10</code></a> Bump version to 3.6.0</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/857a523133a9b3a854ee15764ad36c211fe4357d"><code>857a523</code></a> Add IfIsHumanReadable (<a href="https://redirect.github.com/jonasbb/serde_with/issues/690">#690</a>)</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/6d356b517bb168d6c6740645bffef0a5b39f47d2"><code>6d356b5</code></a> Merge branch 'master' into human-readable</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/eac8871c0354f228add6b0260487f37e4090aa5b"><code>eac8871</code></a> Bump regex from 1.9.6 to 1.10.3 (<a href="https://redirect.github.com/jonasbb/serde_with/issues/686">#686</a>)</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/dca18c41bf44de882568aade624e8d4300ee8b84"><code>dca18c4</code></a> Add JsonSchemaAs impls for all Duration and Timestamp adaptors (<a href="https://redirect.github.com/jonasbb/serde_with/issues/685">#685</a>)</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/cf295734f59e9fbabc63ddbf114b04ca1905009e"><code>cf29573</code></a> Add a pattern key for the duration string</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/47d974f30b85ea41c24735cca7f27d318dd31a97"><code>47d974f</code></a> Add IfIsHumanReadable</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/00382b9fdc1217953be6392b98e74f3a92923638"><code>00382b9</code></a> Merge pull request <a href="https://redirect.github.com/jonasbb/serde_with/issues/689">#689</a> from jonasbb/devcontainer</li>
<li><a href="https://github.com/jonasbb/serde_with/commit/d2b859a7ab79a46dbf794bf4504f66ea8eb32e5d"><code>d2b859a</code></a> Switch installation of pre-commit to postCreateCommand</li>
<li>Additional commits viewable in <a href="https://github.com/jonasbb/serde_with/compare/v3.5.1...v3.6.0">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=serde_with&package-manager=cargo&previous-version=3.5.1&new-version=3.6.0)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-31 15:30_

---

_Merged by @charliermarsh on 2024-01-31 15:48_

---

_Closed by @charliermarsh on 2024-01-31 15:48_

---

_Branch deleted on 2024-01-31 15:48_

---

_Comment by @github-actions[bot] on 2024-01-31 15:49_

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
