```yaml
number: 2417
title: "Semantic-tokens panic while typing out a `match`/`case` statement"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - server
  - fatal
assignees: []
created_at: 2026-01-09T14:28:00Z
updated_at: 2026-01-09T15:04:52Z
url: https://github.com/astral-sh/ty/issues/2417
synced_at: 2026-01-12T15:54:26Z
```

# Semantic-tokens panic while typing out a `match`/`case` statement

---

_@AlexWaygood_

### Summary

I'm getting repeated semantic-tokens panics in the server while typing out `match`/`case` statements in a Python project. I managed to create this standalone video to reproduce it:

https://github.com/user-attachments/assets/66c7d0b3-cde8-45d2-950e-5d49a7367867

The code at the end of the video was:

```py
import ast

def f(x: ast.AST):
    match x:
        case ast.Attribute(value=ast.Name(id), attr)
```

and the logs are:

```
2026-01-09 14:24:20.637830000  INFO Version: ruff/0.14.10+238 (de9f3f90e 2026-01-07)
2026-01-09 14:24:20.656786000  INFO Defaulting to python-platform `darwin`
2026-01-09 14:24:20.657848000  INFO Python version: Python 3.13, platform: darwin
2026-01-09 14:24:20.664373000  INFO Indexed 1 file(s) in 0.004s
2026-01-09 14:24:27.673639000  INFO Defaulting to python-platform `darwin`
2026-01-09 14:24:27.674723000  INFO Python version: Python 3.13, platform: darwin
2026-01-09 14:24:27.678103000  INFO Indexed 1 file(s) in 0.003s
[Error - 14:24:42] Request textDocument/documentSymbol failed.
Error: name must not be falsy
	at bp.validate (file:///Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/workbench/api/node/extensionHostProcess.js:112:26946)
	at new bp (file:///Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/workbench/api/node/extensionHostProcess.js:112:27235)
	at V (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:275579)
	at c (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:18138)
	at t.map (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:18224)
	at Object.asDocumentSymbols (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:283635)
	at i (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:347081)
	at runNextTicks (node:internal/process/task_queues:65:5)
	at process.processImmediate (node:internal/timers:453:9)
	at process.callbackTrampoline (node:internal/async_hooks:130:17)
	at async RN.provideDocumentSymbols (file:///Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/workbench/api/node/extensionHostProcess.js:144:140602)
[Error - 14:24:42] Request textDocument/documentSymbol failed.
Error: name must not be falsy
	at bp.validate (file:///Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/workbench/api/node/extensionHostProcess.js:112:26946)
	at new bp (file:///Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/workbench/api/node/extensionHostProcess.js:112:27235)
	at V (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:275579)
	at c (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:18138)
	at t.map (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:18224)
	at Object.asDocumentSymbols (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:283635)
	at i (/Users/alexw/.vscode/extensions/astral-sh.ty-2026.2.0-darwin-arm64/dist/extension.js:1:347081)
	at async RN.provideDocumentSymbols (file:///Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/workbench/api/node/extensionHostProcess.js:144:140602)
2026-01-09 14:25:20.141538000 ERROR An error occurred with request ID 369: request handler panicked at crates/ty_ide/src/semantic_tokens.rs:248:9:
Tokens must be added in file order: previous token ends at Some(89), new token starts at 77query stacktrace:


run with `RUST_BACKTRACE=1` environment variable to display a backtrace

[Error - 14:25:20] Request textDocument/semanticTokens/full failed.
  Message: request handler panicked at crates/ty_ide/src/semantic_tokens.rs:248:9:
Tokens must be added in file order: previous token ends at Some(89), new token starts at 77query stacktrace:


run with `RUST_BACKTRACE=1` environment variable to display a backtrace

  Code: -32603 
2026-01-09 14:25:20.562263000 ERROR An error occurred with request ID 377: request handler panicked at crates/ty_ide/src/semantic_tokens.rs:248:9:
Tokens must be added in file order: previous token ends at Some(91), new token starts at 77query stacktrace:


run with `RUST_BACKTRACE=1` environment variable to display a backtrace

[Error - 14:25:20] Request textDocument/semanticTokens/full failed.
  Message: request handler panicked at crates/ty_ide/src/semantic_tokens.rs:248:9:
Tokens must be added in file order: previous token ends at Some(91), new token starts at 77query stacktrace:


run with `RUST_BACKTRACE=1` environment variable to display a backtrace

  Code: -32603 
```

### Version

_No response_

---

_Label `server` added by @AlexWaygood on 2026-01-09 14:28_

---

_Label `fatal` added by @AlexWaygood on 2026-01-09 14:28_

---

_Label `bug` added by @AlexWaygood on 2026-01-09 14:28_

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2026-01-09 14:28_

---

_Assigned to @Gankra by @Gankra on 2026-01-09 14:45_

---

_Comment by @MichaReiser on 2026-01-09 15:04_

This suggests that we aren't traversing the match nodes in source order, but in some other order 

---
