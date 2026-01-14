```yaml
number: 22567
title: "[ruff server] Open new workspace when opening a file outside of current workspace"
type: pull_request
state: open
author: ZedThree
labels:
  - server
assignees: []
base: main
head: fix-17944
created_at: 2026-01-14T10:01:25Z
updated_at: 2026-01-14T10:28:26Z
url: https://github.com/astral-sh/ruff/pull/22567
synced_at: 2026-01-14T10:34:28Z
```

# [ruff server] Open new workspace when opening a file outside of current workspace

---

_@ZedThree_

Fixes #17944

## Summary

Here we check if we have settings for the file we're trying to open, as a proxy for checking if it's in an open workspace or not.

Given this filesystem layout:

```
.
├── dir_0
│   ├── ruff.toml
│   └── test_0.py
└── dir_1
    ├── ruff.toml
    └── test_1.py
```

This fixes:

- cwd at top-level, opening `dir_0/test_0.py`
- cwd at top-level, opening `dir_0/test_0.py`, then `../dir_1/test_1.py` in same session
- cwd in `dir_0`, opening `../dir_1/test_1.py`
- cwd in `dir_0`, opening `test_0.py` first, then `../dir_1/test_1.py` in same session

without negatively affecting non-default workspaces (such as opening the folder in VS Code or `lsp-mode` in Emacs).

---

This doesn't work for the situation where we're opening a file in a nested directory below the default workspace, where we _do_ have a config file:

```
.
├── subdir
│   ├── ruff.toml
│   └── test_1.py
├── ruff.toml
└── test_0.py
```

Opening `test_0.py` first (in single file mode), and then opening `subdir/test_1.py` still uses the settings from `./ruff.toml` instead of `subdir/ruff.toml`

## Test Plan

Tested using the files described in [this comment](https://github.com/astral-sh/ruff/issues/17944#issuecomment-3745670262)

I noticed that ty has e2e testing of its lsp server, but this hasn't been implemented for ruff yet. I think this would need something like that to test automatically


---

_Comment by @astral-sh-bot[bot] on 2026-01-14 10:09_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@MichaReiser requested changes on 2026-01-14 10:09_

As mentioned [here](https://github.com/astral-sh/ruff/issues/17944#issuecomment-3748622248), I don't think this is the right approach. Adding a workspace folder corresponds to VS Code's "Add folder" functionality and has a lot of implications. We shouldn't just "add" arbitrary folders when a user opens a file, especially not without cleaning them up.

---

_Label `server` added by @AlexWaygood on 2026-01-14 10:16_

---
