```yaml
number: 7283
title: "Update installation.md: Add x-cmd method to install uv"
type: pull_request
state: closed
author: lunrenyi
labels: []
assignees: []
base: main
head: patch-1
created_at: 2024-09-11T08:09:13Z
updated_at: 2024-09-11T19:39:06Z
url: https://github.com/astral-sh/uv/pull/7283
synced_at: 2026-01-10T12:53:44Z
```

# Update installation.md: Add x-cmd method to install uv

---

_Pull request opened by @lunrenyi on 2024-09-11 08:09_

## Summary

Add x-cmd method to install uv

- Hi, we have implemented a lightweight [package manager using shell and awk](https://www.x-cmd.com/pkg/). It helps you download uv release packages from the internet and extract them into a unified directory for management, without requiring root permissions.

- **I mean, can the installation method provided by x-cmd be added to the uv installation.md?**[The installation method for the x command](https://www.x-cmd.com/start/) and [uv demo](https://www.x-cmd.com/pkg/uv)
  ```sh
  x env use uv
  ```
## Test Plan

Nothing.



---

_Comment by @MichaReiser on 2024-09-11 19:38_

Hi @lunrenyi 

Thanks for contributing to both uv and Ruff to keep our documentation consistent. 

As I explained in the [corresponding Ruff PR]: We intentionally limit the documentation to the commonly used installation methods to keep the documentation brief. 

Thanks again, and good look with X-CMD. It looks exciting.

---

_Closed by @MichaReiser on 2024-09-11 19:38_

---
