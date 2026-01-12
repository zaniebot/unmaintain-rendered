```yaml
number: 11506
title: "`ruff server` detects config file differently to `ruff check`"
type: issue
state: closed
author: JaRoSchm
labels:
  - server
assignees: []
created_at: 2024-05-23T07:49:43Z
updated_at: 2024-05-26T18:47:14Z
url: https://github.com/astral-sh/ruff/issues/11506
synced_at: 2026-01-12T15:54:51Z
```

# `ruff server` detects config file differently to `ruff check`

---

_@JaRoSchm_

Hi,

I just tried the new `ruff server` (ruff 0.4.5) and noticed that it detects its configuration file differently to `ruff check`. I have a repo with a `pyproject.toml` containing no `ruff` configuration. In the directory above there is a `pyproject.toml` containing configuration for `ruff` (in this case enabling additional rules). If I use `ruff check` from inside the repo, it works as expected and uses the enabled rules. Using `ruff server` inside of vim using the plugin vim-lsp doesn't enable the additional rules. If I add the rules to the `pyproject.toml` inside of the repo, they are detected by `ruff server` too.

From the documentation of `ruff server` it is not clear to me, if this is expected behaviour. As a user of `ruff check` and `ruff server` I would expect them to behave equivalently.


---

_Comment by @JaRoSchm on 2024-05-23 08:21_

And here a minimal example:

pyproject.toml:
```toml
[tool.ruff.lint]
select = ["NPY"]
```

repo/pyproject.toml: empty

repo/file.py (should raise NPY002):
```py
import numpy as np

np.random.uniform(100)
```

Relevant config in .vimrc:
```vim
if executable('ruff')
    au User lsp_setup call lsp#register_server({
        \ 'name': 'ruff-server',
        \ 'cmd': {server_info->['ruff', 'server', '--preview']},
        \ 'allowlist': ['python'],
        \ 'workspace_config': {'configurationPreference': 'filesystemFirst'},
        \ })
endif
```

Log of LSP client:
<details>
  <summary>Log</summary>

Do 23 Mai 2024 10:14:57 CEST:["lsp#register_server","server registered","ruff-server"]
Do 23 Mai 2024 10:14:57 CEST:["lsp#register_server","server registered","clangd"]
Do 23 Mai 2024 10:14:57 CEST:["lsp#register_server","server registered","texlab"]
Do 23 Mai 2024 10:14:57 CEST:["s:on_text_document_did_open()",1,"python","/home/jschmidt/tmp/repo","file:///home/jschmidt/tmp/repo/file.py"]
Do 23 Mai 2024 10:14:57 CEST:["Starting server","ruff-server",["ruff","server","--preview"]]
Do 23 Mai 2024 10:14:57 CEST:[{"response":{"data":{"__data__":"vim-lsp","lsp_id":1,"server_name":"ruff-server"},"message":"started lsp server successfully"}}]
Do 23 Mai 2024 10:14:57 CEST:[{"response":{"error":{"data":{"lsp_id":1,"__error__":"vim-lsp","server_name":"ruff-server"},"code":0,"message":"ignore initialization lsp server due to empty root_uri"}}}]
Do 23 Mai 2024 10:14:57 CEST:["--->",1,"ruff-server",{"method":"initialize","params":{"rootUri":"file:///home/jschmidt/tmp/repo","capabilities":{"workspace":{"workspaceFolders":false,"configuration":true,"symbol":{"dynamicRegistration":false},"applyEdit":true},"window":{"workDoneProgress":false},"textDocument":{"callHierarchy":{"dynamicRegistration":false},"rename":{"prepareSupport":true,"dynamicRegistration":false,"prepareSupportDefaultBehavior":1},"codeAction":{"isPreferredSupport":true,"disabledSupport":true,"codeActionLiteralSupport":{"codeActionKind":{"valueSet":["","quickfix","refactor","refactor.extract","refactor.inline","refactor.rewrite","source","source.organizeImports"]}},"dynamicRegistration":false},"completion":{"completionItem":{"snippetSupport":false,"resolveSupport":{"properties":["additionalTextEdits"]},"documentationFormat":["markdown","plaintext"]},"dynamicRegistration":false,"completionItemKind":{"valueSet":[10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,1,2,3,4,5,6,7,8,9]}},"formatting":{"dynamicRegistration":false},"codeLens":{"dynamicRegistration":false},"inlayHint":{"dynamicRegistration":false},"hover":{"dynamicRegistration":false,"contentFormat":["markdown","plaintext"]},"rangeFormatting":{"dynamicRegistration":false},"declaration":{"dynamicRegistration":false,"linkSupport":true},"references":{"dynamicRegistration":false},"typeHierarchy":{"dynamicRegistration":false},"foldingRange":{"rangeLimit":5000,"dynamicRegistration":false,"lineFoldingOnly":true},"documentSymbol":{"symbolKind":{"valueSet":[10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,1,2,3,4,5,6,7,8,9]},"dynamicRegistration":false,"labelSupport":false,"hierarchicalDocumentSymbolSupport":false},"publishDiagnostics":{"relatedInformation":true},"synchronization":{"dynamicRegistration":false,"willSaveWaitUntil":false,"willSave":false,"didSave":true},"documentHighlight":{"dynamicRegistration":false},"implementation":{"dynamicRegistration":false,"linkSupport":true},"typeDefinition":{"dynamicRegistration":false,"linkSupport":true},"semanticTokens":{"serverCancelSupport":false,"requests":{"full":false,"range":false},"multilineTokenSupport":false,"dynamicRegistration":false,"overlappingTokenSupport":false,"tokenTypes":["type","class","enum","interface","struct","typeParameter","parameter","variable","property","enumMember","event","function","method","macro","keyword","modifier","comment","string","number","regexp","operator"],"tokenModifiers":[],"formats":["relative"]},"signatureHelp":{"dynamicRegistration":false},"definition":{"dynamicRegistration":false,"linkSupport":true}}},"rootPath":"/home/jschmidt/tmp/repo","clientInfo":{"name":"vim-lsp"},"processId":30822,"trace":"off"}}]
Do 23 Mai 2024 10:14:57 CEST:["<---",1,"ruff-server",{"response":{"id":1,"jsonrpc":"2.0","result":{"capabilities":{"diagnosticProvider":{"identifier":"Ruff","interFileDependencies":false,"workDoneProgress":true,"workspaceDiagnostics":false},"hoverProvider":true,"documentFormattingProvider":true,"positionEncoding":"utf-16","workspace":{"workspaceFolders":{"changeNotifications":true,"supported":true}},"codeActionProvider":{"resolveProvider":true,"workDoneProgress":true,"codeActionKinds":["quickfix","source.fixAll.ruff","source.organizeImports.ruff","notebook.source.fixAll.ruff","notebook.source.organizeImports.ruff"]},"notebookDocumentSync":{"save":false,"notebookSelector":[{"cells":[{"language":"python"}]}]},"documentRangeFormattingProvider":true,"textDocumentSync":{"willSaveWaitUntil":false,"willSave":false,"change":2,"openClose":true}},"serverInfo":{"version":"0.4.5","name":"ruff"}}},"request":{"id":1,"jsonrpc":"2.0","method":"initialize","params":{"rootUri":"file:///home/jschmidt/tmp/repo","capabilities":{"workspace":{"workspaceFolders":false,"configuration":true,"symbol":{"dynamicRegistration":false},"applyEdit":true},"window":{"workDoneProgress":false},"textDocument":{"callHierarchy":{"dynamicRegistration":false},"rename":{"prepareSupport":true,"dynamicRegistration":false,"prepareSupportDefaultBehavior":1},"codeAction":{"isPreferredSupport":true,"disabledSupport":true,"codeActionLiteralSupport":{"codeActionKind":{"valueSet":["","quickfix","refactor","refactor.extract","refactor.inline","refactor.rewrite","source","source.organizeImports"]}},"dynamicRegistration":false},"completion":{"completionItem":{"snippetSupport":false,"resolveSupport":{"properties":["additionalTextEdits"]},"documentationFormat":["markdown","plaintext"]},"dynamicRegistration":false,"completionItemKind":{"valueSet":[10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,1,2,3,4,5,6,7,8,9]}},"formatting":{"dynamicRegistration":false},"codeLens":{"dynamicRegistration":false},"inlayHint":{"dynamicRegistration":false},"hover":{"dynamicRegistration":false,"contentFormat":["markdown","plaintext"]},"rangeFormatting":{"dynamicRegistration":false},"declaration":{"dynamicRegistration":false,"linkSupport":true},"references":{"dynamicRegistration":false},"typeHierarchy":{"dynamicRegistration":false},"foldingRange":{"rangeLimit":5000,"dynamicRegistration":false,"lineFoldingOnly":true},"documentSymbol":{"symbolKind":{"valueSet":[10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,1,2,3,4,5,6,7,8,9]},"dynamicRegistration":false,"labelSupport":false,"hierarchicalDocumentSymbolSupport":false},"publishDiagnostics":{"relatedInformation":true},"synchronization":{"dynamicRegistration":false,"willSaveWaitUntil":false,"willSave":false,"didSave":true},"documentHighlight":{"dynamicRegistration":false},"implementation":{"dynamicRegistration":false,"linkSupport":true},"typeDefinition":{"dynamicRegistration":false,"linkSupport":true},"semanticTokens":{"serverCancelSupport":false,"requests":{"full":false,"range":false},"multilineTokenSupport":false,"dynamicRegistration":false,"overlappingTokenSupport":false,"tokenTypes":["type","class","enum","interface","struct","typeParameter","parameter","variable","property","enumMember","event","function","method","macro","keyword","modifier","comment","string","number","regexp","operator"],"tokenModifiers":[],"formats":["relative"]},"signatureHelp":{"dynamicRegistration":false},"definition":{"dynamicRegistration":false,"linkSupport":true}}},"rootPath":"/home/jschmidt/tmp/repo","clientInfo":{"name":"vim-lsp"},"processId":30822,"trace":"off"}}}]
Do 23 Mai 2024 10:14:57 CEST:["--->",1,"ruff-server",{"method":"initialized","params":{}}]
Do 23 Mai 2024 10:14:57 CEST:["--->",1,"ruff-server",{"method":"workspace/didChangeConfiguration","params":{"settings":{"configurationPreference":"filesystemFirst"}}}]
Do 23 Mai 2024 10:14:57 CEST:[{"response":{"data":{"__data__":"vim-lsp","server_name":"ruff-server"},"message":"configuration sent"}}]
Do 23 Mai 2024 10:14:57 CEST:["s:update_file_content()",1]
Do 23 Mai 2024 10:14:57 CEST:["--->",1,"ruff-server",{"method":"textDocument/didOpen","params":{"textDocument":{"uri":"file:///home/jschmidt/tmp/repo/file.py","version":1,"languageId":"python","text":"import numpy as np\n\nnp.random.uniform(100)\n"}}}]
Do 23 Mai 2024 10:14:57 CEST:[{"response":{"data":{"path":"file:///home/jschmidt/tmp/repo/file.py","__data__":"vim-lsp","filetype":"python","server_name":"ruff-server"},"message":"textDocument/open sent"}}]
Do 23 Mai 2024 10:14:57 CEST:[{"response":{"data":{"path":"file:///home/jschmidt/tmp/repo/file.py","__data__":"vim-lsp","server_name":"ruff-server"},"message":"not dirty"}}]
Do 23 Mai 2024 10:14:57 CEST:["s:on_stdout client option on_notification() error","Vim(let):E716: Key not present in Dictionary: \"pylsp\"","function <SNR>156_out_cb[1]..<SNR>155_on_stdout[67]..<SNR>126_on_notification[16]..<SNR>126_handle_initialize[25]..<SNR>154_callback[1]..<SNR>154_next[9]..<lambda>45[1]..<SNR>126_ensure_conf[14]..<SNR>154_callback[1]..<SNR>154_next[9]..<lambda>46[1]..<SNR>126_ensure_open[40]..<SNR>154_callback[1]..<SNR>154_next[9]..<lambda>47[1]..<SNR>126_ensure_changed[18]..<SNR>154_callback[1]..<SNR>154_next[9]..<lambda>48[1]..<SNR>126_fire_lsp_buffer_enabled[2]..User Autocommands for \"lsp_buffer_enabled\"..function <SNR>9_on_lsp_buffer_enabled[1]..lsp#get_server_capabilities, line 1"]
Do 23 Mai 2024 10:14:57 CEST:["<---",1,"ruff-server",{"response":{"method":"textDocument/publishDiagnostics","jsonrpc":"2.0","params":{"diagnostics":[],"uri":"file:///home/jschmidt/tmp/repo/file.py","version":1}}}]
Do 23 Mai 2024 10:14:57 CEST:["<---(stderr)",1,"ruff-server","   0.019681s WARN ruff_server::server No workspace(s) were provided during initialization. Using the current working directory as a default workspace...\n   0.024632s WARN ruff_server::server LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.\n"]
Do 23 Mai 2024 10:14:58 CEST:[{"response":{"data":{"__data__":"vim-lsp","server_name":"ruff-server"},"message":"server already started"}}]
Do 23 Mai 2024 10:14:58 CEST:[{"response":{"data":{"__data__":"vim-lsp","init_result":{"id":1,"jsonrpc":"2.0","result":{"capabilities":{"diagnosticProvider":{"identifier":"Ruff","interFileDependencies":false,"workDoneProgress":true,"workspaceDiagnostics":false},"hoverProvider":true,"documentFormattingProvider":true,"positionEncoding":"utf-16","workspace":{"workspaceFolders":{"changeNotifications":true,"supported":true}},"codeActionProvider":{"resolveProvider":true,"workDoneProgress":true,"codeActionKinds":["quickfix","source.fixAll.ruff","source.organizeImports.ruff","notebook.source.fixAll.ruff","notebook.source.organizeImports.ruff"]},"notebookDocumentSync":{"save":false,"notebookSelector":[{"cells":[{"language":"python"}]}]},"documentRangeFormattingProvider":true,"textDocumentSync":{"willSaveWaitUntil":false,"willSave":false,"change":2,"openClose":true}},"serverInfo":{"version":"0.4.5","name":"ruff"}}},"server_name":"ruff-server"},"message":"lsp server already initialized"}}]
Do 23 Mai 2024 10:14:58 CEST:[{"response":{"data":{"__data__":"vim-lsp","server_name":"ruff-server"},"message":"configuration sent"}}]
Do 23 Mai 2024 10:14:58 CEST:[{"response":{"data":{"path":"file:///home/jschmidt/tmp/repo/file.py","__data__":"vim-lsp","server_name":"ruff-server"},"message":"already opened"}}]
Do 23 Mai 2024 10:14:58 CEST:[{"response":{"data":{"path":"file:///home/jschmidt/tmp/repo/file.py","__data__":"vim-lsp","server_name":"ruff-server"},"message":"not dirty"}}]
Do 23 Mai 2024 10:14:58 CEST:["--->",1,"ruff-server",{"method":"textDocument/codeAction","on_notification":"---funcref---","params":{"context":{"diagnostics":[],"only":["","quickfix","refactor","refactor.extract","refactor.inline","refactor.rewrite"]},"range":{"end":{"character":18,"line":0},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///home/jschmidt/tmp/repo/file.py"}}}]
Do 23 Mai 2024 10:14:58 CEST:["<---",1,"ruff-server",{"response":{"id":2,"jsonrpc":"2.0","result":[{"edit":{"changes":{}},"kind":"source.fixAll.ruff","title":"Ruff: Fix all auto-fixable problems"},{"edit":{"changes":{}},"kind":"source.organizeImports.ruff","title":"Ruff: Organize imports"},{"edit":{"changes":{}},"kind":"notebook.source.fixAll.ruff","title":"Ruff: Fix all auto-fixable problems"},{"edit":{"changes":{}},"kind":"notebook.source.organizeImports.ruff","title":"Ruff: Organize imports"}]},"request":{"id":2,"jsonrpc":"2.0","method":"textDocument/codeAction","params":{"context":{"diagnostics":[],"only":["","quickfix","refactor","refactor.extract","refactor.inline","refactor.rewrite"]},"range":{"end":{"character":18,"line":0},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///home/jschmidt/tmp/repo/file.py"}}}}]
Do 23 Mai 2024 10:15:29 CEST:["s:on_text_document_did_close()",1]
</details>

---

_Assigned to @snowsignal by @snowsignal on 2024-05-24 06:51_

---

_Label `server` added by @snowsignal on 2024-05-24 06:51_

---

_Comment by @snowsignal on 2024-05-24 06:56_

Thank you for opening this issue. I was able to reproduce this locally in Helix. I'll look into it 👍 

---

_Closed by @charliermarsh on 2024-05-26 18:11_

---

_Comment by @JaRoSchm on 2024-05-26 18:47_

Thank you for the fast fix!

---
