```yaml
number: 1397
title: Go to Definition Missing correct path
type: issue
state: open
author: YoniChechik
labels:
  - server
assignees: []
created_at: 2025-10-19T09:55:45Z
updated_at: 2025-11-18T16:10:40Z
url: https://github.com/astral-sh/ty/issues/1397
synced_at: 2026-01-12T15:54:25Z
```

# Go to Definition Missing correct path

---

_@YoniChechik_

## Summary
When using "Go to Definition" (Ctrl+Click) on a function in a Python project with git worktrees, ty navigates to the definition in a different worktree instead of the current worktree.


## Repository Structure

We have a Python project using git worktrees. The issue occurs in both configurations:

### Configuration 1: Worktrees in Separate Directory
```
/workspace/
├── my-project/                            (main repo, main branch)
│   ├── .git/                              (main .git directory)
│   │   └── worktrees/                     (git worktree metadata)
│   │       ├── feature-branch-1/
│   │       └── feature-branch-2/
│   ├── mypackage/                         (Python package)
│   │   ├── __init__.py
│   │   ├── module.py
│   │   └── utils.py
│   └── tests/
│
└── worktrees/                             (worktrees directory)
    ├── feature-branch-1/                  (worktree, feature branch)
    │   ├── .git                           (file pointing to main repo)
    │   ├── mypackage/
    │   │   ├── __init__.py
    │   │   ├── module.py                  (modified in this branch)
    │   │   └── utils.py
    │   └── tests/
    │
    └── feature-branch-2/                  (worktree, another feature branch)
        ├── .git                           (file pointing to main repo)
        ├── mypackage/
        │   ├── __init__.py
        │   ├── module.py                  (modified in this branch)
        │   └── utils.py
        └── tests/
```

### Configuration 2: Worktrees Inside Main Repo (also affected)
```
/workspace/my-project/
├── .git/                                  (main .git directory)
├── mypackage/                             (main branch code)
│   ├── __init__.py
│   ├── module.py
│   └── utils.py
├── tests/
└── worktrees/                             (worktrees inside main repo)
    ├── feature-branch-1/
    │   ├── .git                           (file pointing to main repo)
    │   ├── mypackage/
    │   │   ├── module.py                  (modified)
    │   │   └── utils.py
    │   └── tests/
    └── feature-branch-2/
        └── ...
```

## Steps to Reproduce

1. Open VS Code in a git worktree directory (e.g., `/workspace/worktrees/feature-branch-1/`)
2. Open a Python file that imports and uses a function from your package (e.g., `from mypackage.module import some_function`)
3. Ctrl+Click (or F12) on the function name to "Go to Definition"

## Expected Behavior
ty should navigate to the function definition in the **current worktree** at:
```
/workspace/worktrees/feature-branch-1/mypackage/module.py
```

## Actual Behavior
ty sometimes navigates to the function definition in a **different worktree** or the main repo at:
```
/workspace/worktrees/feature-branch-2/mypackage/module.py
```
or
```
/workspace/my-project/mypackage/module.py
```

## Impact
This is problematic because:
1. Different worktrees contain different branch implementations of the same functions
2. Developers end up viewing/editing the wrong version of code
3. It breaks the expected isolation between branches when using worktrees

## Additional Context
- All worktrees share the same `.git` directory structure (normal git worktree behavior)
- The issue occurs regardless of whether worktrees are in a separate directory or nested within the main repo
- Python environment is properly configured with the package installed in editable mode

## Suggested Fix
ty should respect the currently open workspace folder and prioritize symbol resolution within that workspace, treating each worktree as an isolated environment.

this also happens in pylance but I was hoping it will work properly in ty: https://github.com/microsoft/pylance-release/issues/7642

example repo for reproduction and actual visual bug can be seen here: https://github.com/microsoft/pylance-release/issues/7642#issuecomment-3412364298

### Version

ty vscode version 2025.47.12891834

---

_Label `server` added by @AlexWaygood on 2025-10-19 09:59_

---

_Renamed from "Go to Definition Mising correct path" to "Go to Definition Missing correct path" by @AlexWaygood on 2025-10-19 09:59_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:31_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
