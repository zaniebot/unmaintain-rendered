```yaml
number: 12316
title: Bump version to v0.5.2
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2024-07-14T01:13:15Z
updated_at: 2024-07-15T08:06:45Z
url: https://github.com/astral-sh/ruff/pull/12316
synced_at: 2026-01-12T15:55:40Z
```

# Bump version to v0.5.2

---

_@charliermarsh_

_No description provided._

---

_Label `release` added by @charliermarsh on 2024-07-14 01:13_

---

_Comment by @github-actions[bot] on 2024-07-14 01:36_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
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
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_Comment by @charliermarsh on 2024-07-14 01:40_

Will release this in the morning.

---

_@zanieb reviewed on 2024-07-14 04:31_

---

_Review comment by @zanieb on `CHANGELOG.md`:32 on 2024-07-14 04:31_

Just on Windows right?

---

_@zanieb approved on 2024-07-14 04:32_

---

_@MichaReiser approved on 2024-07-14 07:51_

Thanks!
@dhruvmanila should we also do an lsp/vs code release?

---

_Merged by @charliermarsh on 2024-07-14 14:43_

---

_Closed by @charliermarsh on 2024-07-14 14:43_

---

_Branch deleted on 2024-07-14 14:44_

---

_@charliermarsh reviewed on 2024-07-14 14:45_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:32 on 2024-07-14 14:45_

https://github.com/astral-sh/ruff/pull/12319

---

_Comment by @dhruvmanila on 2024-07-15 08:06_

No, I don't think we should bump the version of VS Code extension, specifically because of https://github.com/astral-sh/ruff-vscode/pull/518 which we need to tie with the "stabilized" server release (https://github.com/astral-sh/ruff/pull/12208).

---
