---
number: 17944
title: "Server doesn't detect ruff config for files not under working directory of server"
type: issue
state: open
author: harrymander
labels:
  - server
assignees: []
created_at: 2025-05-08T11:12:40Z
updated_at: 2025-05-08T12:50:45Z
url: https://github.com/astral-sh/ruff/issues/17944
synced_at: 2026-01-07T13:12:16-06:00
---

# Server doesn't detect ruff config for files not under working directory of server

---

_Issue opened by @harrymander on 2025-05-08 11:12_

### Summary

`ruff server` does not find settings (e.g. `ruff.toml`) when processing files that are not under the directory that the server command was invoked in, instead falling back to the default config.

This was tested using Sublime Text with the sublimelsp package using the following directory structure:

```
ruff-server-test
├── project-a
│   ├── ruff.toml
│   └── test.py
└── project-b <- ruff server invoked in this directory
    ├── ruff.toml
    └── test.py
```

If the Sublime workspace is opened in `project-b`, and `project-a/test.py` is added to the workspace as a "free" file, calling `textDocument/formatting` causes the file to be formatted incorrectly. Debug logs including a server restart are shown below (I have isolated lines the relevant lines and added  `!!!` at start).

```
LSP-ruff: 2025-05-08 22:48:24.102709487  INFO Shutdown request received. Waiting for an exit notification...
[22:48:24.102] --> LSP-ruff shutdown (4): None
LSP-ruff: 2025-05-08 22:48:24.124943287  INFO Exit notification received. Server shutting down...
[22:48:24.124] --> LSP-ruff initialize (1): {'processId': 25194, 'clientInfo': {'name': 'Sublime Text LSP', 'version': '2.3.0'}, 'rootUri': 'file:///home/harry/scratch/ruff-server-test/project-b', 'rootPath': '/home/harry/scratch/ruff-server-test/project-b', 'workspaceFolders': [{'name': 'project-b', 'uri': 'file:///home/harry/scratch/ruff-server-test/project-b'}], 'capabilities': {'general': {'regularExpressions': {'engine': 'ECMAScript'}, 'markdown': {'parser': 'Python-Markdown', 'version': '3.2.2'}}, 'textDocument': {'synchronization': {'dynamicRegistration': True, 'didSave': True, 'willSave': True, 'willSaveWaitUntil': True}, 'hover': {'dynamicRegistration': True, 'contentFormat': ['markdown', 'plaintext']}, 'completion': {'dynamicRegistration': True, 'completionItem': {'snippetSupport': True, 'deprecatedSupport': True, 'documentationFormat': ['markdown', 'plaintext'], 'tagSupport': {'valueSet': [1]}, 'resolveSupport': {'properties': ['detail', 'documentation', 'additionalTextEdits']}, 'insertReplaceSupport': True, 'insertTextModeSupport': {'valueSet': [2]}, 'labelDetailsSupport': True}, 'completionItemKind': {'valueSet': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25]}, 'insertTextMode': 2, 'completionList': {'itemDefaults': ['editRange', 'insertTextFormat', 'data']}}, 'signatureHelp': {'dynamicRegistration': True, 'contextSupport': True, 'signatureInformation': {'activeParameterSupport': True, 'documentationFormat': ['markdown', 'plaintext'], 'parameterInformation': {'labelOffsetSupport': True}}}, 'references': {'dynamicRegistration': True}, 'documentHighlight': {'dynamicRegistration': True}, 'documentSymbol': {'dynamicRegistration': True, 'hierarchicalDocumentSymbolSupport': True, 'symbolKind': {'valueSet': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26]}, 'tagSupport': {'valueSet': [1]}}, 'documentLink': {'dynamicRegistration': True, 'tooltipSupport': True}, 'formatting': {'dynamicRegistration': True}, 'rangeFormatting': {'dynamicRegistration': True, 'rangesSupport': True}, 'declaration': {'dynamicRegistration': True, 'linkSupport': True}, 'definition': {'dynamicRegistration': True, 'linkSupport': True}, 'typeDefinition': {'dynamicRegistration': True, 'linkSupport': True}, 'implementation': {'dynamicRegistration': True, 'linkSupport': True}, 'codeAction': {'dynamicRegistration': True, 'codeActionLiteralSupport': {'codeActionKind': {'valueSet': ['quickfix', 'refactor', 'refactor.extract', 'refactor.inline', 'refactor.rewrite', 'source.fixAll', 'source.organizeImports']}}, 'dataSupport': True, 'isPreferredSupport': True, 'resolveSupport': {'properties': ['edit']}}, 'rename': {'dynamicRegistration': True, 'prepareSupport': True, 'prepareSupportDefaultBehavior': 1}, 'colorProvider': {'dynamicRegistration': True}, 'publishDiagnostics': {'relatedInformation': True, 'tagSupport': {'valueSet': [1, 2]}, 'versionSupport': True, 'codeDescriptionSupport': True, 'dataSupport': True}, 'diagnostic': {'dynamicRegistration': True, 'relatedDocumentSupport': True}, 'selectionRange': {'dynamicRegistration': True}, 'foldingRange': {'dynamicRegistration': True, 'foldingRangeKind': {'valueSet': ['comment', 'imports', 'region']}}, 'codeLens': {'dynamicRegistration': True}, 'inlayHint': {'dynamicRegistration': True, 'resolveSupport': {'properties': ['textEdits', 'label.command']}}, 'semanticTokens': {'dynamicRegistration': True, 'requests': {'range': True, 'full': {'delta': True}}, 'tokenTypes': ['namespace', 'type', 'class', 'enum', 'interface', 'struct', 'typeParameter', 'parameter', 'variable', 'property', 'enumMember', 'event', 'function', 'method', 'macro', 'keyword', 'modifier', 'comment', 'string', 'number', 'regexp', 'operator', 'decorator', 'label'], 'tokenModifiers': ['declaration', 'definition', 'readonly', 'static', 'deprecated', 'abstract', 'async', 'modification', 'documentation', 'defaultLibrary'], 'formats': ['relative'], 'overlappingTokenSupport': False, 'multilineTokenSupport': True, 'augmentsSyntaxTokens': True}, 'callHierarchy': {'dynamicRegistration': True}, 'typeHierarchy': {'dynamicRegistration': True}}, 'workspace': {'applyEdit': True, 'didChangeConfiguration': {'dynamicRegistration': True}, 'executeCommand': {}, 'workspaceEdit': {'documentChanges': True, 'failureHandling': 'abort'}, 'workspaceFolders': True, 'symbol': {'dynamicRegistration': True, 'resolveSupport': {'properties': ['location.range']}, 'symbolKind': {'valueSet': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26]}, 'tagSupport': {'valueSet': [1]}}, 'configuration': True, 'codeLens': {'refreshSupport': True}, 'inlayHint': {'refreshSupport': True}, 'semanticTokens': {'refreshSupport': True}, 'diagnostics': {'refreshSupport': True}}, 'window': {'showDocument': {'support': True}, 'showMessage': {'messageActionItem': {'additionalPropertiesSupport': True}}, 'workDoneProgress': True}}, 'initializationOptions': {'settings': {'codeAction': {'disableRuleComment': {'enable': True}, 'fixViolation': {'enable': True}}, 'configuration': None, 'configurationPreference': 'editorFirst', 'exclude': None, 'fixAll': True, 'format': {'preview': None}, 'lineLength': None, 'lint': {'enable': True, 'extendSelect': None, 'ignore': None, 'preview': None, 'select': None}, 'logLevel': 'debug', 'organizeImports': True}}}
[22:48:24.124] <<< LSP-ruff (4) (duration: 22ms): None
[22:48:24.124]  -> LSP-ruff exit: None
LSP-ruff: 2025-05-08 22:48:24.128674236  INFO No workspace settings found for file:///home/harry/scratch/ruff-server-test/project-b, using default settings
LSP-ruff: 2025-05-08 22:48:24.128733887 DEBUG Negotiated position encoding: UTF16
LSP-ruff: 2025-05-08 22:48:24.128745653 DEBUG Indexing settings for workspace: /home/harry/scratch/ruff-server-test/project-b
LSP-ruff: 2025-05-08 22:48:24.130593970 DEBUG No `target-version` found in `ruff.toml` log.target="ruff_workspace::pyproject" log.module_path="ruff_workspace::pyproject" log.file="crates/ruff_workspace/src/pyproject.rs" log.line=179
LSP-ruff: 2025-05-08 22:48:24.130643360 DEBUG No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified log.target="ruff_workspace::pyproject" log.module_path="ruff_workspace::pyproject" log.file="crates/ruff_workspace/src/pyproject.rs" log.line=188

!!! LSP-ruff: 2025-05-08 22:48:24.130856678 DEBUG Loaded settings from: `/home/harry/scratch/ruff-server-test/project-b/ruff.toml` for `/home/harry/scratch/ruff-server-test/project-b`

LSP-ruff: 2025-05-08 22:48:24.130978834 DEBUG Ignored path via `exclude`: /home/harry/scratch/ruff-server-test/project-b/.ruff_cache
LSP-ruff: 2025-05-08 22:48:24.131981675  INFO Registering workspace: /home/harry/scratch/ruff-server-test/project-b
LSP-ruff: 2025-05-08 22:48:24.132271587  WARN LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.
[22:48:24.127] <<< LSP-ruff (1) (duration: 3ms): {'capabilities': {'codeActionProvider': {'codeActionKinds': ['quickfix', 'source.fixAll.ruff', 'source.organizeImports.ruff', 'notebook.source.fixAll.ruff', 'notebook.source.organizeImports.ruff'], 'resolveProvider': True, 'workDoneProgress': True}, 'diagnosticProvider': {'identifier': 'Ruff', 'interFileDependencies': False, 'workDoneProgress': True, 'workspaceDiagnostics': False}, 'documentFormattingProvider': True, 'documentRangeFormattingProvider': True, 'executeCommandProvider': {'commands': ['ruff.applyFormat', 'ruff.applyAutofix', 'ruff.applyOrganizeImports', 'ruff.printDebugInformation'], 'workDoneProgress': False}, 'hoverProvider': True, 'notebookDocumentSync': {'notebookSelector': [{'cells': [{'language': 'python'}]}], 'save': False}, 'positionEncoding': 'utf-16', 'workspace': {'workspaceFolders': {'changeNotifications': True, 'supported': True}}, 'textDocumentSync': {'change': {'syncKind': 2}, 'didOpen': {}, 'didClose': {}}}, 'serverInfo': {'name': 'ruff', 'version': '0.11.8'}}
[22:48:24.128]  -> LSP-ruff initialized: {}

!!! LSP-ruff: 2025-05-08 22:48:24.148410097  WARN No settings available for file:///home/harry/scratch/ruff-server-test/project-a/test.py - falling back to default settings

LSP-ruff: 2025-05-08 22:48:24.148808138 DEBUG Included path via `include`: /home/harry/scratch/ruff-server-test/project-a/test.py
LSP-ruff: 2025-05-08 22:48:24.160964661 DEBUG Included path via `include`: /home/harry/scratch/ruff-server-test/project-b/test.py
[22:48:24.146]  -> LSP-ruff textDocument/didOpen: {'textDocument': {'uri': 'file:///home/harry/scratch/ruff-server-test/project-a/test.py', 'languageId': 'python', 'version': 0, 'text': 'a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]\n'}}
[22:48:24.148] --> LSP-ruff textDocument/diagnostic (2): {'textDocument': {'uri': 'file:///home/harry/scratch/ruff-server-test/project-a/test.py'}, 'identifier': 'Ruff'}
[22:48:24.160]  -> LSP-ruff textDocument/didOpen: {'textDocument': {'uri': 'file:///home/harry/scratch/ruff-server-test/project-b/test.py', 'languageId': 'python', 'version': 14, 'text': 'a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]\n'}}
[22:48:24.160] --> LSP-ruff textDocument/diagnostic (3): {'textDocument': {'uri': 'file:///home/harry/scratch/ruff-server-test/project-b/test.py'}, 'identifier': 'Ruff'}
[22:48:24.176] <<< LSP-ruff (2) (duration: 28ms): {'items': [], 'kind': 'full'}
[22:48:24.179] <<< LSP-ruff (3) (duration: 18ms): {'items': [], 'kind': 'full'}

!!! LSP-ruff: 2025-05-08 22:48:34.373418407  WARN No settings available for file:///home/harry/scratch/ruff-server-test/project-a/test.py - falling back to default settings

LSP-ruff: 2025-05-08 22:48:34.373891352 DEBUG Included path via `include`: /home/harry/scratch/ruff-server-test/project-a/test.py
LSP-ruff: 2025-05-08 22:48:34.374117597 DEBUG Printer::print: enter
[22:48:34.373] --> LSP-ruff textDocument/formatting (4): {'textDocument': {'uri': 'file:///home/harry/scratch/ruff-server-test/project-a/test.py'}, 'options': {'tabSize': 4, 'insertSpaces': True, 'trimTrailingWhitespace': True, 'insertFinalNewline': True, 'trimFinalNewlines': True}, 'workDoneToken': '$ublime-work-done-progress-4'}
[22:48:34.379] <<< LSP-ruff (4) (duration: 6ms): None
``` 

If the sublime workspace is opened in the directory above (`ruff-server-test`), then the server detects both `ruff.toml`s and formats each file according to their respective configs. Not sure if this is really a bug or the intended behaviour, but (at least for me) it's  common to  open files that are not in the workspace - if the editor is configured to auto-format on save these files will be incorrectly formatted.

### Version

ruff 0.11.8

---

_Label `server` added by @ntBre on 2025-05-08 12:50_

---
