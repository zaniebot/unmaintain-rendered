```yaml
number: 17944
title: "Server doesn't detect ruff config for files not under working directory of server"
type: issue
state: open
author: harrymander
labels:
  - server
assignees: []
created_at: 2025-05-08T11:12:40Z
updated_at: 2026-01-16T10:38:52Z
url: https://github.com/astral-sh/ruff/issues/17944
synced_at: 2026-01-16T11:07:22Z
```

# Server doesn't detect ruff config for files not under working directory of server

---

_@harrymander_

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

_Comment by @ZedThree on 2026-01-13 18:01_

Apologies if this is too much detail, just getting everything out of my head! Possible fix is below the line.

A brief summary of the situation from other issues:
- when an editor opens a single file directly, `ruff server` gets an initial (default) workspace which is the current working directory
- for editors not launched from the terminal, the cwd is usually something like `/` or `~`, and since we don't want to scan the whole filesystem, only the cwd and above are scanned for config files (in `RuffSettingsIndex::new`), and then we bail without going deeper
- now, when we open a file not directly in the cwd we get one of two situations:
  - if the file is _under_ the cwd, we use the settings from whatever config file was found in the cwd
  - otherwise, we use the default settings

I have the following setup which I think demonstrates both this issue and the setup in #22536:

```
/tmp/mvce
├── dir_0
│   ├── dir_0_1
│   │   ├── ruff.toml
│   │   └── test_0_1.py
│   ├── ruff.toml
│   └── test_0.py
└── dir_1
    ├── ruff.toml
    └── test_1.py
```

The three `.py` files are all identical:

```py
import os

with open("foo") as f:
    contents = f.read()
```

The config files are different, however:

`dir_0/ruff.toml`:
```toml
[lint]
select = ["FURB", "F"]
preview = true
```

whereas both `dir_1/ruff.toml` and `dir_0_1/ruff.toml`:
```toml
[lint]
select = ["FURB"]
preview = false
```

So that `dir_0/test_0.py` has two warnings, but `dir_1/test_1.py` and `dir_0_1/test_0_1.py` have none -- unless the default settings are used for a file, in which case we get one warning for that file, `F401` on `import os`.

Results summary (I've used nvim here, but this is consistent across editors):
1. `cd dir_0; nvim test_0.py` or `cd dir_1; nvim test_1.py` gives the expected warnings
2. `nvim dir_0/test_0.py` or `nvim dir_1/test_1.py` only gives one warning (bug)
   - this is the issue from #22536
3. `cd dir_0; nvim test_0.py` then `:edit dir_0_1/test_0_1.py` gives two warnings (bug)   - this is because `dir_0/ruff.toml` is used instead of `dir_0_1/ruff.toml`
   - this is _maybe_ the same issue as #20847, with the parent directory taking precedence over the closer config file
4. `cd dir_0; nvim test_0.py` then `:edit ../dir_1/test_1.py` gives one warning (bug)
   - as does the other way round
   - this is the original situation in this issue

Details from the log file from running the above cases in order:

<details>

I've also added my own extra logging, mostly in `session::Index::open_text_document`

1. `cd dir_0; nvim test_0.py`
```
 INFO No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /tmp/mvce/dir_0
 INFO No workspace options found for file:///tmp/mvce/dir_0, using default options
DEBUG Negotiated position encoding: UTF8
DEBUG Indexing settings for workspace: /tmp/mvce/dir_0
DEBUG No `target-version` found in `ruff.toml`
DEBUG No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified
DEBUG Loaded settings from: `/tmp/mvce/dir_0/ruff.toml`
 INFO Registering workspace: /tmp/mvce/dir_0
 WARN LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.
DEBUG notification{method="textDocument/didOpen"}: enter
DEBUG notification{method="textDocument/didOpen"}: *** file:///tmp/mvce/dir_0/test_0.py is in index
DEBUG notification{method="textDocument/didOpen"}: **** contains key: true
DEBUG notification{method="textDocument/didOpen"}: *** has settings: true
DEBUG request{id=2 method="textDocument/diagnostic"}: enter
DEBUG request{id=2 method="textDocument/diagnostic"}: Included path via `include`: /tmp/mvce/dir_0/test_0.py
DEBUG request{id=3 method="shutdown"}: enter
DEBUG request{id=3 method="shutdown"}: Received shutdown request, waiting for shutdown notification
DEBUG Received exit notification, exiting
 INFO Server shut down
```
2. `nvim dir_0/test_0.py`
```
 INFO No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /tmp/mvce
 INFO No workspace options found for file:///tmp/mvce, using default options
DEBUG Negotiated position encoding: UTF8
DEBUG Indexing settings for workspace: /tmp/mvce
 INFO Registering workspace: /tmp/mvce
 WARN LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.
DEBUG notification{method="textDocument/didOpen"}: enter
DEBUG notification{method="textDocument/didOpen"}: *** file:///tmp/mvce/dir_0/test_0.py is in index
DEBUG notification{method="textDocument/didOpen"}: **** contains key: false
DEBUG notification{method="textDocument/didOpen"}: *** has settings: false
DEBUG @@@ using fallback for "/tmp/mvce/dir_0/test_0.py"
DEBUG request{id=2 method="textDocument/diagnostic"}: enter
DEBUG request{id=2 method="textDocument/diagnostic"}: Included path via `include`: /tmp/mvce/dir_0/test_0.py
DEBUG request{id=3 method="shutdown"}: enter
DEBUG request{id=3 method="shutdown"}: Received shutdown request, waiting for shutdown notification
DEBUG Received exit notification, exiting
 INFO Server shut down
```
3. `cd dir_0; nvim test_0.py` then `:edit dir_0_1/test_0_1.py`
```
 INFO No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /tmp/mvce/dir_0
 INFO No workspace options found for file:///tmp/mvce/dir_0, using default options
DEBUG Negotiated position encoding: UTF8
DEBUG Indexing settings for workspace: /tmp/mvce/dir_0
DEBUG No `target-version` found in `ruff.toml`
DEBUG No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified
DEBUG Loaded settings from: `/tmp/mvce/dir_0/ruff.toml`
 INFO Registering workspace: /tmp/mvce/dir_0
 WARN LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.
DEBUG notification{method="textDocument/didOpen"}: enter
DEBUG notification{method="textDocument/didOpen"}: *** file:///tmp/mvce/dir_0/test_0.py is in index
DEBUG notification{method="textDocument/didOpen"}: **** contains key: true
DEBUG notification{method="textDocument/didOpen"}: *** has settings: true
DEBUG request{id=2 method="textDocument/diagnostic"}: enter
DEBUG request{id=2 method="textDocument/diagnostic"}: Included path via `include`: /tmp/mvce/dir_0/test_0.py
DEBUG notification{method="textDocument/didOpen"}: enter
DEBUG notification{method="textDocument/didOpen"}: *** file:///tmp/mvce/dir_0/dir_0_1/test_0_1.py is in index
DEBUG notification{method="textDocument/didOpen"}: **** contains key: false
DEBUG notification{method="textDocument/didOpen"}: *** has settings: true
DEBUG request{id=3 method="textDocument/diagnostic"}: enter
DEBUG request{id=3 method="textDocument/diagnostic"}: Included path via `include`: /tmp/mvce/dir_0/dir_0_1/test_0_1.py
DEBUG request{id=4 method="shutdown"}: enter
DEBUG request{id=4 method="shutdown"}: Received shutdown request, waiting for shutdown notification
DEBUG Received exit notification, exiting
 INFO Server shut down
```
4. `cd dir_0; nvim test_0.py` then `:edit ../dir_1/test_1.py`
```
 INFO No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /tmp/mvce/dir_0
 INFO No workspace options found for file:///tmp/mvce/dir_0, using default options
DEBUG Negotiated position encoding: UTF8
DEBUG Indexing settings for workspace: /tmp/mvce/dir_0
DEBUG No `target-version` found in `ruff.toml`
DEBUG No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified
DEBUG Loaded settings from: `/tmp/mvce/dir_0/ruff.toml`
 INFO Registering workspace: /tmp/mvce/dir_0
 WARN LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.
DEBUG notification{method="textDocument/didOpen"}: enter
DEBUG notification{method="textDocument/didOpen"}: *** file:///tmp/mvce/dir_0/test_0.py is in index
DEBUG notification{method="textDocument/didOpen"}: **** contains key: true
DEBUG notification{method="textDocument/didOpen"}: *** has settings: true
DEBUG request{id=2 method="textDocument/diagnostic"}: enter
DEBUG request{id=2 method="textDocument/diagnostic"}: Included path via `include`: /tmp/mvce/dir_0/test_0.py
DEBUG notification{method="textDocument/didOpen"}: enter
DEBUG notification{method="textDocument/didOpen"}: *** file:///tmp/mvce/dir_1/test_1.py is in index
DEBUG notification{method="textDocument/didOpen"}: **** contains key: false
DEBUG notification{method="textDocument/didOpen"}: *** has settings: false
DEBUG @@@ using fallback for "/tmp/mvce/dir_1/test_1.py"
DEBUG request{id=4 method="textDocument/diagnostic"}: enter
DEBUG request{id=4 method="textDocument/diagnostic"}: Included path via `include`: /tmp/mvce/dir_1/test_1.py
DEBUG request{id=5 method="shutdown"}: enter
DEBUG request{id=5 method="shutdown"}: Received shutdown request, waiting for shutdown notification
DEBUG Received exit notification, exiting
 INFO Server shut down
```

- `**** contains key:` is `self.settings.contains_key(url.to_file_path()?.parent()?)`
- `@@@ using fallback` is in the `.unwrap_or_else` part of `RuffSettingsIndex::get`, during `Index::make_document_ref`

</details>

---

One possible way to fix issues 2. and 4. above is, during `session::Index::open_text_document`, to check if `self.settings_for_url().ruff_settings` has our file's parent directory, and if not, to call `self.open_workspace_folder`.

This seems to work, without negatively affecting editors like VS Code that open folders. I don't know if it would have other consequences. It's also not a fix for #20847. Should I open a PR for it?


---

_Comment by @ntBre on 2026-01-13 19:30_

Thanks for looking into this! @MichaReiser or @dhruvmanila would know better than me if this sounds like a promising approach, but it probably wouldn't hurt to open a PR if you have something working locally. Then we could test it out too.

---

_Comment by @MichaReiser on 2026-01-14 09:31_

thanks for the detailed analysis


> One possible way to fix issues 2. and 4. above is, during session::Index::open_text_document, to check if self.settings_for_url().ruff_settings has our file's parent directory, and if not, to call self.open_workspace_folder.

I'm not convinced we should do this unless we have a very clear strategy for cleaning them up when the file is closed (or if the user adds that directory as a workspace folder). Given that all that's missing are the settings, I wonder if we could just try to discover the settings walking the file's ancestor directories and collect all configuration files instead.

---

_Comment by @ZedThree on 2026-01-14 10:33_

Sorry @MichaReiser, I didn't see your comment till after I made the PR! I tried the workspace approach first, because that seems to be the only way to cache the discovered settings.

I'll try your approach instead

---

_Comment by @dhruvmanila on 2026-01-16 10:00_

Sorry for the delay!

I'm not sure if this is going to be the case for Neovim mainly because Neovim will start a separate server for each of these directories because it has all of the possible configuration files for Ruff to [detect the project root](https://github.com/neovim/nvim-lspconfig/blob/92ee7d42320edfbb81f3cad851314ab197fa324a/lua/lspconfig/configs/ruff.lua#L14-L17) which it uses to detect if there's a common root. In this case, there isn't but if there was a config file in the root project, then it might not spawn a separate server for each of these directories:

```
- ruff (id: 2)
  - Version: 0.14.11
  - Root directory: ~/playground/ruff/issue-17944/project-b
  - Command: { "ruff", "server" }
  - Settings: {}
  - Attached buffers: 6

- ruff (id: 5)
  - Version: 0.14.11
  - Root directory: ~/playground/ruff/issue-17944/project-a
  - Command: { "ruff", "server" }
  - Settings: {}
  - Attached buffers: 13
```

> * cwd at top-level, opening `dir_0/test_0.py`
> * cwd at top-level, opening `dir_0/test_0.py`, then `../dir_1/test_1.py` in same session
> * cwd in `dir_0`, opening `../dir_1/test_1.py`
> * cwd in `dir_0`, opening `test_0.py` first, then `../dir_1/test_1.py` in same session

So, all of these cases (copied from the linked PR) works as expected at least in Neovim because it spawns two separate Ruff servers with different project root.

Although I do see the issue in VS Code.

I'm going to look at the PR now, but I wanted to understand the issue first.

---

_Comment by @dhruvmanila on 2026-01-16 10:16_

> 2\. `nvim dir_0/test_0.py` or `nvim dir_1/test_1.py` only gives one warning (bug)

In this case in Neovim, I'm actually seeing
- both warnings from Ruff for `nvim dir_0/test_0.py` which is as expected, doesn't look like a bug)
- seeing no warnings for `nvim dir_1/test_1.py` which is also as expected because the `dir_1/ruff.toml` doesn't have `F` selected, preview is disabled so `FURB101` will not be checked

> 3\. `cd dir_0; nvim test_0.py` then `:edit dir_0_1/test_0_1.py` gives two warnings (bug)   - this is because `dir_0/ruff.toml` is used instead of `dir_0_1/ruff.toml`

I'm seeing the expected behavior in this case as well for Neovim where the later addition of `dir_0_1/test_0_1.py` gives me zero warnings from Ruff as it spawns a different LSP for that file because it is considered to have a different root than the existing LSP.

> 4\. `cd dir_0; nvim test_0.py` then `:edit ../dir_1/test_1.py` gives one warning (bug)

Same here, I'm seeing expected behavior because of the way Neovim tries to find project root for the LSP.

---

_Comment by @ZedThree on 2026-01-16 10:38_

Ah, I hadn't set `root_markers` in my neovim config. That does indeed make things work correctly. It's not my usual editor, so I hadn't realised that was required -- could be useful to add this to the docs?

The issue does seem to also affect other editors like VS Code in single file mode, and helix.

---
