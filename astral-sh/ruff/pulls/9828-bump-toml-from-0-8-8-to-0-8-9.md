```yaml
number: 9828
title: Bump toml from 0.8.8 to 0.8.9
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/toml-0.8.9
created_at: 2024-02-05T08:31:00Z
updated_at: 2024-02-05T13:05:28Z
url: https://github.com/astral-sh/ruff/pull/9828
synced_at: 2026-01-12T15:55:30Z
```

# Bump toml from 0.8.8 to 0.8.9

---

_@dependabot_

Bumps [toml](https://github.com/toml-rs/toml) from 0.8.8 to 0.8.9.
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/toml-rs/toml/commit/f5c6f4b473f71ff02bfd41202c13059e9eb12fb9"><code>f5c6f4b</code></a> chore: Release</li>
<li><a href="https://github.com/toml-rs/toml/commit/24e599ebf9f787ef99ff4ce718b02eb5d0fa1245"><code>24e599e</code></a> docs: Update changelog</li>
<li><a href="https://github.com/toml-rs/toml/commit/d00d616e2bb15f4e3ca401a081c295ce6820c455"><code>d00d616</code></a> Merge pull request <a href="https://redirect.github.com/toml-rs/toml/issues/670">#670</a> from epage/span</li>
<li><a href="https://github.com/toml-rs/toml/commit/7e239714d05121fbd9d111224f7a11a7d9d99134"><code>7e23971</code></a> fix(serde): Improve spans for empty tables</li>
<li><a href="https://github.com/toml-rs/toml/commit/d5423f623e831ef24aadc8cb469f760ad4c8bfad"><code>d5423f6</code></a> test(serde): Show bad span</li>
<li><a href="https://github.com/toml-rs/toml/commit/9db97b366b47b32db0f3d974856bc179f19f1d64"><code>9db97b3</code></a> Merge pull request <a href="https://redirect.github.com/toml-rs/toml/issues/668">#668</a> from JustusAdam/patch-1</li>
<li><a href="https://github.com/toml-rs/toml/commit/5381d7b920ec4a5540837a568b7b12b94ea499f4"><code>5381d7b</code></a> docs: Describe Table order based on concepts</li>
<li><a href="https://github.com/toml-rs/toml/commit/106d51f5c213cc4da136a29989bb8c45f0d3911f"><code>106d51f</code></a> test: Update compliance suite</li>
<li><a href="https://github.com/toml-rs/toml/commit/062e058d012e1d855a577473f84afdd8f711d349"><code>062e058</code></a> Merge pull request <a href="https://redirect.github.com/toml-rs/toml/issues/665">#665</a> from toml-rs/renovate/actions-setup-python-5.x</li>
<li><a href="https://github.com/toml-rs/toml/commit/0da2b51b25705f9abcbd93dec0f192bdccb7d370"><code>0da2b51</code></a> Merge pull request <a href="https://redirect.github.com/toml-rs/toml/issues/666">#666</a> from toml-rs/renovate/github-codeql-action-3.x</li>
<li>Additional commits viewable in <a href="https://github.com/toml-rs/toml/compare/toml-v0.8.8...toml-v0.8.9">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=toml&package-manager=cargo&previous-version=0.8.8&new-version=0.8.9)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-02-05 08:31_

---

_Comment by @github-actions[bot] on 2024-02-05 08:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>
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

_Merged by @MichaReiser on 2024-02-05 13:05_

---

_Closed by @MichaReiser on 2024-02-05 13:05_

---

_Branch deleted on 2024-02-05 13:05_

---
