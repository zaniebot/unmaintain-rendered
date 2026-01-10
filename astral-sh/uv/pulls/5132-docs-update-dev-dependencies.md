```yaml
number: 5132
title: "docs: update dev dependencies"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/update-dev-dependencies
created_at: 2024-07-16T21:14:28Z
updated_at: 2024-07-16T21:22:15Z
url: https://github.com/astral-sh/uv/pull/5132
synced_at: 2026-01-10T13:42:52Z
```

# docs: update dev dependencies

---

_Pull request opened by @mkniewallner on 2024-07-16 21:14_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

While playing out with `uv` preview features, I've noticed 2 issues with the development dependencies documentation:
- it is mentioned that the feature is not implemented, but it looks like it actually is
- despite what is said, it doesn't seem that it's possible to use a map for development dependencies yet:
  ```toml
  [tool.uv.dev-dependencies]
  test = [
      "pytest >=8.1.1,<9"
  ]
  lint = [
      "mypy >=1,<2"
  ]

  [tool.uv]
  default-dev-dependencies = ["test"]
  ```

  ```console
  $ uv sync --preview
  error: Failed to parse: `pyproject.toml`
    Caused by: TOML parse error at line 32, column 1
     |
  32 | [tool.uv.dev-dependencies]
     | ^^^^^^^^^^^^^^^^^^^^^^^^^^
  invalid type: map, expected a sequence
  ```

---

_Marked ready for review by @mkniewallner on 2024-07-16 21:15_

---

_@zanieb approved on 2024-07-16 21:20_

Thanks! We haven't revisited this doc since the draft but we will soon.

---

_Merged by @zanieb on 2024-07-16 21:20_

---

_Closed by @zanieb on 2024-07-16 21:20_

---

_Label `documentation` added by @zanieb on 2024-07-16 21:20_

---

_Branch deleted on 2024-07-16 21:22_

---
