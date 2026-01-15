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
updated_at: 2026-01-15T12:49:48Z
url: https://github.com/astral-sh/ruff/pull/22582
synced_at: 2026-01-15T13:52:05Z
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

_Comment by @astral-sh-bot[bot] on 2026-01-14 19:46_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@MichaReiser reviewed on 2026-01-15 12:49_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_python.rs`:53 on 2026-01-15 12:49_

I'd prefer a stricter regex as mentioned in the original issue. We should also make the same change for `uvx` and `uv tool run`.

---
