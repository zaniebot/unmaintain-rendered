```yaml
number: 13510
title: "Make `uv python install` consider `pyproject.toml`'s `requires-python` field"
type: pull_request
state: open
author: guoci
labels: []
assignees: []
base: main
head: python_install_with_project
created_at: 2025-05-17T19:36:13Z
updated_at: 2025-05-19T07:23:19Z
url: https://github.com/astral-sh/uv/pull/13510
synced_at: 2026-01-12T16:10:42Z
```

# Make `uv python install` consider `pyproject.toml`'s `requires-python` field

---

_@guoci_



<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Make `uv python install` read `requires-python` field in  `pyproject.toml` before installing a default Python. Currently, only the command line arguments and the files `.python-version` and `.python-versions` are used.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Tests included in pull request.
<!-- How was it tested? -->

---

_Review requested from @zanieb by @konstin on 2025-05-19 07:23_

---

_Assigned to @zanieb by @konstin on 2025-05-19 07:23_

---
