```yaml
number: 11585
title: "Add `tool.uv.build-constraint-dependencies` to `pyproject.toml`"
type: pull_request
state: merged
author: cning112
labels:
  - enhancement
assignees: []
merged: true
base: main
head: add-build-constraints-to-pyproject-toml
created_at: 2025-02-17T23:07:53Z
updated_at: 2025-02-18T01:58:36Z
url: https://github.com/astral-sh/uv/pull/11585
synced_at: 2026-01-10T11:10:38Z
```

# Add `tool.uv.build-constraint-dependencies` to `pyproject.toml`

---

_Pull request opened by @cning112 on 2025-02-17 23:07_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Resolves #6913. 

Add `tool.uv.build-constraint-dependencies` to pyproject.toml.
The changes are analogous to the constraint-dependencies feature implemented in #5248.

Add documentation for `build-constraint-dependencies`


## Test Plan

Add tests for `uv lock`, `uv add`, `uv pip install` and `uv pip compile`.


---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-18 01:01_

---

_Label `enhancement` added by @charliermarsh on 2025-02-18 01:01_

---

_@charliermarsh approved on 2025-02-18 01:21_

This is really thorough, thanks. I just tweaked some of the tests and docs.

---

_Comment by @charliermarsh on 2025-02-18 01:24_

I hope you'll consider continuing to contribute :)

---

_Renamed from "Add 'tool.uv.build-constraint-dependencies' to pyproject.toml" to "Add `tool.uv.build-constraint-dependencies` to `pyproject.toml`" by @charliermarsh on 2025-02-18 01:31_

---

_Merged by @charliermarsh on 2025-02-18 01:58_

---

_Closed by @charliermarsh on 2025-02-18 01:58_

---
