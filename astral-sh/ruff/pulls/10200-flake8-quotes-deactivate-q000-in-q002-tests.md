```yaml
number: 10200
title: "[`flake8-quotes`] Deactivate `Q000` in `Q002`-tests"
type: pull_request
state: closed
author: robincaloudis
labels:
  - internal
assignees: []
base: main
head: rc-deactivate-q000
created_at: 2024-03-02T17:46:15Z
updated_at: 2024-03-16T16:56:53Z
url: https://github.com/astral-sh/ruff/pull/10200
synced_at: 2026-01-12T15:55:31Z
```

# [`flake8-quotes`] Deactivate `Q000` in `Q002`-tests

---

_@robincaloudis_

## Summary

During the work on https://github.com/astral-sh/ruff/pull/10199, I noticed that
the rule Q000 was used in tests that focus on
the behavior of docstring quotes (Q002). 
As deactivation leads to more granular tests, this 
change makes sure that the different test files 
keep their focus in the long term and do not
start to test something unrelated. Furthermore,
this keeps snap files to the bare minimum.

Opening this PR,as I do not want to have this 
changes in https://github.com/astral-sh/ruff/pull/10199, as both are logical unrelated 
to each other.

## Test Plan

Not needed. Will be implicitly tested.


---

_Renamed from "[`flake8-quotes`] Deactivate `Q000` in tests focusing on `Q001`` Q002` " to "[`flake8-quotes`] Deactivate `Q000` in tests focusing on `Q001` and `Q002` " by @robincaloudis on 2024-03-02 17:46_

---

_Renamed from "[`flake8-quotes`] Deactivate `Q000` in tests focusing on `Q001` and `Q002` " to "[`flake8-quotes`] Deactivate `Q000` in tests focusing on `Q002`" by @robincaloudis on 2024-03-02 17:46_

---

_Renamed from "[`flake8-quotes`] Deactivate `Q000` in tests focusing on `Q002`" to "[`flake8-quotes`] Deactivate `Q000` in `Q002`-tests" by @robincaloudis on 2024-03-02 17:47_

---

_Comment by @robincaloudis on 2024-03-02 17:55_

@charliermarsh, do you mind to review this PR? Thank you.

---

_Comment by @github-actions[bot] on 2024-03-02 18:04_

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

_Comment by @charliermarsh on 2024-03-03 23:44_

There is arguably some value here because it ensures that these rules _aren't_ flagged in the docstring tests, right? I.e., that there are no false positives for `Q000`?

---

_@MichaReiser requested changes on 2024-03-06 16:41_

I'm reassigning this back to you, to reply to charlie's question.

---

_Label `internal` added by @MichaReiser on 2024-03-06 16:41_

---

_Comment by @robincaloudis on 2024-03-16 16:56_

I close this PR as I totally agree with the reviewers comments. 

---

_Closed by @robincaloudis on 2024-03-16 16:56_

---
