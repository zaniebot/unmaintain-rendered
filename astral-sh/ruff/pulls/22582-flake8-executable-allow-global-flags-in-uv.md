```yaml
number: 22582
title: "flake8-executable: allow global flags in uv shebangs (EXE003)"
type: pull_request
state: open
author: Jkhall81
labels: []
assignees: []
base: main
head: fix/exe003-uv-global-args-21753
created_at: 2026-01-14T19:36:02Z
updated_at: 2026-01-14T19:36:02Z
url: https://github.com/astral-sh/ruff/pull/22582
synced_at: 2026-01-14T19:42:40Z
```

# flake8-executable: allow global flags in uv shebangs (EXE003)

---

_@Jkhall81_

## Summary

This PR allows the use of global flags (e.g., `--quiet`, `--offline`) in `uv` shebangs for rule `EXE003`. Previously, flags placed between `uv` and `run` caused a false positive "missing python" error. The logic has been updated to independently check for the presence of `uv` and `run` within the shebang directive.

Fixes #21753

## Test Plan

- Added new test cases to `crates/ruff_linter/resources/test/fixtures/flake8_executable/EXE003_uv.py` covering `uv --quiet run` and `uv --offline run`.

---
