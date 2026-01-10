```yaml
number: 16215
title: Include document specific debug info
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/debug-info-command
created_at: 2025-02-17T15:42:56Z
updated_at: 2025-02-18T10:08:56Z
url: https://github.com/astral-sh/ruff/pull/16215
synced_at: 2026-01-10T19:57:22Z
```

# Include document specific debug info

---

_Pull request opened by @dhruvmanila on 2025-02-17 15:42_

## Summary

Related https://github.com/astral-sh/ruff-vscode/pull/692.

## Test Plan

**When there's no active text document:**

```
[Info  - 10:57:03 PM] Global:
executable = /Users/dhruv/work/astral/ruff/target/debug/ruff
version = 0.9.6
position_encoding = UTF16
workspace_root_folders = [
    "/Users/dhruv/playground/ruff",
]
indexed_configuration_files = [
    "/Users/dhruv/playground/ruff/pyproject.toml",
    "/Users/dhruv/playground/ruff/formatter/ruff.toml",
]
open_documents = 0
client_capabilities = ResolvedClientCapabilities {
    code_action_deferred_edit_resolution: true,
    apply_edit: true,
    document_changes: true,
    workspace_refresh: true,
    pull_diagnostics: true,
}

global_client_settings = ResolvedClientSettings {
    fix_all: true,
    organize_imports: true,
    lint_enable: true,
    disable_rule_comment_enable: true,
    fix_violation_enable: true,
    show_syntax_errors: true,
    editor_settings: ResolvedEditorSettings {
        configuration: None,
        lint_preview: None,
        format_preview: None,
        select: None,
        extend_select: None,
        ignore: None,
        exclude: None,
        line_length: None,
        configuration_preference: EditorFirst,
    },
}
```

**When there's an active text document that's been passed as param:**

```
[Info  - 10:53:33 PM] Global:
executable = /Users/dhruv/work/astral/ruff/target/debug/ruff
version = 0.9.6
position_encoding = UTF16
workspace_root_folders = [
    "/Users/dhruv/playground/ruff",
]
indexed_configuration_files = [
    "/Users/dhruv/playground/ruff/pyproject.toml",
    "/Users/dhruv/playground/ruff/formatter/ruff.toml",
]
open_documents = 1
client_capabilities = ResolvedClientCapabilities {
    code_action_deferred_edit_resolution: true,
    apply_edit: true,
    document_changes: true,
    workspace_refresh: true,
    pull_diagnostics: true,
}

Document:
uri = file:///Users/dhruv/playground/ruff/lsp/play.py
kind = Text
version = 1
client_settings = ResolvedClientSettings {
    fix_all: true,
    organize_imports: true,
    lint_enable: true,
    disable_rule_comment_enable: true,
    fix_violation_enable: true,
    show_syntax_errors: true,
    editor_settings: ResolvedEditorSettings {
        configuration: None,
        lint_preview: None,
        format_preview: None,
        select: None,
        extend_select: None,
        ignore: None,
        exclude: None,
        line_length: None,
        configuration_preference: EditorFirst,
    },
}
config_path = Some("/Users/dhruv/playground/ruff/pyproject.toml")

...
```

Replace `...` at the end with the output of `ruff check --show-settings path.py`

---

_Label `server` added by @dhruvmanila on 2025-02-17 15:42_

---

_Comment by @github-actions[bot] on 2025-02-17 17:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2025-02-18 04:42_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:48 on 2025-02-18 04:42_

Currently, we only look at the first argument but a potential new VS Code command could be to look at all opened documents which would require passing multiple arguments. But, I'm going to leave this for now.

---

_Marked ready for review by @dhruvmanila on 2025-02-18 04:42_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-18 04:42_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/execute_command.rs`:25 on 2025-02-18 07:46_

Could we document the `uri` parameter? The name doesn't give any information (nor the struct name) what the uri points to

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/execute_command.rs`:182 on 2025-02-18 07:46_

```suggestion
            "Active Document:
```

Or `Open Document`

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/execute_command.rs`:199 on 2025-02-18 07:47_

Nit: You may want to store `query` in a temporary variable to shorten some of those calls.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:171 on 2025-02-18 07:48_

Nit: I think I prefer `open_documents_len` to make it explicit that this doesn't return the open documents itself.

---

_@MichaReiser approved on 2025-02-18 07:48_

This is great and i also forgot about it. At least, it never came to my mind when asking users for debugging information which is a shame.

---

_@dhruvmanila reviewed on 2025-02-18 08:48_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:25 on 2025-02-18 08:48_

I'll replace `DebugCommandArgument` with [`TextDocumentIdentifier`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocumentIdentifier) because that's what we want here.

Edit: wait, I'm thinking a bit about this

---

_@dhruvmanila reviewed on 2025-02-18 09:01_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:25 on 2025-02-18 09:01_

Looking at how `rust-analyzer` accepts parameters, it seems useful to have a consistent structure similar to how requests would accept params, so this could be updated to:
```
{ textDocument?: TextDocumentIdentifier }
```
For example:
```
{
    textDocument: {
        uri: "file:///Users/dhruv/dotfiles/config/nvim/lua/dm/lsp/extensions/rust_analyzer.lua"
    }
}
```

---

_@dhruvmanila reviewed on 2025-02-18 09:04_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:182 on 2025-02-18 09:04_

It could possibly be any document, it depends on what the client sends us. In case of our VS Code extension, we send the active document but, for example, I could send any other document that I would want the information for in Neovim :)

---

_Merged by @dhruvmanila on 2025-02-18 10:08_

---

_Closed by @dhruvmanila on 2025-02-18 10:08_

---

_Branch deleted on 2025-02-18 10:08_

---
