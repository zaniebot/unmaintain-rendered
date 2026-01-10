```yaml
number: 11831
title: "`ruff server`: Introduce the `ruff.printDebugInformation` command"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/command/print-debug
created_at: 2024-06-10T20:56:32Z
updated_at: 2024-06-13T03:06:12Z
url: https://github.com/astral-sh/ruff/pull/11831
synced_at: 2026-01-10T21:56:00Z
```

# `ruff server`: Introduce the `ruff.printDebugInformation` command

---

_Pull request opened by @snowsignal on 2024-06-10 20:56_

## Summary

Closes #11715.

Introduces a new command, `ruff.printDebugInformation`. This will print useful information about the status of the server to `stderr`.

Right now, the information shown by this command includes:
* The path to the server executable
* The version of the executable
* The text encoding being used
* The number of open documents and workspaces
* A list of registered configuration files
* The capabilities of the client

## Test Plan

First, checkout and use [the corresponding `ruff-vscode` PR](https://github.com/astral-sh/ruff-vscode/pull/495).

Running the `Print debug information` command in VS Code should show something like the following in the Output channel:

<img width="991" alt="Screenshot 2024-06-11 at 11 41 46 AM" src="https://github.com/astral-sh/ruff/assets/19577865/ab93c009-bb7b-4291-b057-d44fdc6f9f86">




---

_Label `server` added by @snowsignal on 2024-06-10 20:56_

---

_Review requested from @MichaReiser by @snowsignal on 2024-06-10 21:06_

---

_Comment by @github-actions[bot] on 2024-06-10 21:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:196 on 2024-06-11 06:26_

Nit: The method names suggest that the implementation counts the documents and workspaces but that isn't the case, it just takes the len. Maybe `num_documents()` and `num_workspaces`?

---

_@MichaReiser approved on 2024-06-11 06:26_

This is excellent! Do we have a place where we could document the new command? Like, have an issue, run this command and create an issue in the RUff repository.

Would it be possible to show the user a message hinting them that they should look at the output panel? It may otherwise not be clear where they can find the debug output

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:44 on 2024-06-11 06:33_

nit: slight preference to just use `==` instead (`if command == Command::Debug`)

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:50 on 2024-06-11 06:33_

You might already be aware of this but printing to stderr is always displayed with Error log level in Neovim. We don't need to solve this here but I'm not sure if the server should print anything to stderr.

---

_@dhruvmanila approved on 2024-06-11 06:35_

Thank you for doing this! This will be very helpful

---

_@dhruvmanila reviewed on 2024-06-11 06:38_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:172 on 2024-06-11 06:38_

nit: would "executable" be more useful than "path" as it's a path to the `ruff` executable?

---

_@dhruvmanila reviewed on 2024-06-11 06:41_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:50 on 2024-06-11 06:41_

Pyright sends this information as a log message at INFO level: https://github.com/microsoft/pyright/blob/2b0a1d92dda37fb80420e80d1d3933d9287be21f/packages/pyright-internal/src/commands/dumpFileDebugInfoCommand.ts#L239-L241

---

_@dhruvmanila reviewed on 2024-06-11 06:44_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:50 on 2024-06-11 06:44_

It's a bit unreadable in Neovim unless the user would manually format it.
```
[ERROR][2024-06-11 12:14:00] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/dhruv/work/astral/ruff/target/debug/ruff"	"stderr"	'Debug information:\npath = /Users/dhruv/work/astral/ruff/target/debug/ruff\nversion = 0.4.8\nencoding = UTF16\nopen_document_count = 1\nactive_workspace_count = 1\nconfiguration_files = ["/Users/dhruv/playground/ruff/pyproject.toml", "/Users/dhruv/playground/ruff/repro/ruff.toml"]\ncapabilities.code_action_deferred_edit_resolution = true\ncapabilities.apply_edit = true\ncapabilities.document_changes = false\ncapabilities.workspace_refresh = true\ncapabilities.pull_diagnostics = true\n\n    \n'
```

Edit: Compared to sending this as a log message (Pyright's output as an example):
```
2024-05-19 09:37:35 [INFO] * Dump debug info for '/Users/dhruv/playground/ruff/parser/_.py'
* Token info (5 tokens)
[0] '/Users/dhruv/playground/ruff/parser/_.py:1:1' (Identifier, (0,0)-(0,1)) {"start":0,"length":1,"type":7,"value":"x"}
[1] '/Users/dhruv/playground/ruff/parser/_.py:1:3' (Operator, Assign, (0,2)-(0,3)) {"start":2,"length":1,"type":9,"operatorType":2}
[2] '/Users/dhruv/playground/ruff/parser/_.py:1:5' (Number, (0,4)-(0,5)) {"start":4,"length":1,"type":6,"isInteger":true,"isImaginary":false,"value":1}
[3] '/Users/dhruv/playground/ruff/parser/_.py:1:6' (NewLine, LineFeed, (0,5)-(1,0)) {"start":5,"length":1,"type":2,"newLineType":1}
[4] '/Users/dhruv/playground/ruff/parser/_.py:2:1' (EndOfStream, (1,0)-(1,0)) {"start":6,"length":0,"type":1}
```

---

_@snowsignal reviewed on 2024-06-11 14:21_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/execute_command.rs`:44 on 2024-06-11 14:21_

The advantage of `let` is that we don't need to add a `PartialEq` implementation to `Command` - but that's fine, I'm not that opinionated on this.

---

_@snowsignal reviewed on 2024-06-11 14:23_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/execute_command.rs`:50 on 2024-06-11 14:23_

Sure, we can make this a log message. This means it won't appear on Helix, but Helix doesn't support LSP commands anyway, so it should be fine.

---

_Merged by @snowsignal on 2024-06-11 18:42_

---

_Closed by @snowsignal on 2024-06-11 18:42_

---

_Branch deleted on 2024-06-11 18:42_

---

_Comment by @rainzee on 2024-06-12 08:46_

Considering a command that could be added to vsc? Using the C-S-p command center

![image](https://github.com/astral-sh/ruff/assets/4585440/1ea1f771-ca86-44da-8a3b-2f549c8407b1)


---

_Comment by @dhruvmanila on 2024-06-12 08:48_

@rainzee I think https://github.com/astral-sh/ruff-vscode/pull/495 does that.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:50 on 2024-06-13 03:06_

Thank you for doing that! Now, it's much better:
```
2024-06-13 07:58:53 [INFO] executable = /Users/dhruv/work/astral/ruff/target/debug/ruff
version = 0.4.8
encoding = UTF16
open_document_count = 1
active_workspace_count = 1
configuration_files = ["/Users/dhruv/playground/ruff/pyproject.toml", "/Users/dhruv/playground/ruff/repro/ruff.toml"]
capabilities.code_action_deferred_edit_resolution = true
capabilities.apply_edit = true
capabilities.document_changes = false
capabilities.workspace_refresh = true
capabilities.pull_diagnostics = true
```

---

_@dhruvmanila reviewed on 2024-06-13 03:06_

---
