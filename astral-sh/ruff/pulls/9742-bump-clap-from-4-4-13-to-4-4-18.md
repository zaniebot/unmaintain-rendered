```yaml
number: 9742
title: Bump clap from 4.4.13 to 4.4.18
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/clap-4.4.18
created_at: 2024-01-31T15:31:41Z
updated_at: 2024-01-31T15:50:23Z
url: https://github.com/astral-sh/ruff/pull/9742
synced_at: 2026-01-10T22:57:09Z
```

# Bump clap from 4.4.13 to 4.4.18

---

_Pull request opened by @dependabot on 2024-01-31 15:31_

Bumps [clap](https://github.com/clap-rs/clap) from 4.4.13 to 4.4.18.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/clap-rs/clap/releases">clap's releases</a>.</em></p>
<blockquote>
<h2>v4.4.18</h2>
<h2>[4.4.18] - 2024-01-16</h2>
<h3>Fixes</h3>
<ul>
<li><em>(error)</em> When lacking <code>usage</code> feature, ensure the list of required arguments is unique</li>
</ul>
<h2>v4.4.17</h2>
<h2>[4.4.17] - 2024-01-15</h2>
<h3>Fixes</h3>
<ul>
<li>Fix <code>panic!</code> when mixing <code>args_conflicts_with_subcommands</code> with <code>ArgGroup</code> (which is implicit with <code>derive</code>) introduced in 4.4.15</li>
</ul>
<h2>v4.4.16</h2>
<h2>[4.4.16] - 2024-01-12</h2>
<h3>Fixes</h3>
<ul>
<li>Ensure invalid escape sequences in user-defined strings are correctly stripped when terminal doesn't support color</li>
</ul>
<h2>v4.4.15</h2>
<h2>[4.4.15] - 2024-01-11</h2>
<h3>Fixes</h3>
<ul>
<li>Improve error for <code>args_conflicts_with_subcommands</code></li>
<li>Ensure we error for <code>args_conflicts_with_subcommands</code> when using subcommand short and long flags</li>
</ul>
<h2>v4.4.14</h2>
<h2>[4.4.14] - 2024-01-08</h2>
<h3>Documentation</h3>
<ul>
<li>Fix <code>find</code> cookbook entry to allow repeats of flags/options</li>
</ul>
<h3>Features</h3>
<ul>
<li>Allow <code>num_args(0)</code> on options which allows making them emulate being a flag for position-tracking flags</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/clap-rs/clap/blob/master/CHANGELOG.md">clap's changelog</a>.</em></p>
<blockquote>
<h2>[4.4.18] - 2024-01-16</h2>
<h3>Fixes</h3>
<ul>
<li><em>(error)</em> When lacking <code>usage</code> feature, ensure the list of required arguments is unique</li>
</ul>
<h2>[4.4.17] - 2024-01-15</h2>
<h3>Fixes</h3>
<ul>
<li>Fix <code>panic!</code> when mixing <code>args_conflicts_with_subcommands</code> with <code>ArgGroup</code> (which is implicit with <code>derive</code>) introduced in 4.4.15</li>
</ul>
<h2>[4.4.16] - 2024-01-12</h2>
<h3>Fixes</h3>
<ul>
<li>Ensure invalid escape sequences in user-defined strings are correctly stripped when terminal doesn't support color</li>
</ul>
<h2>[4.4.15] - 2024-01-11</h2>
<h3>Fixes</h3>
<ul>
<li>Improve error for <code>args_conflicts_with_subcommands</code></li>
<li>Ensure we error for <code>args_conflicts_with_subcommands</code> when using subcommand short and long flags</li>
</ul>
<h2>[4.4.14] - 2024-01-08</h2>
<h3>Documentation</h3>
<ul>
<li>Fix <code>find</code> cookbook entry to allow repeats of flags/options</li>
</ul>
<h3>Features</h3>
<ul>
<li>Allow <code>num_args(0)</code> on options which allows making them emulate being a flag for position-tracking flags</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/clap-rs/clap/commit/0134f45ff0e2e2be8c451565e4fbf5d3cb7b7cfd"><code>0134f45</code></a> chore: Release</li>
<li><a href="https://github.com/clap-rs/clap/commit/995ee032779d802606e599caf4f498ea51e92e82"><code>995ee03</code></a> docs: Update changelog</li>
<li><a href="https://github.com/clap-rs/clap/commit/2f1890907ed4e78674feeb96df34cfb813b84686"><code>2f18909</code></a> Merge pull request <a href="https://redirect.github.com/clap-rs/clap/issues/5314">#5314</a> from epage/required</li>
<li><a href="https://github.com/clap-rs/clap/commit/0a635b9a20077e2f932a9baee527052d8ed45d9e"><code>0a635b9</code></a> fix(parser): Don't duplicate requireds when usage disabled</li>
<li><a href="https://github.com/clap-rs/clap/commit/e648e086f3934afb40b121b5999b9e23657ddc28"><code>e648e08</code></a> Merge pull request <a href="https://redirect.github.com/clap-rs/clap/issues/5311">#5311</a> from sourcefrog/doc-exitcode</li>
<li><a href="https://github.com/clap-rs/clap/commit/8c83971b8c356b8c9abfbbb2320cb946a2ee8139"><code>8c83971</code></a> docs: Link to exit code info</li>
<li><a href="https://github.com/clap-rs/clap/commit/b250c0b5f5920b59e551bf0ec90e17c6103ae4a2"><code>b250c0b</code></a> Merge pull request <a href="https://redirect.github.com/clap-rs/clap/issues/5310">#5310</a> from epage/pty</li>
<li><a href="https://github.com/clap-rs/clap/commit/c742b8eb0ca648b645b616e064e00408944f390e"><code>c742b8e</code></a> chore(complete): Update completest-pty</li>
<li><a href="https://github.com/clap-rs/clap/commit/f524d84c1d3eca1c980c5150c750d9e00cbbdb0c"><code>f524d84</code></a> chore: Release</li>
<li><a href="https://github.com/clap-rs/clap/commit/944fb81cf593af1cd3a58dd959c934f0ff483182"><code>944fb81</code></a> docs: Update changelog</li>
<li>Additional commits viewable in <a href="https://github.com/clap-rs/clap/compare/v4.4.13...v4.4.18">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=clap&package-manager=cargo&previous-version=4.4.13&new-version=4.4.18)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-01-31 15:31_

---

_Merged by @charliermarsh on 2024-01-31 15:37_

---

_Closed by @charliermarsh on 2024-01-31 15:37_

---

_Branch deleted on 2024-01-31 15:37_

---

_Comment by @github-actions[bot] on 2024-01-31 15:50_

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
