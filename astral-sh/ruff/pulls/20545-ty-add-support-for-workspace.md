```yaml
number: 20545
title: "[ty] Add support for workspace/didChangeConfiguration notification"
type: pull_request
state: closed
author: Guillaume-Fgt
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: didChangeConfiguration_server
created_at: 2025-09-24T07:57:00Z
updated_at: 2025-12-10T17:41:44Z
url: https://github.com/astral-sh/ruff/pull/20545
synced_at: 2026-01-12T15:57:04Z
```

# [ty] Add support for workspace/didChangeConfiguration notification

---

_@Guillaume-Fgt_

## Summary

Hi, I started working on implementing didChangeConfiguration in response to https://github.com/astral-sh/ty/issues/953 but I need help.

The current status of my PR (draft):
* I see the capability I added in Client capabilities: ResolvedClientCapabilities(
    WORKSPACE_DIAGNOSTIC_REFRESH | ... | **DID_CHANGE_CONFIGURATION**
* `didChangeConfiguration` notifications sent by VSCode to the server are catched. But here is my first question: VSCode only send this notification when I change 

> Ty › Trace: Server Traces the communication between VSCode and the ty language server.

```
[Trace - 9:35:32 AM] Sending notification '$/setTrace'.
[Trace - 9:35:32 AM] Sending notification 'workspace/didChangeConfiguration'.
[Trace - 9:35:32 AM] Received request 'workspace/configuration - (3)'.
[Trace - 9:35:32 AM] Sending response 'workspace/configuration - (3)'. Processing request took 1ms
```

 For all the other settings (12 in total for ty), VSCode send a `request shutdown` instead. 

```
[Trace - 7:58:21 AM] Sending request 'shutdown - (16)'.
[Trace - 7:58:21 AM] Received response 'shutdown - (16)' in 2ms.
[Trace - 7:58:21 AM] Sending notification 'exit'.
[Trace - 7:58:21 AM] Sending request 'initialize - (0)'.
[Trace - 7:58:21 AM] Received response 'initialize - (0)' in 20ms.
[Trace - 7:58:21 AM] Sending notification 'initialized'.
[Trace - 7:58:21 AM] Sending notification 'textDocument/didOpen'.
[Trace - 7:58:21 AM] Received request 'workspace/configuration - (0)'.
[Trace - 7:58:21 AM] Sending response 'workspace/configuration - (0)'. Processing request took 1ms
[Trace - 7:58:21 AM] Received request 'client/registerCapability - (1)'.
[Trace - 7:58:21 AM] Sending response 'client/registerCapability - (1)'. Processing request took 1ms
[Trace - 7:58:21 AM] Sending request 'textDocument/diagnostic - (1)'.
[Trace - 7:58:21 AM] Received response 'textDocument/diagnostic - (1)' in 379ms.
[Trace - 7:58:21 AM] Sending request 'textDocument/diagnostic - (2)'.
[Trace - 7:58:21 AM] Received response 'textDocument/diagnostic - (2)' in 1ms.
```
If I change a settings outside of ty, I get the `didChangeConfiguration` notification. For example, for `Editor: format on save`.
Is it a normal behavior? In my understanding, we don't control what the client send to us but maybe I missed something...

* when the server receive the notification, the `DidChangeConfigurationParams` is empty. It is normal according to [lsp specs](https://github.com/astral-sh/ty/issues/82#issuecomment-3045358677) and some github issues I found ([1]( https://github.com/microsoft/vscode-languageserver-node/issues/380#issuecomment-414691493) - [2]( https://github.com/microsoft/language-server-protocol/issues/676)). So in my understanding, we have to pull the settings from the client and apply them to all managed workspaces in the session.  I began to implement something and I get this for the moment (one python file open):

```
2025-09-24 09:36:33.201403900  INFO GLOBAL: GlobalSettings { diagnostic_mode: OpenFilesOnly, experimental: ExperimentalSettings { rename: true, auto_import: true } }

2025-09-24 09:36:33.201471700  INFO INIT: InitializationOptions { log_level: Some(Info), log_file: None, options: ClientOptions { global: GlobalOptions { diagnostic_mode: None, experimental: None }, workspace: WorkspaceOptions { disable_language_services: None, inlay_hints: None, python_extension: None }, unknown: {} } }

2025-09-24 09:36:33.207605400  INFO WORKSPACES: [(Url { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/c%3A/Users/guill/Documents/Python/Advent_Of_Code", query: None, fragment: None }, ClientOptions { global: GlobalOptions { diagnostic_mode: Some(OpenFilesOnly), experimental: Some(Experimental { rename: Some(true), auto_import: Some(true) }) }, workspace: WorkspaceOptions { disable_language_services: Some(true), inlay_hints: Some(InlayHintOptions { variable_types: Some(false), call_argument_names: Some(false) }), python_extension: Some(PythonExtension { active_environment: Some(ActiveEnvironment { executable: PythonExecutable { uri: Url { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/c%3A/Users/guill/Documents/Python/Advent_Of_Code/.venv/Scripts/python.exe", query: None, fragment: None }, sys_prefix: "C:\\Users\\guill\\Documents\\Python\\Advent_Of_Code\\.venv" }, environment: Some(PythonEnvironment { folder_uri: Url { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/c%3A/Users/guill/Documents/Python/Advent_Of_Code/.venv/Scripts/python.exe", query: None, fragment: None }, kind: "VirtualEnvironment", name: None }), version: Some(EnvironmentVersion { major: 3, minor: 12, patch: 2, sys_version: "3.12.2.final.0" }) }) }) }, unknown: {"importStrategy": String("fromEnvironment"), "logFile": Null, "logLevel": String("info"), "interpreter": Array [], "path": Array [String("C:\\Users\\guill\\Documents\\Rust\\others\\ruff\\target\\debug\\ty.exe")], "trace": Object {"server": String("messages")}} })]
```

But I am not sure I used the correct way to retrieve the settings and which one to choose. For example, I see there is a `request_workspace_configurations` in `main_loop.rs` but don't know how I can access it. So I duplicated some code to retrieve the settings for INFO WORKSPACES. And after, we will have to apply them to the session. I imagine with `session.apply_changes()`

TLDR: 
My goals by opening this draft PR:
- Is my implementation of didChangeConfiguration notification is correct?
- help to handle the server response and discuss the best strategy (gather client settings, change session settings).

I hope I am helping you by opening this draft PR, not complicating your work. You can tell me, no offense taken.
## Test Plan

Not relevant for the moment.


---

_Comment by @github-actions[bot] on 2025-09-24 07:58_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-24 07:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-09-24 08:07_

Thank you for working on this. 

Regarding the notification that VS code sends. This is because of 

https://github.com/astral-sh/ty-vscode/blob/3732ee9dc82feeffc4ca459897318418e136ccaa/src/extension.ts#L152-L156

We'll need something more elaborated that decides whether to restart the server or sending `didChangeConfiguration` dependent on which settings have changed (`ty.path` requires a restart, there might be other options requiring a restart)

---

_Label `server` added by @MichaReiser on 2025-09-24 08:07_

---

_Label `ty` added by @MichaReiser on 2025-09-24 08:07_

---

_Comment by @Guillaume-Fgt on 2025-09-24 12:40_

oh, ok, I understand now. In ty-vscode, by just simply deactivating the server restart like that:

```ts
    onDidChangeConfiguration(async () => {
      // TODO(dhruvmanila): Notify the server with `DidChangeConfigurationNotification` and let
      // the server pull in the updated configuration.
    }),
```
I have the `didChangeConfiguration` triggering for all ty settings in VScode. I also tried this version where I receive a double notification when it's a ty parameter: one automatically triggered for all parameters, the other by calling sendNotification  for ty settings:

```ts
    onDidChangeConfiguration(async (e: vscode.ConfigurationChangeEvent) => {
      if (e.affectsConfiguration('ty')) {
           const client = getClient();
           if (client) {
               client.sendNotification("workspace/didChangeConfiguration");
    }),
```




---

_Comment by @MichaReiser on 2025-12-10 17:41_

Thanks for your contribution. I'll close this PR because it has become stale. Please don't hesitate to resubmit the PR and reference this PR if you plan to keep working on this feature.

---

_Closed by @MichaReiser on 2025-12-10 17:41_

---
