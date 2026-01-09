---
number: 18332
title: Warn on multiple config files
type: issue
state: open
author: timhoffm
labels:
  - wish
assignees: []
created_at: 2025-05-27T14:29:07Z
updated_at: 2025-05-27T14:32:24Z
url: https://github.com/astral-sh/ruff/issues/18332
synced_at: 2026-01-07T13:12:16-06:00
---

# Warn on multiple config files

---

_Issue opened by @timhoffm on 2025-05-27 14:29_

Ruff takes the first config file from `.ruff.toml`, `ruff.toml`, `pyplroject.toml`, see https://github.com/astral-sh/ruff/blob/8d5655a7baa1ecc84a906cbfc0e2294cf173dee7/crates/ruff_workspace/src/pyproject.rs#L67

I recently got tripped up because a project had specified both `ruff.toml` and ruff settings in `pyproject.toml`. It would be good if ruff could warn that it found multiple config files. Something like

> WARNING: Multiple config files found: ruff.toml, pyproject.toml. Only the first config is used.

---

_Label `wish` added by @MichaReiser on 2025-05-27 14:32_

---
