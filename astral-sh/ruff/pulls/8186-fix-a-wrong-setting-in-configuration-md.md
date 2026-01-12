```yaml
number: 8186
title: Fix a wrong setting in configuration.md
type: pull_request
state: merged
author: lmanc
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-10-24T21:53:18Z
updated_at: 2023-10-24T22:05:09Z
url: https://github.com/astral-sh/ruff/pull/8186
synced_at: 2026-01-12T02:32:42Z
```

# Fix a wrong setting in configuration.md

---

_Pull request opened by @lmanc on 2023-10-24 21:53_

## Summary

The previous configuration for `ruff` contained an unrecognized field `magic-trailing-comma` set to "respect". As of version 0.1.2 of `ruff`, this field was not recognized and resulted in a TOML parse error when running the `ruff format .` command. This change removes the `magic-trailing-comma` field and adds the recognized `skip-magic-trailing-comma` field set to `false`.

## Test Plan

Tested locally with `ruff` 0.1.2.


---

_@zanieb approved on 2023-10-24 22:04_

Thanks!

---

_Label `documentation` added by @zanieb on 2023-10-24 22:05_

---

_Merged by @zanieb on 2023-10-24 22:05_

---

_Closed by @zanieb on 2023-10-24 22:05_

---
