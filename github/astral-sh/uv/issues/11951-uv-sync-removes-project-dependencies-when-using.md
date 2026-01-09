---
number: 11951
title: "`uv sync` removes `[project.dependencies]` when using scikit-build-core as build backend"
type: issue
state: closed
author: mann-patwa
labels:
  - question
assignees: []
created_at: 2025-03-04T15:11:15Z
updated_at: 2025-03-04T15:12:02Z
url: https://github.com/astral-sh/uv/issues/11951
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv sync` removes `[project.dependencies]` when using scikit-build-core as build backend

---

_Issue opened by @mann-patwa on 2025-03-04 15:11_

### Question

Description
When running `uv sync`, it installs the dependencies from the `[dependency-groups.dev]` section correctly but removes all dependencies listed in `[project.dependencies]`.

However, when I remove scikit-build-core as the build backend from `pyproject.toml` and then run `uv sync`, it correctly installs all dependencies, including `[project.dependencies]`.

This suggests that `uv sync` might not be handling `[project.dependencies]` properly when a build backend like scikit-build-core is specified.

Is there a scenario where:
1) `uv sync` be prioritizing --group dev and ignoring [project.dependencies] when a build backend is specified?
2) There is an issue with how scikit-build-core interacts with uv?



### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @mann-patwa on 2025-03-04 15:11_

---

_Closed by @mann-patwa on 2025-03-04 15:12_

---
