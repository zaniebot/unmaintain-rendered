```yaml
number: 22570
title: "[ruff server] Find config for files outside of open workspaces"
type: pull_request
state: open
author: ZedThree
labels:
  - server
assignees: []
base: main
head: fix-17944-alt
created_at: 2026-01-14T12:08:40Z
updated_at: 2026-01-14T12:20:59Z
url: https://github.com/astral-sh/ruff/pull/22570
synced_at: 2026-01-14T12:43:59Z
```

# [ruff server] Find config for files outside of open workspaces

---

_@ZedThree_

Fixes #17944

There's two different routes we can go down for finding settings for such files:
- the file is below an open workspace, so we can add any settings we find to that workspace
- the file is not below an open workspace, in which case we can't cache them

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

The main downside to this approach is the lack of workspace for files in `dir_1` -- we can't share already parsed settings.

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

_Label `server` added by @AlexWaygood on 2026-01-14 12:09_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 12:20_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---
