```yaml
number: 10176
title: "[`pylint`] Implement `useless-exception-statement` (`W0133`)"
type: pull_request
state: merged
author: mikeleppane
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: rule(pylint)/useless_exception_statement_PLW0133
created_at: 2024-02-29T17:10:45Z
updated_at: 2024-03-01T15:34:19Z
url: https://github.com/astral-sh/ruff/pull/10176
synced_at: 2026-01-12T15:55:31Z
```

# [`pylint`] Implement `useless-exception-statement` (`W0133`)

---

_@mikeleppane_

## Summary

This review contains a new rule for handling `useless exception statements` (`PLW0133`). Is it based on the following pylint's rule: [W0133](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/pointless-exception-statement.html)


Note: this rule does not cover the case if an error is a custom exception class.

See: [Rule request](https://github.com/astral-sh/ruff/issues/10145)

## Test Plan

```bash
cargo test & manually
```


---

_Comment by @github-actions[bot] on 2024-02-29 18:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/cc1776a76788ac684f7f03bde720b5e22b21606c/disnake/state.py#L456'>disnake/state.py:456:13:</a> PLW0133 Missing `raise` statement on exception
+ <a href='https://github.com/DisnakeDev/disnake/blob/cc1776a76788ac684f7f03bde720b5e22b21606c/disnake/state.py#L476'>disnake/state.py:476:13:</a> PLW0133 Missing `raise` statement on exception
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0133 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

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

_Comment by @mikeleppane on 2024-02-29 19:08_

False positive for custom exception class using initializer:

https://github.com/aiven/aiven-client/blob/55047f68a02ea339ccd9546e1b95088a10ebc8b6/aiven/client/client.py#L34

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-01 02:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-01 02:07_

---

_Label `rule` added by @charliermarsh on 2024-03-01 02:07_

---

_Label `preview` added by @charliermarsh on 2024-03-01 02:07_

---

_Renamed from "rule(PLW0133): Implement useless exception statement" to "[`pylint`] Implement `useless-exception-statement` (`W0133`)" by @charliermarsh on 2024-03-01 02:07_

---

_Comment by @charliermarsh on 2024-03-01 02:12_

Reviewing now...

---

_Merged by @charliermarsh on 2024-03-01 02:37_

---

_Closed by @charliermarsh on 2024-03-01 02:37_

---
