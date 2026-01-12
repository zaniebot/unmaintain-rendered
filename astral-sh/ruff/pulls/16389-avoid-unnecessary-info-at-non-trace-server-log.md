```yaml
number: 16389
title: Avoid unnecessary info at non-trace server log level
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/server-logging
created_at: 2025-02-26T06:06:11Z
updated_at: 2025-02-26T08:01:19Z
url: https://github.com/astral-sh/ruff/pull/16389
synced_at: 2026-01-12T15:55:54Z
```

# Avoid unnecessary info at non-trace server log level

---

_@dhruvmanila_

## Summary

Currently, the log messages emitted by the server includes multiple information which isn't really required most of the time.

Here's the current format:
```
   0.000755625s DEBUG main ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/playground/ruff
   0.016334666s DEBUG ThreadId(10) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/playground/ruff/.vscode
   0.019954541s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/playground/ruff
   0.020160416s TRACE ruff:main notification{method="textDocument/didOpen"}: ruff_server::server::api: enter
   0.020209625s TRACE ruff:worker:0 request{id=1 method="textDocument/diagnostic"}: ruff_server::server::api: enter
   0.020228166s DEBUG ruff:worker:0 request{id=1 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/test.py
   0.020359833s  INFO     ruff:main ruff_server::server: Configuration file watcher successfully registered
```

This PR updates the following:
* Uses current timestamp (same as red-knot) for all log levels instead of the uptime value
* Includes the target and thread names only at the trace level

What this means is that the message is reduced to only important information at DEBUG level:

```
2025-02-26 11:35:02.198375000 DEBUG Indexing settings for workspace: /Users/dhruv/playground/ruff
2025-02-26 11:35:02.209933000 DEBUG Ignored path via `exclude`: /Users/dhruv/playground/ruff/.vscode
2025-02-26 11:35:02.217165000  INFO Registering workspace: /Users/dhruv/playground/ruff
2025-02-26 11:35:02.217631000 DEBUG Included path via `include`: /Users/dhruv/playground/ruff/lsp/test.py
2025-02-26 11:35:02.217684000  INFO Configuration file watcher successfully registered
```

while still showing the other information (thread names and target) at trace level:
```
2025-02-26 11:35:27.819617000 DEBUG main ruff_server::session::index::ruff_settings: Indexing settings for workspace: /Users/dhruv/playground/ruff
2025-02-26 11:35:27.830500000 DEBUG ThreadId(11) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/playground/ruff/.vscode
2025-02-26 11:35:27.837212000  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/playground/ruff
2025-02-26 11:35:27.837714000 TRACE ruff:main notification{method="textDocument/didOpen"}: ruff_server::server::api: enter
2025-02-26 11:35:27.838019000  INFO ruff:main ruff_server::server: Configuration file watcher successfully registered
2025-02-26 11:35:27.838084000 TRACE ruff:worker:1 request{id=1 method="textDocument/diagnostic"}: ruff_server::server::api: enter
2025-02-26 11:35:27.838205000 DEBUG ruff:worker:1 request{id=1 method="textDocument/diagnostic"}: ruff_server::resolve: Included path via `include`: /Users/dhruv/playground/ruff/lsp/test.py
```


---

_Label `server` added by @dhruvmanila on 2025-02-26 06:06_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-26 06:06_

---

_Comment by @github-actions[bot] on 2025-02-26 06:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2025-02-26 07:17_

Nice

Nit: It took me a moment to understand what changed compared to today. A short description explaining the changes in addition to how the new output looks would have been helpful

---

_Merged by @dhruvmanila on 2025-02-26 08:01_

---

_Closed by @dhruvmanila on 2025-02-26 08:01_

---

_Branch deleted on 2025-02-26 08:01_

---
