```yaml
number: 12386
title: "Provide custom job permissions to `cargo-dist`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - release
assignees: []
merged: true
base: main
head: dhruv/perms-2
created_at: 2024-07-18T16:45:39Z
updated_at: 2024-07-18T17:04:07Z
url: https://github.com/astral-sh/ruff/pull/12386
synced_at: 2026-01-10T21:47:02Z
```

# Provide custom job permissions to `cargo-dist`

---

_Pull request opened by @dhruvmanila on 2024-07-18 16:45_

We can't just directly update the `release.yml` file because that's auto-generated using `cargo-dist`. So, update the permissions in `Cargo.toml` and then use `cargo dist generate` to make sure there's no diff.

---

_Label `release` added by @dhruvmanila on 2024-07-18 16:45_

---

_Merged by @dhruvmanila on 2024-07-18 16:49_

---

_Closed by @dhruvmanila on 2024-07-18 16:49_

---

_Branch deleted on 2024-07-18 16:49_

---

_Comment by @github-actions[bot] on 2024-07-18 17:04_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---
