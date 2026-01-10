```yaml
number: 10152
title: " [`flake8-bugbear`] Avoid adding default initializers to stubs (`B006`)"
type: pull_request
state: merged
author: Philipp-Thiel
labels:
  - bug
assignees: []
merged: true
base: main
head: B006_stubs
created_at: 2024-02-28T15:46:00Z
updated_at: 2024-02-28T18:30:10Z
url: https://github.com/astral-sh/ruff/pull/10152
synced_at: 2026-01-10T22:57:10Z
```

#  [`flake8-bugbear`] Avoid adding default initializers to stubs (`B006`)

---

_Pull request opened by @Philipp-Thiel on 2024-02-28 15:46_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adapts the fix for rule B006 to no longer modify the body of function stubs, while retaining the change in method signature.

## Test Plan

<!-- How was it tested? -->

The existing tests for B006 were adapted to reflect this change in behavior.

## Relevant issue

https://github.com/astral-sh/ruff/issues/10083


---

_Comment by @github-actions[bot] on 2024-02-28 16:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Rule `S410` was removed and cannot be selected.
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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-28 17:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-28 17:39_

---

_Label `bug` added by @charliermarsh on 2024-02-28 17:39_

---

_@charliermarsh approved on 2024-02-28 18:11_

Thanks!

---

_Renamed from " B006 (unsafe fix) : does not fix function stubs correctly #10083 " to " [`flake8-bugbear`] Avoid adding default initializers to stubs (`B006`)" by @charliermarsh on 2024-02-28 18:12_

---

_Merged by @charliermarsh on 2024-02-28 18:19_

---

_Closed by @charliermarsh on 2024-02-28 18:19_

---
