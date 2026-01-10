```yaml
number: 11224
title: "`ruff server` respects `per-file-ignores` configuration"
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: jane/server/lint-with-path
created_at: 2024-05-01T00:26:06Z
updated_at: 2024-05-02T02:24:36Z
url: https://github.com/astral-sh/ruff/pull/11224
synced_at: 2026-01-10T22:37:02Z
```

# `ruff server` respects `per-file-ignores` configuration

---

_Pull request opened by @snowsignal on 2024-05-01 00:26_

## Summary

Fixes #11185
Fixes #11214 

Document path and package information is now forwarded to the Ruff linter, which allows `per-file-ignores` to correctly match against the file name. This also fixes an issue where the import sorting rule didn't distinguish between third-party and first-party packages since we didn't pass in the package root.

## Test Plan

`per-file-ignores` should ignore files as expected. One quick way to check is by adding this to your `pyproject.toml`:
```toml
[tool.ruff.lint.per-file-ignores]
"__init__.py" = ["ALL"]
```

Then, confirm that no diagnostics appear when you add code to an `__init__.py` file (besides syntax errors).

The import sorting fix can be verified by failing to reproduce the original issue - an `I001` diagnostic should not appear in `other_module.py`.

---

_Label `bug` added by @snowsignal on 2024-05-01 00:26_

---

_Label `server` added by @snowsignal on 2024-05-01 00:26_

---

_Comment by @github-actions[bot] on 2024-05-01 00:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-05-01 04:52_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/fix.rs`:26 on 2024-05-01 07:02_

Can you try what happens if you run `fix_all` on a new, unnamed and unsaved file. Does the new file already have a valid path? 

---

_@MichaReiser approved on 2024-05-01 07:03_

---

_@snowsignal reviewed on 2024-05-02 02:24_

---

_Review comment by @snowsignal on `crates/ruff_server/src/fix.rs`:26 on 2024-05-02 02:24_

It does, although the workspace edit seems to fail in certain cases, for some reason? I'll investigate the issue in a follow-up, since it seems to be unrelated to this change.

---

_Merged by @snowsignal on 2024-05-02 02:24_

---

_Closed by @snowsignal on 2024-05-02 02:24_

---

_Branch deleted on 2024-05-02 02:24_

---
