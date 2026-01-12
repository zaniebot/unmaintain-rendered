```yaml
number: 8901
title: "uv-pip-install: force-rebuild scikit-build-core based projects on changes to the source files?"
type: issue
state: closed
author: Thibaulltt
labels: []
assignees: []
created_at: 2024-11-07T22:41:07Z
updated_at: 2024-11-07T23:06:06Z
url: https://github.com/astral-sh/uv/issues/8901
synced_at: 2026-01-12T15:59:37Z
```

# uv-pip-install: force-rebuild scikit-build-core based projects on changes to the source files?

---

_@Thibaulltt_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hi all, I'm using `scikit-build-core` to build a set of C++ CMake targets that then get installed in a python module local to the folder, basically like this:

```txt
project-root
- package-namespace
  | - cpp
  |   | - include/
  |   | - src/
  |   | - tests/
  |   | - CMakeLists.txt
  | - package-py
  |   | - __init__.py
  | - pyproject.toml
- ...other files...
```

And I run `uv pip install --editable .` from `package-namespace` in order to install it in a conda environment. But the changes on C++ files do _not_ get picked up when performing a `uv pip install`, even with the `--refresh` flag. Can I specify in `pyproject.toml` what files should be watched in order to invalidate uv's cache ?

---

_Comment by @zanieb on 2024-11-07 22:53_

Did you try setting a `cache-key`? https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata

---

_Comment by @Thibaulltt on 2024-11-07 23:06_

Ah, missed that in the documentation. Thanks! Works like a charm.

---

_Closed by @Thibaulltt on 2024-11-07 23:06_

---
