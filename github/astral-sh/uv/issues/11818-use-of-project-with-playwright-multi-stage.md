---
number: 11818
title: Use of project with playwright -- multi-stage package install
type: issue
state: open
author: mcint
labels:
  - question
assignees: []
created_at: 2025-02-27T02:32:48Z
updated_at: 2025-02-28T13:19:15Z
url: https://github.com/astral-sh/uv/issues/11818
synced_at: 2026-01-07T13:12:18-06:00
---

# Use of project with playwright -- multi-stage package install

---

_Issue opened by @mcint on 2025-02-27 02:32_

### Question

I want to use a heavy weight package, playwright, and, while I can install it and use it as a `tool`, when I want to run it as part of a project, the dependency is not available.

- It cannot be simply included as a dependency, because it requires a follow-up `playwright install-deps [...]` step, which downloads a browser.
- I don't know if I can, nor how to, nest virtualenvs, for use of a heavier dependency shared between projects that doesn't need to be updated as often, and would be better not to install multiple times.

What's the best, uv-idiomatic way to achieve this task?

### Platform

Linux 5.15.0-131-generic x86_64 GNU/Linux

### Version

uv 0.5.31                                                                                                                                  

---

_Label `question` added by @mcint on 2025-02-27 02:32_

---

_Comment by @konstin on 2025-02-28 13:19_

On my machine (ubuntu 24.04) `playwright install-deps` wants to install system dependencies, that would be installed once per machine, but doesn't change the venv, and browser seem to be installed per-user. Are there any modifications that need to made to the venv? Otherwise uv hardlinks dependency files from a cache on installation, so installing a package multiple times is generally cheap.

---
