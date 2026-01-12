```yaml
number: 8611
title: "Fix docs typo for `ruff format` preview configuration"
type: pull_request
state: merged
author: sjdemartini
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-preview-format-docs-typo
created_at: 2023-11-11T01:54:24Z
updated_at: 2023-11-17T22:40:55Z
url: https://github.com/astral-sh/ruff/pull/8611
synced_at: 2026-01-12T15:55:26Z
```

# Fix docs typo for `ruff format` preview configuration

---

_@sjdemartini_

## Summary

The ruff configuration section is called "format", rather than "preview". Using the configuration as it was written in the docs gives an error:

```
$ ruff format --check .
ruff failed
  Cause: TOML parse error at line 143, column 1
    |
143 | [tool.ruff.preview]
    | ^^^^^^^^^^^^^^^^^^^
invalid type: map, expected a boolean
```

## Test Plan

Tested running `ruff format` with the following in my `pyproject.toml`:

```toml
[tool.ruff.format]
preview = true
```

and it worked properly (using preview rules for formatting).

---

_@zanieb approved on 2023-11-11 01:56_

Thank you for contributing!

---

_Label `documentation` added by @zanieb on 2023-11-11 01:56_

---

_Merged by @zanieb on 2023-11-11 03:07_

---

_Closed by @zanieb on 2023-11-11 03:07_

---

_Branch deleted on 2023-11-17 22:40_

---
