```yaml
number: 10124
title: Bump bstr from 1.9.0 to 1.9.1
type: pull_request
state: merged
author: dependabot
labels:
  - internal
assignees: []
merged: true
base: main
head: dependabot/cargo/bstr-1.9.1
created_at: 2024-02-26T09:05:02Z
updated_at: 2024-02-26T09:23:46Z
url: https://github.com/astral-sh/ruff/pull/10124
synced_at: 2026-01-12T15:55:31Z
```

# Bump bstr from 1.9.0 to 1.9.1

---

_@dependabot_

Bumps [bstr](https://github.com/BurntSushi/bstr) from 1.9.0 to 1.9.1.
<details>
<summary>Commits</summary>
<ul>
<li><a href="https://github.com/BurntSushi/bstr/commit/4f41e0b68c9d5c2aa5e675a357b2adac75f9aa53"><code>4f41e0b</code></a> 1.9.1</li>
<li><a href="https://github.com/BurntSushi/bstr/commit/8c4693531cf35049f06b80603444042546426155"><code>8c46935</code></a> style: rejigger imports</li>
<li><a href="https://github.com/BurntSushi/bstr/commit/7b6e0259850dfbcfa4707e88e3a6a4f2a1ad4b34"><code>7b6e025</code></a> io: read of zero bytes should quit instantly</li>
<li><a href="https://github.com/BurntSushi/bstr/commit/cc13102d64de7b43efaf804046065b68f6c9a0c0"><code>cc13102</code></a> bench: fix broken benchmark</li>
<li>See full diff in <a href="https://github.com/BurntSushi/bstr/compare/1.9.0...1.9.1">compare view</a></li>
</ul>
</details>
<br />


[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=bstr&package-manager=cargo&previous-version=1.9.0&new-version=1.9.1)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

_Label `internal` added by @dependabot[bot] on 2024-02-26 09:05_

---

_Merged by @MichaReiser on 2024-02-26 09:14_

---

_Closed by @MichaReiser on 2024-02-26 09:14_

---

_Branch deleted on 2024-02-26 09:14_

---

_Comment by @github-actions[bot] on 2024-02-26 09:23_

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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---
