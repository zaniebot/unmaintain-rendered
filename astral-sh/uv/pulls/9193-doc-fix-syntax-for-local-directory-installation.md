```yaml
number: 9193
title: DOC - Fix syntax for local directory installation
type: pull_request
state: merged
author: trallard
labels:
  - documentation
assignees: []
merged: true
base: main
head: trallard/patch-docs-pip
created_at: 2024-11-18T12:33:53Z
updated_at: 2024-11-18T13:31:09Z
url: https://github.com/astral-sh/uv/pull/9193
synced_at: 2026-01-12T16:08:41Z
```

# DOC - Fix syntax for local directory installation

---

_@trallard_

## Summary

This is a minor fix as the command in https://docs.astral.sh/uv/pip/packages/#installing-a-package to install projects in editable mode from local directories results in the following error:

```
error: Failed to parse: `@`
  Caused by: Expected package name starting with an alphanumeric character, found `@`
```

This PR adds the missing `"`

## Test Plan

<!-- How was it tested? -->
Using the fixed syntax here does not result in the above error, and packages are correctly installed ✨ 


---

_@charliermarsh approved on 2024-11-18 13:30_

---

_Merged by @charliermarsh on 2024-11-18 13:31_

---

_Closed by @charliermarsh on 2024-11-18 13:31_

---

_Comment by @charliermarsh on 2024-11-18 13:31_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-11-18 13:31_

---
