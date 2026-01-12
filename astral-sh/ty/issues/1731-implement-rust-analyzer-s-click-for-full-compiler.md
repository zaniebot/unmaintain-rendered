```yaml
number: 1731
title: "Implement rust-analyzer's \"Click for full compiler diagnostic\" feature"
type: issue
state: open
author: AlexWaygood
labels:
  - server
assignees: []
created_at: 2025-12-02T20:12:11Z
updated_at: 2025-12-03T05:07:02Z
url: https://github.com/astral-sh/ty/issues/1731
synced_at: 2026-01-12T15:54:25Z
```

# Implement rust-analyzer's "Click for full compiler diagnostic" feature

---

_@AlexWaygood_

Rust-analyzer has a great feature in VSCode that I use _extensively_ when writing Rust, where you can click to see the full compiler diagnostic in a new tab:

https://github.com/user-attachments/assets/7c14c7d6-46ea-4716-9f05-4df032cdc913

Would it be possible for ty to implement something similar?

---

_Label `server` added by @AlexWaygood on 2025-12-02 20:12_

---

_Comment by @MichaReiser on 2025-12-02 20:19_

This would be VS Code specific

---

_Comment by @dhruvmanila on 2025-12-03 05:07_

RA passes this information via the `data` field: https://github.com/rust-lang/rust-analyzer/blob/master/docs/book/src/contributing/lsp-extensions.md#colored-diagnostic-output and here's the PR that implemented it: https://github.com/rust-lang/rust-analyzer/pull/13633, https://github.com/rust-lang/rust-analyzer/pull/13848.

---
