```yaml
number: 12381
title: Bump version to 0.5.3
type: pull_request
state: merged
author: dhruvmanila
labels:
  - release
  - no-build
assignees: []
merged: true
base: main
head: dhruv/bump
created_at: 2024-07-18T15:52:20Z
updated_at: 2024-07-18T16:23:22Z
url: https://github.com/astral-sh/ruff/pull/12381
synced_at: 2026-01-10T21:47:02Z
```

# Bump version to 0.5.3

---

_Pull request opened by @dhruvmanila on 2024-07-18 15:52_

_No description provided._

---

_Label `release` added by @dhruvmanila on 2024-07-18 15:52_

---

_@charliermarsh reviewed on 2024-07-18 15:58_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:43 on 2024-07-18 15:58_

Nit: "Wasm"

---

_@AlexWaygood approved on 2024-07-18 15:59_

---

_Label `no-build` added by @dhruvmanila on 2024-07-18 16:03_

---

_Merged by @dhruvmanila on 2024-07-18 16:07_

---

_Closed by @dhruvmanila on 2024-07-18 16:07_

---

_Branch deleted on 2024-07-18 16:07_

---

_@charliermarsh reviewed on 2024-07-18 16:08_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:5 on 2024-07-18 16:08_

Maybe:

**Ruff 0.5.3 marks the stable release of the Ruff language server and introduces revamped [documentation](https://docs.astral.sh/ruff/editors), including [setup guides for your editor of choice](https://docs.astral.sh/ruff/editors/setup) and [the language server itself](https://docs.astral.sh/ruff/editors/settings)**.


---

_@dhruvmanila reviewed on 2024-07-18 16:11_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:5 on 2024-07-18 16:11_

0.5.3 seems a bit redundant given the heading, otherwise I like it.

---

_@dhruvmanila reviewed on 2024-07-18 16:14_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:5 on 2024-07-18 16:14_

Actually, I guess it's fine. It's similar to the one for the beta release.


---

_Comment by @github-actions[bot] on 2024-07-18 16:23_

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
