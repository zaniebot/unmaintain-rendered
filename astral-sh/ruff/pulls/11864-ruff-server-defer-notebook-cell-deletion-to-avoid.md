```yaml
number: 11864
title: "`ruff server`: Defer notebook cell deletion to avoid an error message"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/defer-cell-index-deletion
created_at: 2024-06-13T19:26:11Z
updated_at: 2024-06-18T03:47:49Z
url: https://github.com/astral-sh/ruff/pull/11864
synced_at: 2026-01-12T15:55:39Z
```

# `ruff server`: Defer notebook cell deletion to avoid an error message

---

_@snowsignal_

## Summary

Fixes https://github.com/astral-sh/ruff-vscode/issues/496.

Cells are no longer removed from the notebook index when a notebook gets updated, but rather when `textDocument/didClose` is called for them. This solves an issue where their premature removal from the notebook cell index would cause their URL to be un-queryable in the `textDocument/didClose` handler.

## Test Plan

Create and then delete a notebook cell in VS Code. No error should appear.



---

_Label `server` added by @snowsignal on 2024-06-13 19:26_

---

_Comment by @github-actions[bot] on 2024-06-13 19:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:162 on 2024-06-13 20:26_

Does that mean that we never retain cells until didClose is called? Does the specification explain why vs code might reference cells after they have been closed ?

---

_@MichaReiser reviewed on 2024-06-13 20:27_

---

_Comment by @MichaReiser on 2024-06-13 20:28_

Any chance that this solves https://github.com/astral-sh/ruff/issues/11851

---

_Comment by @T-256 on 2024-06-14 01:56_

> Any chance that this solves #11851

Created https://github.com/astral-sh/ruff/pull/11867 for that. According to [this message's log](https://github.com/astral-sh/ruff/issues/11851#issuecomment-2166221751) it seems he was got multiple error on closing a notebook document with multiple cells in it.

---

_@dhruvmanila reviewed on 2024-06-14 02:34_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index.rs`:169 on 2024-06-14 02:34_

I'm not sure if we should remove this logic. Does VS Code send two requests (`notebookDocument/didChange` and `textDocument/didClose`) when a cell is deleted? Is there a case where it might only send either one of them? Can you check which request is sent by VS Code between `notebookDocument/didChange` and `textDocument/didClose` assuming that's what's causing this bug?

Looking at the `pygls` implementation, it retains both logic except that it doesn't warn the user when removing a cell if it doesn't exists: https://github.com/openlawlibrary/pygls/blob/da9b77b7446ffc3c8a503670408dfc76e4a777b0/pygls/workspace/workspace.py#L272-L273

---

_@dhruvmanila reviewed on 2024-06-14 02:45_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index.rs`:169 on 2024-06-14 02:45_

Ok, I don't see any additional requests sent by VS Code that references the closed cell between `notebookDocument/didChange` and `textDocument/didClose`:
```
2024-06-14 08:11:46.288 [info] [Trace - 8:11:46 AM] Sending notification 'notebookDocument/didChange'.
2024-06-14 08:11:46.289 [info] Params: {
    "notebookDocument": {
        "version": 7,
        "uri": "untitled:Untitled-1.ipynb?jupyter-notebook"
    },
    "change": {
        "cells": {
            "structure": {
                "array": {
                    "start": 1,
                    "deleteCount": 1
                },
                "didOpen": [],
                "didClose": [
                    {
                        "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W2sdW50aXRsZWQ%3D"
                    }
                ]
            }
        }
    }
}


2024-06-14 08:11:46.290 [info]   96.769647s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-1.ipynb?jupyter-notebook'.
  96.769686s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-1.ipynb?jupyter-notebook'.

2024-06-14 08:11:46.290 [info] [Trace - 8:11:46 AM] Received notification 'textDocument/publishDiagnostics'.
2024-06-14 08:11:46.291 [info] Params: {
    "diagnostics": [],
    "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D",
    "version": 7
}


2024-06-14 08:11:46.303 [info] [Trace - 8:11:46 AM] Sending notification 'textDocument/didClose'.
2024-06-14 08:11:46.303 [info] Params: {
    "textDocument": {
        "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W2sdW50aXRsZWQ%3D"
    }
}
```

So, I guess it's just that the server is trying to take the snapshot in `textDocument/didClose` handler but the `notebookDocument/didChange` already removed this cell.

---

_@MichaReiser reviewed on 2024-06-14 07:56_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:169 on 2024-06-14 07:56_

Do I understand it correctly that this means that VS code always sends an explicit `didClose` notification for all cells that were also closed in the `didChange` notification?

---

_@dhruvmanila reviewed on 2024-06-14 09:15_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index.rs`:169 on 2024-06-14 09:15_

Yes, it seems like so. I think it's because cells are modeled as a text document which means that if a cell is removed it triggers both `notebookDocument/didChange` and `textDocument/didClose` request.

---

_@MichaReiser reviewed on 2024-06-14 14:06_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:350 on 2024-06-14 14:06_

Could we keep erroring in this case or what's the motivation for changing this from an error to a warning.

---

_Comment by @MichaReiser on 2024-06-14 15:00_

Reading through the comments, I think the change makes sense. However, I would find it helpful if we can expand the comment explaining how cells are closed now with an explanation on how VS code handles it. I think that would also be helpful for someone coming back to this PR to understand why and what has changed.

---

_@snowsignal reviewed on 2024-06-14 22:00_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/index.rs`:162 on 2024-06-14 22:00_

VS Code uses `didClose` to close a notebook cell. Unfortunately I can't find anything in the specification about this.

---

_@snowsignal reviewed on 2024-06-14 22:01_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/index.rs`:350 on 2024-06-14 22:01_

Technically this is just moving over the code that removes the cell from the index 😄 

The error happened because we try to take a snapshot and that fails. This is a less critical error because if we fail to remove a notebook cell from the index, that means it's already been removed for one reason or another.

---

_@snowsignal reviewed on 2024-06-14 22:03_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/index.rs`:169 on 2024-06-14 22:03_

@dhruvmanila 
> Can you check which request is sent by VS Code between notebookDocument/didChange and textDocument/didClose assuming that's what's causing this bug?

As you saw, there aren't any requests being sent in between. Rather, the `didClose` handler tries to take a document snapshot before removing the document. This is so that it can publish an empty diagnostics list for the document/cell before closing it. That's what was causing the problem.

---

_@MichaReiser approved on 2024-06-15 08:09_

---

_Merged by @snowsignal on 2024-06-18 03:37_

---

_Closed by @snowsignal on 2024-06-18 03:37_

---

_Branch deleted on 2024-06-18 03:37_

---

_Comment by @codspeed-hq[bot] on 2024-06-18 03:38_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/server/defer-cell-index-deletion)

### Merging #11864 will **degrade performances by 9.48%**

<sub>Comparing <code>jane/server/defer-cell-index-deletion</code> (9c4abdb) with <code>jane/server/defer-cell-index-deletion</code> (dd865d0)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 28` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/jane/server/defer-cell-index-deletion)._

### Benchmarks breakdown

|     | Benchmark | `jane/server/defer-cell-index-deletion` | `jane/server/defer-cell-index-deletion` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 701.9 µs | 775.5 µs | -9.48% |
| ⚡ | `parser[pydantic/types.py]` | 2.2 ms | 2.1 ms | +4.26% |


---
