```yaml
number: 12880
title: "\"Fix all\" code action on notebook causes panic with multi-byte character"
type: issue
state: closed
author: sato1214syun
labels:
  - bug
  - server
assignees: []
created_at: 2024-08-14T06:33:10Z
updated_at: 2024-08-16T14:20:13Z
url: https://github.com/astral-sh/ruff/issues/12880
synced_at: 2026-01-12T15:54:52Z
```

# "Fix all" code action on notebook causes panic with multi-byte character

---

_@sato1214syun_

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  keywords: panic, Q000

* A minimal code snippet that reproduces the bug.
[test.zip](https://github.com/user-attachments/files/16608365/test.zip)

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
Implemented formatOnSave of VSCode for notebook

* The current Ruff settings (any relevant sections from your `pyproject.toml`).
[ruff.toml.zip](https://github.com/user-attachments/files/16608416/ruff.toml.zip)


* The current Ruff version (`ruff --version`).
  ruff 0.5.7
  ruff vscode extension v2024.40.0

Eror message
```
2024-08-14 15:23:07.676 [info] panicked at crates/ruff_server/src/edit/range.rs:196:39:
byte index 161 is not a char boundary; it is inside 'あ' (bytes 159..162) of `"""aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa."""
'あaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa`[...]

2024-08-14 15:23:07.678 [info]    0: std::backtrace::Backtrace::force_capture
   1: ruff_server::server::Server::
2024-08-14 15:23:07.678 [info] run
   2: std::panicking::rust_panic_with_hook
   3: <std::panicking::begin_panic_handler::StaticStrPayload as core::panic::PanicPayload>::take_box
   4: <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt
   5: _rust_begin_unwind
   6: core::panicking::panic_fmt
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
   9: <ruff_server::server::api::Error as core::fmt::Display>::fmt
  10: 
2024-08-14 15:23:07.678 [info] <ruff_text_size::range::TextRange as ruff_server::edit::range::ToRangeExt>::to_range
  11: ruff_server::edit::text_document::TextDocument::apply_changes
  12: <ruff_server::server::api::requests::code_action_resolve::CodeActionResolve as ruff_server::server::api::traits::BackgroundDocumentRequestHandler>::run_with_snapshot
  13: <ruff_server::server::api::requests::code_action_resolve::CodeActionResolve as ruff_server
2024-08-14 15:23:07.678 [info] ::server::api::traits::BackgroundDocumentRequestHandler>::run_with_snapshot
  14: <ruff_server::session::capabilities::ResolvedClientCapabilities as core::fmt::Display>::fmt
  15: <ruff_server::server::api::requests::diagnostic::DocumentDiagnostic as ruff_server::server::api::traits::BackgroundDocumentRequestHandler>::run_with_snapshot
  16: <ruff_server::server::api::requests::diagnostic::DocumentDiagnostic as ruff_server::server::api::traits::BackgroundDocumentRequestHandler>::run_with_snapshot
  17: <ruff_server::server::api
2024-08-14 15:23:07.679 [info] ::requests::diagnostic::DocumentDiagnostic as ruff_server::server::api::traits::BackgroundDocumentRequestHandler>::run_with_snapshot
  18: std::sys::pal::unix::thread::Thread::new
  19: __pthread_joiner_wake
```

---

_Comment by @dhruvmanila on 2024-08-14 07:08_

(I'll look into this in some time.)

---

_Label `bug` added by @MichaReiser on 2024-08-14 07:11_

---

_Label `server` added by @MichaReiser on 2024-08-14 07:11_

---

_Comment by @dhruvmanila on 2024-08-14 12:57_

Thank you for providing all the details. Although, I'm unable to reproduce this panic. Can you provide your VS Code settings? By "formatOnSave", do you mean `notebook.formatOnSave.enabled` setting?

---

_Comment by @sato1214syun on 2024-08-14 18:59_

Thank you @dhruvmanila for investigating. It happens when I press "command + s" or implement "Ruff: fix all auto-fixable problems" in command palette.

Environment:
M1 mac
MacOS 14.5
vscode version 1.92.1

My vscode profile for python is below.
```
{
  // python
  "python.analysis.autoFormatStrings": true,
  "python.analysis.gotoDefinitionInStringLiteral": true,
  "python.analysis.inlayHints.callArgumentNames": "partial",
  "python.analysis.inlayHints.functionReturnTypes": true,
  "python.analysis.inlayHints.pytestParameters": true,
  "python.analysis.inlayHints.variableTypes": true,
  "python.languageServer": "Pylance",
  // ruff
  "[python]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit",
      "source.organizeImports.": "explicit",
    },
  },
  "notebook.formatOnCellExecution": true,
  "notebook.formatOnSave.enabled": true,
  "notebook.codeActionsOnSave": {
    "notebook.source.fixAll": "explicit",
    "notebook.source.organizeImports": "explicit",
  },
  // jupyter
  "jupyter.askForKernelRestart": false,
  "jupyter.interactiveWindow.creationMode": "perFile",
  // other extension
  "markdown.extension.math.enabled": false,
  "git.suggestSmartCommit": false,
  "workbench.colorTheme": "Default Dark+",
  "github.copilot.editor.enableAutoCompletions": true,
  "editor.fontFamily": "\"Bizin Gothic Discord NF\", Menlo, Monaco, 'Courier New', monospace",
  "workbench.editorAssociations": {
    "{git,gitlens}:/**/*.{md,csv,svg}": "default"
  },
  "autoDocstring.docstringFormat": "numpy",
  "dataWrangler.enabledFileTypes": {
    "csv": false,
    "tsv": false
  },
}
```

---

_Comment by @dhruvmanila on 2024-08-16 04:01_

Thanks for providing the details. I can reproduce it using the "Fix all" code action.

---

_Renamed from "formatOnSave on notebook causes panic with code including multi-byte character, specific code length and Q000 " to ""Fix all" code action on notebook causes panic with code including multi-byte character" by @dhruvmanila on 2024-08-16 04:02_

---

_Renamed from ""Fix all" code action on notebook causes panic with code including multi-byte character" to ""Fix all" code action on notebook causes panic with multi-byte character" by @dhruvmanila on 2024-08-16 04:02_

---

_Comment by @dhruvmanila on 2024-08-16 04:03_

It doesn't occur when performing the action via the command-line (`ruff check --fix`) so it must be in this function (`<ruff_text_size::range::TextRange as ruff_server::edit::range::ToRangeExt>::to_range`)

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-08-16 13:44_

---

_Closed by @dhruvmanila on 2024-08-16 14:20_

---

_Closed by @dhruvmanila on 2024-08-16 14:20_

---
