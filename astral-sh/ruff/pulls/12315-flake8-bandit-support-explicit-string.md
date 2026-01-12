```yaml
number: 12315
title: "[`flake8-bandit`] Support explicit string concatenations in S310 HTTP detection"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/s310
created_at: 2024-07-13T23:06:54Z
updated_at: 2024-07-14T14:44:10Z
url: https://github.com/astral-sh/ruff/pull/12315
synced_at: 2026-01-12T15:55:40Z
```

# [`flake8-bandit`] Support explicit string concatenations in S310 HTTP detection

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/12314.

---

_Label `bug` added by @charliermarsh on 2024-07-13 23:06_

---

_Comment by @github-actions[bot] on 2024-07-13 23:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f23fdede6750f335af3c0e3049edab12faf56d12/scripts/lib/puppet_cache.py#L81'>scripts/lib/puppet_cache.py:81:14:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S310 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f23fdede6750f335af3c0e3049edab12faf56d12/scripts/lib/puppet_cache.py#L81'>scripts/lib/puppet_cache.py:81:14:</a> S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S310 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

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

_Merged by @charliermarsh on 2024-07-14 14:44_

---

_Closed by @charliermarsh on 2024-07-14 14:44_

---

_Branch deleted on 2024-07-14 14:44_

---
