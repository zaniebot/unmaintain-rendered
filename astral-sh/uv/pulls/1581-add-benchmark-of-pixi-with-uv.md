```yaml
number: 1581
title: Add benchmark of pixi with uv
type: pull_request
state: closed
author: olivier-lacroix
labels:
  - documentation
  - performance
assignees: []
draft: true
base: main
head: pixi-bench
created_at: 2024-02-17T10:19:09Z
updated_at: 2024-02-21T18:15:53Z
url: https://github.com/astral-sh/uv/pull/1581
synced_at: 2026-01-10T14:54:43Z
```

# Add benchmark of pixi with uv

---

_Pull request opened by @olivier-lacroix on 2024-02-17 10:19_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds Pixi as one of the tool benchmarked against uv.

- Pixi can generate multi-platform lock files. here, lock-files are limited to the platform the benchmark is executed on
- Pixi can install packages from conda and/or pypi. here, packages are installed from pypi

Still in draft as generating lock-files currently fails due to https://github.com/prefix-dev/pixi/issues/817


---

_Renamed from "Add benchmark of Pixi with uv" to "Add benchmark of pixi with uv" by @olivier-lacroix on 2024-02-17 10:19_

---

_Label `documentation` added by @zanieb on 2024-02-17 16:52_

---

_Label `performance` added by @zanieb on 2024-02-17 16:52_

---

_Comment by @zanieb on 2024-02-21 17:19_

This seems redundant now that they're retiring rip in favor of uv.

---

_Closed by @zanieb on 2024-02-21 17:21_

---

_Comment by @erlend-sh on 2024-02-21 18:15_

> This seems redundant now that they're retiring rip in favor of uv.

For anyone else like me wondering what you’re referring to: https://prefix.dev/blog/uv_in_pixi

Very ecosystem-conscious move by the Prefix team! 👏 

---
