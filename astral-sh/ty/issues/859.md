```yaml
number: 859
title: "Server panics if `ProgramSettings` of default database are invalid"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - server
  - fatal
assignees: []
created_at: 2025-07-21T07:09:46Z
updated_at: 2025-12-18T14:47:05Z
url: https://github.com/astral-sh/ty/issues/859
synced_at: 2026-01-10T01:53:59Z
```

# Server panics if `ProgramSettings` of default database are invalid

---

_Issue opened by @InSyncWithFoo on 2025-07-21 07:09_

It appears that ty panics when given an URI that does not point to an actual file (and whose parent isn't a directory):

<details>
<summary>Logs with stack traces</summary>

```text
06:52:05,649 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Initializing;0): Starting LSP server 
06:52:05,649 FINE   on.impl.DaemonCodeAnalyzerImpl - Stopping process: toRestartAlarm true myDisposed false reason: 'Write action finish' 
06:52:05,649 FINE   on.impl.DaemonCodeAnalyzerImpl - Rescheduling highlighting: isDone false 
06:52:05,649 INFO   rm.lsp.api.LspServerDescriptor - TYServerDescriptor@diagnostics: starting LSP server: /home/runner/.local/bin/ty [server] 
06:52:05,649 FINE   figurations.GeneralCommandLine - Executing [/home/runner/.local/bin/ty server] 
06:52:05,649 FINE   figurations.GeneralCommandLine -   environment: {} (+CONSOLE) 
06:52:05,649 FINE   figurations.GeneralCommandLine -   charset: UTF-8 
06:52:05,654 FINE   figurations.GeneralCommandLine - Process /home/runner/.local/bin/ty server started with pid 2908 
06:52:05,655 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Initializing;0): LSP server process started: /home/runner/.local/bin/ty server 
06:52:05,660 FINE   connector.Lsp4jServerConnector - TYServerDescriptor@diagnostics: LSP server listener thread started 
06:52:05,660 FINE   connector.Lsp4jServerConnector - TYServerDescriptor@diagnostics: initializing LSP server 
06:52:05,668 FINE   connector.Lsp4jServerConnector - --> TYServerDescriptor@diagnostics: {"jsonrpc":"2.0","id":"1","method":"initialize","params":{"processId":null,"rootPath":"/src","rootUri":"file:///src","initializationOptions":{},"capabilities":{"workspace":{"applyEdit":true,"workspaceEdit":{"documentChanges":true,"resourceOperations":["create"],"failureHandling":"abort","normalizesLineEndings":true},"didChangeWatchedFiles":{"relativePatternSupport":true,"dynamicRegistration":true},"executeCommand":{"dynamicRegistration":false},"workspaceFolders":true,"semanticTokens":{"refreshSupport":true}},"textDocument":{"synchronization":{"willSave":false,"willSaveWaitUntil":false,"didSave":true,"dynamicRegistration":false},"completion":{"completionItem":{"snippetSupport":true,"documentationFormat":["markdown","plaintext"],"deprecatedSupport":true,"tagSupport":{"valueSet":[1]},"insertReplaceSupport":true,"resolveSupport":{"properties":["documentation"]},"labelDetailsSupport":true},"completionItemKind":{"valueSet":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25]},"completionList":{"itemDefaults":["commitCharacters","editRange","insertTextFormat","insertTextMode","data"]}},"hover":{"contentFormat":["markdown","plaintext"]},"references":{"dynamicRegistration":true},"formatting":{"dynamicRegistration":true},"definition":{"linkSupport":true},"typeDefinition":{"linkSupport":true},"codeAction":{"codeActionLiteralSupport":{"codeActionKind":{"valueSet":["quickfix","","source","refactor"]}},"disabledSupport":true,"dataSupport":true,"resolveSupport":{"properties":["edit"]}},"documentLink":{"tooltipSupport":true,"dynamicRegistration":true},"colorProvider":{"dynamicRegistration":true},"publishDiagnostics":{"tagSupport":{"valueSet":[1,2]},"versionSupport":true,"dataSupport":true},"semanticTokens":{"requests":{"range":false,"full":{"delta":false}},"tokenTypes":["namespace","type","class","enum","interface","struct","typeParameter","parameter","variable","property","enumMember","event","function","method","macro","keyword","modifier","comment","string","number","regexp","operator","decorator"],"tokenModifiers":["declaration","definition","readonly","static","deprecated","abstract","async","modification","documentation","defaultLibrary"],"formats":["relative"],"overlappingTokenSupport":true,"multilineTokenSupport":true,"serverCancelSupport":false}},"window":{"showMessage":{},"showDocument":{"support":true}},"general":{"staleRequestSupport":{"cancel":true,"retryOnContentModified":[]}}},"clientInfo":{"name":"PyCharm","version":"251.26927.90"},"workspaceFolders":[{"uri":"file:///src","name":"src"}]}} 
06:52:05,668 FINE   connector.Lsp4jServerConnector - <-- TYServerDescriptor@diagnostics: {"jsonrpc":"2.0","id":"1","result":{"capabilities":{"completionProvider":{"triggerCharacters":["."]},"declarationProvider":true,"definitionProvider":true,"diagnosticProvider":{"identifier":"ty","interFileDependencies":true,"workspaceDiagnostics":false},"hoverProvider":true,"inlayHintProvider":{},"positionEncoding":"utf-16","semanticTokensProvider":{"full":true,"legend":{"tokenModifiers":["definition","readonly","async"],"tokenTypes":["namespace","class","parameter","selfParameter","clsParameter","variable","property","function","method","keyword","string","number","decorator","builtinConstant","typeParameter"]},"range":true},"signatureHelpProvider":{"retriggerCharacters":[")"],"triggerCharacters":["(",","]},"textDocumentSync":{"change":2,"openClose":true},"typeDefinitionProvider":true},"serverInfo":{"name":"ty","version":"0.0.1-alpha.15"}}} 
06:52:05,674 FINE   connector.Lsp4jServerConnector - --> TYServerDescriptor@diagnostics: {"jsonrpc":"2.0","method":"initialized","params":{}} 
06:52:05,674 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Running;0): LSP server initialized in 0.025s, name = ty, version = 0.0.1-alpha.15 
06:52:05,676 FINE   connector.Lsp4jServerConnector - <-- TYServerDescriptor@diagnostics: {"jsonrpc":"2.0","id":0,"method":"workspace/configuration","params":{"items":[{"scopeUri":"file:///src","section":"ty"}]}} 
06:52:05,678 FINE   connector.Lsp4jServerConnector - --> TYServerDescriptor@diagnostics: {"jsonrpc":"2.0","id":0,"result":[null]} 
06:52:05,678 FINE   connector.Lsp4jServerConnector - <-- TYServerDescriptor@diagnostics: {"jsonrpc":"2.0","id":1,"method":"client/registerCapability","params":{"registrations":[{"id":"workspace/didChangeWatchedFiles","method":"workspace/didChangeWatchedFiles","registerOptions":{"watchers":[{"globPattern":"**/ty.toml"},{"globPattern":"**/.gitignore"},{"globPattern":"**/.ignore"},{"globPattern":"**/pyproject.toml"},{"globPattern":"**/*.py"},{"globPattern":"**/*.pyi"},{"globPattern":"**/*.ipynb"}]}}]}} 
06:52:05,678 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Running;0): STDERR: 2025-07-21 06:52:05.678331553  WARN Failed to deserialize workspace options for file:///src: invalid type: null, expected struct ClientOptions. Using default options. 
06:52:05,678 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Running;0): STDERR: 2025-07-21 06:52:05.678617249 ERROR Failed to create project for `/src`: Failed to discover project configuration: project path '/src' is not a directory. Falling back to default settings 
06:52:05,679 FINE   connector.Lsp4jServerConnector - --> TYServerDescriptor@diagnostics: {"jsonrpc":"2.0","id":1,"result":null} 
06:52:05,679 FINE   connector.Lsp4jServerConnector - <-- TYServerDescriptor@diagnostics: {"jsonrpc":"2.0","method":"window/showMessage","params":{"message":"Failed to load project rooted at /src. Please refer to the logs for more details.","type":1}} 
06:52:05,680 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Running;0): window/showMessage: Failed to load project rooted at /src. Please refer to the logs for more details. 
06:52:05,682 FINE   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Running;0): Opening files after server initialization or after move/rename: [temp:///src/invalid-assignment.py] 
06:52:05,682 FINE   on.impl.DaemonCodeAnalyzerImpl - Stopping process: toRestartAlarm true myDisposed false reason: 'Write action finish' 
06:52:05,682 FINE   on.impl.DaemonCodeAnalyzerImpl - Rescheduling highlighting: isDone false 
06:52:05,683 FINE   connector.Lsp4jServerConnector - --> TYServerDescriptor@diagnostics: {"jsonrpc":"2.0","method":"textDocument/didOpen","params":{"textDocument":{"uri":"file:///src/invalid-assignment.py","languageId":"python","version":2,"text":"a: str = 1\n"}}} 
06:52:05,685 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Running;1): STDERR: 2025-07-21 06:52:05.685708676  INFO Defaulting to python-platform `linux` 
06:52:05,686 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Running;1): STDERR: 2025-07-21 06:52:05.686029436 ERROR panicked at crates/ty_server/src/session.rs:364:30: 
Default configuration to be valid: /src does not point to a directory 
   0: <unknown> 
   1: <unknown> 
   2: <unknown> 
   3: <unknown> 
   4: <unknown> 
   5: <unknown> 
   6: <unknown> 
   7: <unknown> 
   8: <unknown> 
   9: <unknown> 
  10: <unknown> 
  11: <unknown> 
  12: <unknown> 
 
panicked at crates/ty_server/src/session.rs:364:30: 
Default configuration to be valid: /src does not point to a directory 
   0: <unknown> 
   1: <unknown> 
   2: <unknown> 
   3: <unknown> 
   4: <unknown> 
   5: <unknown> 
   6: <unknown> 
   7: <unknown> 
   8: <unknown> 
   9: <unknown> 
  10: <unknown> 
  11: <unknown> 
  12: <unknown> 
06:52:05,687 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Running;1): STDERR: 2025-07-21 06:52:05.687145482 ERROR panicked at /root/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/jod-thread-1.0.0/src/lib.rs:33:22: 
called `Result::unwrap()` on an `Err` value: Any { .. } 
   0: <unknown> 
   1: <unknown> 
   2: <unknown> 
   3: <unknown> 
   4: <unknown> 
   5: <unknown> 
   6: <unknown> 
   7: <unknown> 
   8: <unknown> 
   9: <unknown> 
  10: <unknown> 
  11: <unknown> 
  12: <unknown> 
  13: <unknown> 
  14: <unknown> 
  15: __libc_start_main 
  16: <unknown> 
 
panicked at /root/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/jod-thread-1.0.0/src/lib.rs:33:22: 
called `Result::unwrap()` on an `Err` value: Any { .. } 
   0: <unknown> 
   1: <unknown> 
   2: <unknown> 
   3: <unknown> 
   4: <unknown> 
   5: <unknown> 
   6: <unknown> 
   7: <unknown> 
   8: <unknown> 
   9: <unknown> 
  10: <unknown> 
  11: <unknown> 
  12: <unknown> 
  13: <unknown> 
  14: <unknown> 
  15: __libc_start_main 
  16: <unknown> 
06:52:05,689 INFO   latform.lsp.impl.LspServerImpl - TYServerDescriptor@diagnostics(Running;1): LSP server process terminated. Exit code: 101
```
</details>

The URI is from a test framework (IntelliJ's). That ty can't resolve things is expected, but it shouldn't panic here.

---

_Comment by @MichaReiser on 2025-07-21 07:14_

> The URI is from a test framework (IntelliJ's). That ty can't resolve things is expected, but it shouldn't panic here.

Fair. It's not entirely clear what ty should do in this case. I guess the easiest is to simply ignore this owrkspace all together. But I'm not sure if this is more useful in your case. 

Can you tell us more what this test framework is doing? Is it to test LSP functionality?

---

_Label `server` added by @MichaReiser on 2025-07-21 07:14_

---

_Label `fatal` added by @MichaReiser on 2025-07-21 07:14_

---

_Comment by @InSyncWithFoo on 2025-07-21 07:24_

The framework is used under the hood by many tests within the IntelliJ code base (and so is not specifically used to test language servers). On setup, it creates a mock project at some temporary directory; this mock project's path is also set to a mock URI (here, `temp:///src`).

I don't actually understand it either. All I know is that it works in such a manner by default.

---

_Renamed from "Invalid URIs cause panic" to "Invalid workspace root URIs cause panic" by @MichaReiser on 2025-07-21 07:29_

---

_Added to milestone `Beta` by @MichaReiser on 2025-07-28 06:59_

---

_Renamed from "Invalid workspace root URIs cause panic" to "Server panics if `ProgramSettings` of default database aren't invalid" by @MichaReiser on 2025-07-28 07:00_

---

_Comment by @MichaReiser on 2025-07-28 07:01_

Note, this is also an issue outside tests, e.g .when the `VIRTUALEN_ENV` variable points to an invalid directory as described here https://github.com/astral-sh/ty/issues/611#issuecomment-3123403717

Overall, my assumption that resolving the default `ProgramSettings` can never fail was incorrect. 



---

_Assigned to @Gankra by @Gankra on 2025-08-22 14:15_

---

_Comment by @Gankra on 2025-11-12 21:33_

Status: none of the reported ways of inducing a crash happen anymore. Without any known crashers, we maybe should just close? (If anyone knows other settings-based-crashers, do speak up).

---

_Closed by @Gankra on 2025-11-13 18:00_

---

_Reopened by @zanieb on 2025-12-17 20:47_

---

_Comment by @zanieb on 2025-12-17 20:48_

Opening because https://github.com/astral-sh/ty/issues/2031 encounters this panic.

---

_Renamed from "Server panics if `ProgramSettings` of default database aren't invalid" to "Server panics if `ProgramSettings` of default database are invalid" by @carljm on 2025-12-17 21:23_

---

_Closed by @Gankra on 2025-12-18 14:47_

---
