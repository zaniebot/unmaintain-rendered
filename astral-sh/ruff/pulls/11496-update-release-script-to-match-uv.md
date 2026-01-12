```yaml
number: 11496
title: "Update release script to match `uv`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - release
assignees: []
merged: true
base: main
head: zb/release-sh
created_at: 2024-05-22T19:57:35Z
updated_at: 2024-07-03T12:35:30Z
url: https://github.com/astral-sh/ruff/pull/11496
synced_at: 2026-01-12T15:55:38Z
```

# Update release script to match `uv`

---

_@zanieb_

See https://github.com/astral-sh/uv/pull/3764

---

_Label `internal` added by @zanieb on 2024-05-22 19:57_

---

_Comment by @github-actions[bot] on 2024-05-22 20:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_Marked ready for review by @zanieb on 2024-05-22 21:40_

---

_@T-256 reviewed on 2024-05-22 22:46_

---

_Review comment by @T-256 on `scripts/release.sh`:20 on 2024-05-22 22:46_

```suggestion
uv tool run --from rooster-blue --isolated -- rooster contributors --quiet
```

---

_@zanieb reviewed on 2024-05-22 22:58_

---

_Review comment by @zanieb on `scripts/release.sh`:20 on 2024-05-22 22:58_

Ah thanks — I dropped the contributors display over in `uv`.

Maybe we should merge this one after #9559 

---

_@charliermarsh approved on 2024-05-23 19:50_

---

_@T-256 reviewed on 2024-06-26 09:14_

---

_Review comment by @T-256 on `CONTRIBUTING.md`:335 on 2024-06-26 09:14_

```suggestion
```
it is cached in user's data dir now.

---

_Label `release` added by @zanieb on 2024-07-03 04:43_

---

_@zanieb reviewed on 2024-07-03 04:43_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:335 on 2024-07-03 04:43_

It's technically not even cached, idk if this matters though.

---

_@MichaReiser approved on 2024-07-03 06:12_

Thanks. We probably need to update our contributing guidelines to mention how to install uv

---

_Merged by @zanieb on 2024-07-03 12:35_

---

_Closed by @zanieb on 2024-07-03 12:35_

---

_Branch deleted on 2024-07-03 12:35_

---
