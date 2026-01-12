```yaml
number: 10149
title: "Implement isort's `default-section` setting"
type: pull_request
state: merged
author: mmerickel
labels:
  - isort
  - configuration
assignees: []
merged: true
base: main
head: isort-default-section
created_at: 2024-02-28T03:55:40Z
updated_at: 2024-03-01T03:50:20Z
url: https://github.com/astral-sh/ruff/pull/10149
synced_at: 2026-01-12T15:55:31Z
```

# Implement isort's `default-section` setting

---

_@mmerickel_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This fixes https://github.com/astral-sh/ruff/issues/7868.

Support isort's `default-section` feature which allows any imports that match sections that are not in `section-order` to be mapped to a specifically named section.

https://pycqa.github.io/isort/docs/configuration/options.html#default-section

This has a few implications:

- It is no longer required that all known sections are defined in `section-order`.
- This is technically a bw-incompat change because currently if folks define custom groups, and do not define a `section-order`, the code used to add all known sections to `section-order` while emitting warnings. **However, when this happened, users would be seeing warnings so I do not think it should count as a bw-incompat change.**

## Test Plan

- Added a new test.
- Did not break any existing tests.

Finally, I ran the following config against Pyramid's complex codebase that was previously using isort and this change worked there.

### pyramid's previous isort config

https://github.com/Pylons/pyramid/blob/5f7e286b0629b0a5f1225fe51172cba77eb8fda1/pyproject.toml#L22-L37

```toml
[tool.isort]
profile = "black"
multi_line_output = 3
src_paths = ["src", "tests"]
skip_glob = ["docs/*"]
include_trailing_comma = true
force_grid_wrap = false
combine_as_imports = true
line_length = 79
force_sort_within_sections = true
no_lines_before = "THIRDPARTY"
sections = "FUTURE,THIRDPARTY,FIRSTPARTY,LOCALFOLDER"
default_section = "THIRDPARTY"
known_first_party = "pyramid"
```

### tested with ruff isort config

```toml
[tool.ruff.lint.isort]
case-sensitive = true
combine-as-imports = true
force-sort-within-sections = true
section-order = [
    "future",
    "third-party",
    "first-party",
    "local-folder",
]
default-section = "third-party"
known-first-party = [
    "pyramid",
]
```

---

_Comment by @github-actions[bot] on 2024-02-28 04:08_

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

_Renamed from "add tool.ruff.isort.default-section" to "add ruff.lint.isort.default-section" by @mmerickel on 2024-02-28 04:16_

---

_Comment by @archoversight on 2024-02-28 21:45_

This will allow me to drop my dependency on `isort` in projects and just use `ruff` for it all. :+1:

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-29 14:57_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-29 14:57_

---

_Label `isort` added by @charliermarsh on 2024-02-29 14:57_

---

_Label `configuration` added by @charliermarsh on 2024-02-29 14:57_

---

_@charliermarsh approved on 2024-03-01 02:53_

This is good work -- thanks @mmerickel!

I'll admit that I have _some_ hesitancy to support this setting. I think it's possible to come up with a more intuitive and user-friendly configuration scheme for the use-case that motivated the change (e.g., if we want users to be able to merge standard library and third-party imports, perhaps we should focus on how to enable _that_?). At the same time, I understand why it's helpful to follow isort here.


---

_Renamed from "add ruff.lint.isort.default-section" to "Implement isort's `default-section` setting" by @charliermarsh on 2024-03-01 03:25_

---

_Merged by @charliermarsh on 2024-03-01 03:32_

---

_Closed by @charliermarsh on 2024-03-01 03:32_

---

_Comment by @mmerickel on 2024-03-01 03:44_

Thank you @charliermarsh!

---

_Comment by @charliermarsh on 2024-03-01 03:50_

Thank you! Nice PR.

---
