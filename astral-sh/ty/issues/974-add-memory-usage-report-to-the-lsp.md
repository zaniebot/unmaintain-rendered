```yaml
number: 974
title: Add memory usage report to the LSP
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - server
assignees: []
created_at: 2025-08-12T13:47:50Z
updated_at: 2025-09-22T11:28:10Z
url: https://github.com/astral-sh/ty/issues/974
synced_at: 2026-01-10T02:06:24Z
```

# Add memory usage report to the LSP

---

_Issue opened by @MichaReiser on 2025-08-12 13:47_

ty's CLI emits a detailed memory usage report when run with `TY_MEMORY_REPORT=full`. It would be great to have a similar command for the LSP that prints debug information, which also includes the memory report.

Ruff's LSP has something similar: https://github.com/astral-sh/ruff/blob/015222900f16ae3100b3415e1b6be44aed867e4b/crates/ruff_server/src/server/api/requests/execute_command.rs#L46-L57

This would be useful when debugging how adding an LRU cache improves memory usage over time

---

_Label `help wanted` added by @MichaReiser on 2025-08-12 13:47_

---

_Label `server` added by @MichaReiser on 2025-08-12 13:47_

---

_Comment by @dhruvmanila on 2025-08-13 04:53_

Relevant VS Code extension issue: https://github.com/astral-sh/ty-vscode/issues/6

And, the implementation in Ruff VS Code extension: https://github.com/astral-sh/ruff-vscode/blob/3d052e6b11421e510264c01cf0e3a43b33595560/src/extension.ts#L200-L203

---

_Comment by @dhruvmanila on 2025-08-13 04:57_

Maybe we should make this a custom request instead of a command. Rust-analyzer uses a separate request endpoint e.g., https://rust-analyzer.github.io/book/contributing/lsp-extensions.html#analyzer-status, https://github.com/rust-lang/rust-analyzer/blob/master/docs/book/src/contributing/lsp-extensions.md#view-syntax-tree, etc. although I'm not sure which one is more beneficial.

---

_Comment by @MichaReiser on 2025-09-20 11:05_

https://github.com/astral-sh/ruff/pull/20379 added the custom command. What's now left to do is to expose the command in VS code. 

@Guillaume-Fgt: Are you also interested in contributing the necessary VS code changes:

1. Adding the command in the `package.json` ([source](https://github.com/astral-sh/ruff-vscode/blob/67f5c516ae2175a32a468aaaa05a614344230ad0/package.json#L447-L451))
2. Add a handler that automatically opens a window in VS code ([source](https://github.com/astral-sh/ruff-vscode/blob/67f5c516ae2175a32a468aaaa05a614344230ad0/src/common/commands.ts#L1-L124))

---

_Comment by @Guillaume-Fgt on 2025-09-20 11:41_

As I said in the PR, yes I am. I will work on it 👍.

---

_Assigned to @Guillaume-Fgt by @MichaReiser on 2025-09-20 11:46_

---

_Comment by @MichaReiser on 2025-09-20 11:47_

Thank you. The code for the VS code extension is here https://github.com/astral-sh/ty-vscode

---

_Closed by @MichaReiser on 2025-09-22 11:28_

---
