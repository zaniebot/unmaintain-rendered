```yaml
number: 1088
title: Re-evaluate AST garbage collection
type: issue
state: open
author: MichaReiser
labels:
  - performance
  - server
assignees: []
created_at: 2025-08-22T09:37:12Z
updated_at: 2025-08-22T09:37:12Z
url: https://github.com/astral-sh/ty/issues/1088
synced_at: 2026-01-10T02:06:24Z
```

# Re-evaluate AST garbage collection

---

_Issue opened by @MichaReiser on 2025-08-22 09:37_

We collect AST nodes of first-party files when checking the entire project to reduce peak memory usage. 

This is great for the CLI, but may negatively impact the LSP performance, because ty has to reparse all first-party files when computing the workspace symbols, auto import completions, or when re-checking a project. 

For the LSP use case, a better approach is probably to enable LRU on parsed ASTs, because it ensures that we never throw away parsed modules within a revision and retain frequently used ASTs in memory (but collect ASTs that are rarely used or only used once and never again). 

The downside of only using LRU is that it results in higher peak memory usage, but that's only the case when all LSP features other than diagnostics is disabled because the workspace symbols feature doesn't drop the ASTs after indexing.

CC: @ibraheemdev, CC: @BurntSushi 

---

_Label `performance` added by @MichaReiser on 2025-08-22 09:37_

---

_Label `server` added by @MichaReiser on 2025-08-22 09:37_

---
