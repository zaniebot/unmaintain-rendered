```yaml
number: 14001
title: "Add `UV_PACKAGE` environment variable"
type: pull_request
state: open
author: pedro-avalos
labels: []
assignees: []
base: main
head: add-uv-package-envvar
created_at: 2025-06-12T16:46:38Z
updated_at: 2025-06-12T17:10:32Z
url: https://github.com/astral-sh/uv/pull/14001
synced_at: 2026-01-12T16:10:57Z
```

# Add `UV_PACKAGE` environment variable

---

_@pedro-avalos_

## Summary

Resolves #13826.

This PR introduces the environment variable `UV_PACKAGE`, which is the same as `--package` for commands that execute in a workspace (e.g., `uv build`).


## Test Plan

![image](https://github.com/user-attachments/assets/54a27482-8d56-40a1-9b9e-219b253fd1e6)



---
