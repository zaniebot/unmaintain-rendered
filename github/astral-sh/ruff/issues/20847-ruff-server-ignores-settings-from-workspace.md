---
number: 20847
title: "`ruff server`: Ignores settings from workspace folder if the folder is excluded by a configuration in an ancestor directory"
type: issue
state: open
author: lewis6991
labels:
  - bug
  - server
assignees: []
created_at: 2025-10-13T15:49:06Z
updated_at: 2025-10-21T10:05:14Z
url: https://github.com/astral-sh/ruff/issues/20847
synced_at: 2026-01-07T13:12:16-06:00
---

# `ruff server`: Ignores settings from workspace folder if the folder is excluded by a configuration in an ancestor directory

---

_Issue opened by @lewis6991 on 2025-10-13 15:49_

### Summary

I have a project with this structure:
```
├── pyproject.toml
└── sub/
    ├── main.py
    └── pyproject.toml
```

`ruff server` seems to always use the `pyproject.toml` defined in the root, even if `ruff server` is called with `cwd` set to `sub`. This is inconsistent with `ruff check`.

Reproduction steps:

```
mkdir ruff_issue
cd ruff_issue
git init
echo '[tool.ruff]\nexclude=["sub"]' > pyproject.toml
mkdir sub
touch sub/pyproject.toml
echo 'print "Hello world!"' > sub/main.py

git add \
    pyproject.toml
    sub/pyproject.toml \
    sub/main.py

git commit -m "Initial commit"

ruff check # uses root pyproject.toml, reports no issues, expected
# ruff server with cwd set to root, reports no issues, expected

cd sub

ruff check # uses sub/pyproject.toml, reports issues, expected
# ruff server with cwd set to sub, reports no issues, NOT EXPECTED
```

I don't know how to get diagnostics from `ruff server` without using an editor. I'm currently using Neovim with the default config defined in https://github.com/neovim/nvim-lspconfig which simply calls `ruff server` in the directory of the closest `pyproject.toml` in a bottom-up search.

### Version

ruff 0.14.0

---

_Comment by @MichaReiser on 2025-10-13 17:43_

Can you try setting [`logLevel](https://docs.astral.sh/ruff/editors/settings/#loglevel) to debug and share the logs. It might help us understand what's going on here.

---

_Label `needs-info` added by @MichaReiser on 2025-10-13 17:43_

---

_Label `server` added by @MichaReiser on 2025-10-13 17:43_

---

_Comment by @lewis6991 on 2025-10-14 08:07_

I added the following to my Neovim configuration:

```lua
vim.lsp.log.set_level('debug')

lsp.config('ruff', {
  cmd = { 'ruff', 'server', '-v' },
  settings = {
    logLevel = 'debug',
  },
})
```

And the log produced after opening `sub/main.py` is:

```
[START][2025-10-14 09:04:17] LSP logging initiated
[INFO][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"Starting RPC client"	{ cmd = { "ruff", "server", "-v" }, extra = {} }
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 1, jsonrpc = "2.0", method = "initialize", params = { capabilities = { general = { positionEncodings = { "utf-8", "utf-16", "utf-32" } }, textDocument = { callHierarchy = { dynamicRegistration = false }, codeAction = { codeActionLiteralSupport = { codeActionKind = { valueSet = { "", "quickfix", "refactor", "refactor.extract", "refactor.inline", "refactor.rewrite", "source", "source.organizeImports" } } }, dataSupport = true, disabledSupport = true, dynamicRegistration = true, honorsChangeAnnotations = true, isPreferredSupport = true, resolveSupport = { properties = { "edit", "command" } } }, codeLens = { dynamicRegistration = false, resolveSupport = { properties = { "command" } } }, colorProvider = { dynamicRegistration = true }, completion = { completionItem = { commitCharactersSupport = false, deprecatedSupport = true, documentationFormat = { "markdown", "plaintext" }, preselectSupport = false, resolveSupport = { properties = { "additionalTextEdits", "command" } }, snippetSupport = true, tagSupport = { valueSet = { 1 } } }, completionItemKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25 } }, completionList = { itemDefaults = { "editRange", "insertTextFormat", "insertTextMode", "data" } }, contextSupport = true, dynamicRegistration = false }, declaration = { linkSupport = true }, definition = { dynamicRegistration = true, linkSupport = true }, diagnostic = { dataSupport = true, dynamicRegistration = false, relatedDocumentSupport = true, relatedInformation = true, tagSupport = { valueSet = { 1, 2 } } }, documentHighlight = { dynamicRegistration = false }, documentSymbol = { dynamicRegistration = false, hierarchicalDocumentSymbolSupport = true, symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } }, tagSupport = { valueSet = { 1 } } }, foldingRange = { dynamicRegistration = false, foldingRange = { collapsedText = true }, foldingRangeKind = { valueSet = { "comment", "imports", "region" } }, lineFoldingOnly = true }, formatting = { dynamicRegistration = true }, hover = { contentFormat = { "markdown", "plaintext" }, dynamicRegistration = true }, implementation = { linkSupport = true }, inlayHint = { dynamicRegistration = true, resolveSupport = { properties = { "textEdits", "tooltip", "location", "command" } } }, inlineCompletion = { dynamicRegistration = false }, linkedEditingRange = { dynamicRegistration = false }, onTypeFormatting = { dynamicRegistration = false }, publishDiagnostics = { dataSupport = true, relatedInformation = true, tagSupport = { valueSet = { 1, 2 } } }, rangeFormatting = { dynamicRegistration = true, rangesSupport = true }, references = { dynamicRegistration = false }, rename = { dynamicRegistration = true, honorsChangeAnnotations = true, prepareSupport = true }, selectionRange = { dynamicRegistration = false }, semanticTokens = { augmentsSyntaxTokens = true, dynamicRegistration = false, formats = { "relative" }, multilineTokenSupport = true, overlappingTokenSupport = true, requests = { full = { delta = true }, range = false }, serverCancelSupport = false, tokenModifiers = { "declaration", "definition", "readonly", "static", "deprecated", "abstract", "async", "modification", "documentation", "defaultLibrary" }, tokenTypes = { "namespace", "type", "class", "enum", "interface", "struct", "typeParameter", "parameter", "variable", "property", "enumMember", "event", "function", "method", "macro", "keyword", "modifier", "comment", "string", "number", "regexp", "operator", "decorator" } }, signatureHelp = { dynamicRegistration = false, signatureInformation = { activeParameterSupport = true, documentationFormat = { "markdown", "plaintext" }, noActiveParameterSupport = true, parameterInformation = { labelOffsetSupport = true } } }, synchronization = { didSave = true, dynamicRegistration = false, willSave = true, willSaveWaitUntil = true }, typeDefinition = { linkSupport = true } }, window = { showDocument = { support = true }, showMessage = { messageActionItem = { additionalPropertiesSupport = true } }, workDoneProgress = true }, workspace = { applyEdit = true, configuration = true, diagnostics = { refreshSupport = false }, didChangeConfiguration = { dynamicRegistration = false }, didChangeWatchedFiles = { dynamicRegistration = true, relativePatternSupport = true }, inlayHint = { refreshSupport = true }, semanticTokens = { refreshSupport = true }, symbol = { dynamicRegistration = false, symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } } }, workspaceEdit = { changeAnnotationSupport = { groupsOnLabel = true }, normalizesLineEndings = true, resourceOperations = { "rename", "create", "delete" } }, workspaceFolders = true } }, clientInfo = { name = "Neovim", version = "0.12.0-dev+g198c9e9bc" }, processId = 16096, rootPath = "/Users/lewrus01/projects/ruff_issue/sub", rootUri = "file:///Users/lewrus01/projects/ruff_issue/sub", trace = "off", workDoneToken = "1", workspaceFolders = { { name = "/Users/lewrus01/projects/ruff_issue/sub", uri = "file:///Users/lewrus01/projects/ruff_issue/sub" } } } }
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 1, jsonrpc = "2.0", result = { capabilities = { codeActionProvider = { codeActionKinds = { "quickfix", "source.fixAll.ruff", "source.organizeImports.ruff", "notebook.source.fixAll.ruff", "notebook.source.organizeImports.ruff" }, resolveProvider = true, workDoneProgress = true }, diagnosticProvider = { identifier = "Ruff", interFileDependencies = false, workDoneProgress = true, workspaceDiagnostics = false }, documentFormattingProvider = true, documentRangeFormattingProvider = true, executeCommandProvider = { commands = { "ruff.applyFormat", "ruff.applyAutofix", "ruff.applyOrganizeImports", "ruff.printDebugInformation" }, workDoneProgress = false }, hoverProvider = true, notebookDocumentSync = { notebookSelector = { { cells = { { language = "python" } } } }, save = false }, positionEncoding = "utf-8", textDocumentSync = { change = 2, openClose = true, willSave = false, willSaveWaitUntil = false }, workspace = { workspaceFolders = { changeNotifications = true, supported = true } } }, serverInfo = { name = "ruff", version = "0.14.0" } } }
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ jsonrpc = "2.0", method = "initialized", params = vim.empty_dict() }
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ jsonrpc = "2.0", method = "workspace/didChangeConfiguration", params = { settings = { logLevel = "debug" } } }
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ jsonrpc = "2.0", method = "textDocument/didOpen", params = { textDocument = { languageId = "python", text = 'print "Hello world!"\n', uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py", version = 0 } } }
[INFO][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ruff]"	"server_capabilities"	{ server_capabilities = { codeActionProvider = { codeActionKinds = { "quickfix", "source.fixAll.ruff", "source.organizeImports.ruff", "notebook.source.fixAll.ruff", "notebook.source.organizeImports.ruff" }, resolveProvider = true, workDoneProgress = true }, diagnosticProvider = { identifier = "Ruff", interFileDependencies = false, workDoneProgress = true, workspaceDiagnostics = false }, documentFormattingProvider = true, documentRangeFormattingProvider = true, executeCommandProvider = { commands = { "ruff.applyFormat", "ruff.applyAutofix", "ruff.applyOrganizeImports", "ruff.printDebugInformation" }, workDoneProgress = false }, hoverProvider = true, notebookDocumentSync = { notebookSelector = { { cells = { { language = "python" } } } }, save = false }, positionEncoding = "utf-8", textDocumentSync = { change = 2, openClose = true, willSave = false, willSaveWaitUntil = false }, workspace = { workspaceFolders = { changeNotifications = true, supported = true } } } }
[ERROR][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ruff"	"stderr"	"2025-10-14 09:04:17.483714000  INFO No workspace options found for file:///Users/lewrus01/projects/ruff_issue/sub, using default options\n2025-10-14 09:04:17.489175000  INFO Registering workspace: /Users/lewrus01/projects/ruff_issue/sub\n"
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 0, jsonrpc = "2.0", method = "client/registerCapability", params = { registrations = { { id = "ruff-server-watch", method = "workspace/didChangeWatchedFiles", registerOptions = { watchers = { { globPattern = "**/.ruff.toml" }, { globPattern = "**/ruff.toml" }, { globPattern = "**/pyproject.toml" } } } } } } }
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ruff]"	"client.request"	1	"textDocument/diagnostic"	{ textDocument = { uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py" } }	<function 1>	1
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 2, jsonrpc = "2.0", method = "textDocument/diagnostic", params = { textDocument = { uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py" } } }
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 2, jsonrpc = "2.0", result = { items = {}, kind = "full" } }
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request: callback result"	{ result = vim.NIL, status = true }
[DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 0, jsonrpc = "2.0", result = vim.NIL }
[ERROR][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ruff"	"stderr"	"2025-10-14 09:04:17.522461000  INFO Configuration file watcher successfully registered\n"
```

---

_Comment by @MichaReiser on 2025-10-14 08:44_

I used claude to pretty print the logs

```
 [START][2025-10-14 09:04:17] LSP logging initiated

  [INFO][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "Starting RPC client"
  {
    "cmd": ["ruff", "server", "-v"],
    "extra": {}
  }

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc.send"
  {
    "id": 1,
    "jsonrpc": "2.0",
    "method": "initialize",
    "params": {
      "capabilities": {
        "general": {
          "positionEncodings": ["utf-8", "utf-16", "utf-32"]
        },
        "textDocument": {
          "callHierarchy": {
            "dynamicRegistration": false
          },
          "codeAction": {
            "codeActionLiteralSupport": {
              "codeActionKind": {
                "valueSet": [
                  "",
                  "quickfix",
                  "refactor",
                  "refactor.extract",
                  "refactor.inline",
                  "refactor.rewrite",
                  "source",
                  "source.organizeImports"
                ]
              }
            },
            "dataSupport": true,
            "disabledSupport": true,
            "dynamicRegistration": true,
            "honorsChangeAnnotations": true,
            "isPreferredSupport": true,
            "resolveSupport": {
              "properties": ["edit", "command"]
            }
          },
          "codeLens": {
            "dynamicRegistration": false,
            "resolveSupport": {
              "properties": ["command"]
            }
          },
          "colorProvider": {
            "dynamicRegistration": true
          },
          "completion": {
            "completionItem": {
              "commitCharactersSupport": false,
              "deprecatedSupport": true,
              "documentationFormat": ["markdown", "plaintext"],
              "preselectSupport": false,
              "resolveSupport": {
                "properties": ["additionalTextEdits", "command"]
              },
              "snippetSupport": true,
              "tagSupport": {
                "valueSet": [1]
              }
            },
            "completionItemKind": {
              "valueSet": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25]
            },
            "completionList": {
              "itemDefaults": ["editRange", "insertTextFormat", "insertTextMode", "data"]
            },
            "contextSupport": true,
            "dynamicRegistration": false
          },
          "declaration": {
            "linkSupport": true
          },
          "definition": {
            "dynamicRegistration": true,
            "linkSupport": true
          },
          "diagnostic": {
            "dataSupport": true,
            "dynamicRegistration": false,
            "relatedDocumentSupport": true,
            "relatedInformation": true,
            "tagSupport": {
              "valueSet": [1, 2]
            }
          },
          "documentHighlight": {
            "dynamicRegistration": false
          },
          "documentSymbol": {
            "dynamicRegistration": false,
            "hierarchicalDocumentSymbolSupport": true,
            "symbolKind": {
              "valueSet": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26]
            },
            "tagSupport": {
              "valueSet": [1]
            }
          },
          "foldingRange": {
            "dynamicRegistration": false,
            "foldingRange": {
              "collapsedText": true
            },
            "foldingRangeKind": {
              "valueSet": ["comment", "imports", "region"]
            },
            "lineFoldingOnly": true
          },
          "formatting": {
            "dynamicRegistration": true
          },
          "hover": {
            "contentFormat": ["markdown", "plaintext"],
            "dynamicRegistration": true
          },
          "implementation": {
            "linkSupport": true
          },
          "inlayHint": {
            "dynamicRegistration": true,
            "resolveSupport": {
              "properties": ["textEdits", "tooltip", "location", "command"]
            }
          },
          "inlineCompletion": {
            "dynamicRegistration": false
          },
          "linkedEditingRange": {
            "dynamicRegistration": false
          },
          "onTypeFormatting": {
            "dynamicRegistration": false
          },
          "publishDiagnostics": {
            "dataSupport": true,
            "relatedInformation": true,
            "tagSupport": {
              "valueSet": [1, 2]
            }
          },
          "rangeFormatting": {
            "dynamicRegistration": true,
            "rangesSupport": true
          },
          "references": {
            "dynamicRegistration": false
          },
          "rename": {
            "dynamicRegistration": true,
            "honorsChangeAnnotations": true,
            "prepareSupport": true
          },
          "selectionRange": {
            "dynamicRegistration": false
          },
          "semanticTokens": {
            "augmentsSyntaxTokens": true,
            "dynamicRegistration": false,
            "formats": ["relative"],
            "multilineTokenSupport": true,
            "overlappingTokenSupport": true,
            "requests": {
              "full": {
                "delta": true
              },
              "range": false
            },
            "serverCancelSupport": false,
            "tokenModifiers": [
              "declaration",
              "definition",
              "readonly",
              "static",
              "deprecated",
              "abstract",
              "async",
              "modification",
              "documentation",
              "defaultLibrary"
            ],
            "tokenTypes": [
              "namespace",
              "type",
              "class",
              "enum",
              "interface",
              "struct",
              "typeParameter",
              "parameter",
              "variable",
              "property",
              "enumMember",
              "event",
              "function",
              "method",
              "macro",
              "keyword",
              "modifier",
              "comment",
              "string",
              "number",
              "regexp",
              "operator",
              "decorator"
            ]
          },
          "signatureHelp": {
            "dynamicRegistration": false,
            "signatureInformation": {
              "activeParameterSupport": true,
              "documentationFormat": ["markdown", "plaintext"],
              "noActiveParameterSupport": true,
              "parameterInformation": {
                "labelOffsetSupport": true
              }
            }
          },
          "synchronization": {
            "didSave": true,
            "dynamicRegistration": false,
            "willSave": true,
            "willSaveWaitUntil": true
          },
          "typeDefinition": {
            "linkSupport": true
          }
        },
        "window": {
          "showDocument": {
            "support": true
          },
          "showMessage": {
            "messageActionItem": {
              "additionalPropertiesSupport": true
            }
          },
          "workDoneProgress": true
        },
        "workspace": {
          "applyEdit": true,
          "configuration": true,
          "diagnostics": {
            "refreshSupport": false
          },
          "didChangeConfiguration": {
            "dynamicRegistration": false
          },
          "didChangeWatchedFiles": {
            "dynamicRegistration": true,
            "relativePatternSupport": true
          },
          "inlayHint": {
            "refreshSupport": true
          },
          "semanticTokens": {
            "refreshSupport": true
          },
          "symbol": {
            "dynamicRegistration": false,
            "symbolKind": {
              "valueSet": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26]
            }
          },
          "workspaceEdit": {
            "changeAnnotationSupport": {
              "groupsOnLabel": true
            },
            "normalizesLineEndings": true,
            "resourceOperations": ["rename", "create", "delete"]
          },
          "workspaceFolders": true
        }
      },
      "clientInfo": {
        "name": "Neovim",
        "version": "0.12.0-dev+g198c9e9bc"
      },
      "processId": 16096,
      "rootPath": "/Users/lewrus01/projects/ruff_issue/sub",
      "rootUri": "file:///Users/lewrus01/projects/ruff_issue/sub",
      "trace": "off",
      "workDoneToken": "1",
      "workspaceFolders": [
        {
          "name": "/Users/lewrus01/projects/ruff_issue/sub",
          "uri": "file:///Users/lewrus01/projects/ruff_issue/sub"
        }
      ]
    }
  }

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc.receive"
  {
    "id": 1,
    "jsonrpc": "2.0",
    "result": {
      "capabilities": {
        "codeActionProvider": {
          "codeActionKinds": [
            "quickfix",
            "source.fixAll.ruff",
            "source.organizeImports.ruff",
            "notebook.source.fixAll.ruff",
            "notebook.source.organizeImports.ruff"
          ],
          "resolveProvider": true,
          "workDoneProgress": true
        },
        "diagnosticProvider": {
          "identifier": "Ruff",
          "interFileDependencies": false,
          "workDoneProgress": true,
          "workspaceDiagnostics": false
        },
        "documentFormattingProvider": true,
        "documentRangeFormattingProvider": true,
        "executeCommandProvider": {
          "commands": [
            "ruff.applyFormat",
            "ruff.applyAutofix",
            "ruff.applyOrganizeImports",
            "ruff.printDebugInformation"
          ],
          "workDoneProgress": false
        },
        "hoverProvider": true,
        "notebookDocumentSync": {
          "notebookSelector": [
            {
              "cells": [
                {
                  "language": "python"
                }
              ]
            }
          ],
          "save": false
        },
        "positionEncoding": "utf-8",
        "textDocumentSync": {
          "change": 2,
          "openClose": true,
          "willSave": false,
          "willSaveWaitUntil": false
        },
        "workspace": {
          "workspaceFolders": {
            "changeNotifications": true,
            "supported": true
          }
        }
      },
      "serverInfo": {
        "name": "ruff",
        "version": "0.14.0"
      }
    }
  }

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc.send"
  {
    "jsonrpc": "2.0",
    "method": "initialized",
    "params": {}
  }

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc.send"
  {
    "jsonrpc": "2.0",
    "method": "workspace/didChangeConfiguration",
    "params": {
      "settings": {
        "logLevel": "debug"
      }
    }
  }

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc.send"
  {
    "jsonrpc": "2.0",
    "method": "textDocument/didOpen",
    "params": {
      "textDocument": {
        "languageId": "python",
        "text": "print \"Hello world!\"\n",
        "uri": "file:///Users/lewrus01/projects/ruff_issue/sub/main.py",
        "version": 0
      }
    }
  }

  [INFO][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "LSP[ruff]"    "server_capabilities"
  {
    "server_capabilities": {
      "codeActionProvider": {
        "codeActionKinds": [
          "quickfix",
          "source.fixAll.ruff",
          "source.organizeImports.ruff",
          "notebook.source.fixAll.ruff",
          "notebook.source.organizeImports.ruff"
        ],
        "resolveProvider": true,
        "workDoneProgress": true
      },
      "diagnosticProvider": {
        "identifier": "Ruff",
        "interFileDependencies": false,
        "workDoneProgress": true,
        "workspaceDiagnostics": false
      },
      "documentFormattingProvider": true,
      "documentRangeFormattingProvider": true,
      "executeCommandProvider": {
        "commands": [
          "ruff.applyFormat",
          "ruff.applyAutofix",
          "ruff.applyOrganizeImports",
          "ruff.printDebugInformation"
        ],
        "workDoneProgress": false
      },
      "hoverProvider": true,
      "notebookDocumentSync": {
        "notebookSelector": [
          {
            "cells": [
              {
                "language": "python"
              }
            ]
          }
        ],
        "save": false
      },
      "positionEncoding": "utf-8",
      "textDocumentSync": {
        "change": 2,
        "openClose": true,
        "willSave": false,
        "willSaveWaitUntil": false
      },
      "workspace": {
        "workspaceFolders": {
          "changeNotifications": true,
          "supported": true
        }
      }
    }
  }

  [ERROR][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc"    "ruff"    "stderr"
  "2025-10-14 09:04:17.483714000  INFO No workspace options found for file:///Users/lewrus01/projects/ruff_issue/sub, using default options
  2025-10-14 09:04:17.489175000  INFO Registering workspace: /Users/lewrus01/projects/ruff_issue/sub
  "

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc.receive"
  {
    "id": 0,
    "jsonrpc": "2.0",
    "method": "client/registerCapability",
    "params": {
      "registrations": [
        {
          "id": "ruff-server-watch",
          "method": "workspace/didChangeWatchedFiles",
          "registerOptions": {
            "watchers": [
              {
                "globPattern": "**/.ruff.toml"
              },
              {
                "globPattern": "**/ruff.toml"
              },
              {
                "globPattern": "**/pyproject.toml"
              }
            ]
          }
        }
      ]
    }
  }

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "LSP[ruff]"    "client.request"    1    "textDocument/diagnostic"
  {
    "textDocument": {
      "uri": "file:///Users/lewrus01/projects/ruff_issue/sub/main.py"
    }
  }
  <function 1>    1

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc.send"
  {
    "id": 2,
    "jsonrpc": "2.0",
    "method": "textDocument/diagnostic",
    "params": {
      "textDocument": {
        "uri": "file:///Users/lewrus01/projects/ruff_issue/sub/main.py"
      }
    }
  }

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc.receive"
  {
    "id": 2,
    "jsonrpc": "2.0",
    "result": {
      "items": [],
      "kind": "full"
    }
  }

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "server_request: callback result"
  {
    "result": null,
    "status": true
  }

  [DEBUG][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc.send"
  {
    "id": 0,
    "jsonrpc": "2.0",
    "result": null
  }

  [ERROR][2025-10-14 09:04:17] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151    "rpc"    "ruff"    "stderr"
  "2025-10-14 09:04:17.522461000  INFO Configuration file watcher successfully registered
```

---

_Comment by @MichaReiser on 2025-10-14 08:54_

Hmm, I don't think changing the log level worked because I don't see any `DEBUG` level logs (only info). 

I'm not using neovim myself, but I think you have to put `logLevel` into `init_options`:

```
{init_options}? (lsp.LSPObject) Values to pass in the initialization request as initializationOptions. See initialize in the LSP spec. 
```

---

_Comment by @lewis6991 on 2025-10-14 09:34_

I think you are right, I changed my config to:

```lua
lsp.config('ruff', {
  cmd = { 'ruff', 'server', '-v' },
  init_options = {
    logLevel = 'debug',
  }
})
```


```
[START][2025-10-14 10:29:47] LSP logging initiated
[INFO][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"Starting RPC client"
{
  cmd = { "ruff", "server", "-v" },
  extra = {}
}
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"
{
  id = 1,
  jsonrpc = "2.0",
  method = "initialize",
  params = {
    capabilities = {
      general = {
        positionEncodings = { "utf-8", "utf-16", "utf-32" }
      },
      textDocument = {
        callHierarchy = { dynamicRegistration = false },
        codeAction = {
          codeActionLiteralSupport = {
            codeActionKind = {
              valueSet = {
                "", "quickfix", "refactor", "refactor.extract", "refactor.inline", "refactor.rewrite", "source", "source.organizeImports"
              }
            }
          },
          dataSupport = true,
          disabledSupport = true,
          dynamicRegistration = true,
          honorsChangeAnnotations = true,
          isPreferredSupport = true,
          resolveSupport = { properties = { "edit", "command" } }
        },
        codeLens = {
          dynamicRegistration = false,
          resolveSupport = { properties = { "command" } }
        },
        colorProvider = { dynamicRegistration = true },
        completion = {
          completionItem = {
            commitCharactersSupport = false,
            deprecatedSupport = true,
            documentationFormat = { "markdown", "plaintext" },
            preselectSupport = false,
            resolveSupport = { properties = { "additionalTextEdits", "command" } },
            snippetSupport = true,
            tagSupport = { valueSet = { 1 } }
          },
          completionItemKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25 } },
          completionList = { itemDefaults = { "editRange", "insertTextFormat", "insertTextMode", "data" } },
          contextSupport = true,
          dynamicRegistration = false
        },
        declaration = { linkSupport = true },
        definition = { dynamicRegistration = true, linkSupport = true },
        diagnostic = {
          dataSupport = true,
          dynamicRegistration = false,
          relatedDocumentSupport = true,
          relatedInformation = true,
          tagSupport = { valueSet = { 1, 2 } }
        },
        documentHighlight = { dynamicRegistration = false },
        documentSymbol = {
          dynamicRegistration = false,
          hierarchicalDocumentSymbolSupport = true,
          symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } },
          tagSupport = { valueSet = { 1 } }
        },
        foldingRange = {
          dynamicRegistration = false,
          foldingRange = { collapsedText = true },
          foldingRangeKind = { valueSet = { "comment", "imports", "region" } },
          lineFoldingOnly = true
        },
        formatting = { dynamicRegistration = true },
        hover = { contentFormat = { "markdown", "plaintext" }, dynamicRegistration = true },
        implementation = { linkSupport = true },
        inlayHint = {
          dynamicRegistration = true,
          resolveSupport = { properties = { "textEdits", "tooltip", "location", "command" } }
        },
        inlineCompletion = { dynamicRegistration = false },
        linkedEditingRange = { dynamicRegistration = false },
        onTypeFormatting = { dynamicRegistration = false },
        publishDiagnostics = {
          dataSupport = true,
          relatedInformation = true,
          tagSupport = { valueSet = { 1, 2 } }
        },
        rangeFormatting = { dynamicRegistration = true, rangesSupport = true },
        references = { dynamicRegistration = false },
        rename = { dynamicRegistration = true, honorsChangeAnnotations = true, prepareSupport = true },
        selectionRange = { dynamicRegistration = false },
        semanticTokens = {
          augmentsSyntaxTokens = true,
          dynamicRegistration = false,
          formats = { "relative" },
          multilineTokenSupport = true,
          overlappingTokenSupport = true,
          requests = { full = { delta = true }, range = false },
          serverCancelSupport = false,
          tokenModifiers = {
            "declaration", "definition", "readonly", "static", "deprecated", "abstract", "async", "modification", "documentation", "defaultLibrary"
          },
          tokenTypes = {
            "namespace", "type", "class", "enum", "interface", "struct", "typeParameter", "parameter", "variable", "property", "enumMember", "event", "function", "method", "macro", "keyword", "modifier", "comment", "string", "number", "regexp", "operator", "decorator"
          }
        },
        signatureHelp = {
          dynamicRegistration = false,
          signatureInformation = {
            activeParameterSupport = true,
            documentationFormat = { "markdown", "plaintext" },
            noActiveParameterSupport = true,
            parameterInformation = { labelOffsetSupport = true }
          }
        },
        synchronization = {
          didSave = true,
          dynamicRegistration = false,
          willSave = true,
          willSaveWaitUntil = true
        },
        typeDefinition = { linkSupport = true }
      },
      window = {
        showDocument = { support = true },
        showMessage = { messageActionItem = { additionalPropertiesSupport = true } },
        workDoneProgress = true
      },
      workspace = {
        applyEdit = true,
        configuration = true,
        diagnostics = { refreshSupport = false },
        didChangeConfiguration = { dynamicRegistration = false },
        didChangeWatchedFiles = { dynamicRegistration = true, relativePatternSupport = true },
        inlayHint = { refreshSupport = true },
        semanticTokens = { refreshSupport = true },
        symbol = {
          dynamicRegistration = false,
          symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } }
        },
        workspaceEdit = {
          changeAnnotationSupport = { groupsOnLabel = true },
          normalizesLineEndings = true,
          resourceOperations = { "rename", "create", "delete" }
        },
        workspaceFolders = true
      }
    },
    clientInfo = {
      name = "Neovim",
      version = "0.12.0-dev+g198c9e9bc"
    },
    initializationOptions = { logLevel = "debug" },
    processId = 64637,
    rootPath = "/Users/lewrus01/projects/ruff_issue/sub",
    rootUri = "file:///Users/lewrus01/projects/ruff_issue/sub",
    trace = "off",
    workDoneToken = "1",
    workspaceFolders = {
      {
        name = "/Users/lewrus01/projects/ruff_issue/sub",
        uri = "file:///Users/lewrus01/projects/ruff_issue/sub"
      }
    }
  }
}
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"
{
  id = 1,
  jsonrpc = "2.0",
  result = {
    capabilities = {
      codeActionProvider = {
        codeActionKinds = {
          "quickfix",
          "source.fixAll.ruff",
          "source.organizeImports.ruff",
          "notebook.source.fixAll.ruff",
          "notebook.source.organizeImports.ruff"
        },
        resolveProvider = true,
        workDoneProgress = true
      },
      diagnosticProvider = {
        identifier = "Ruff",
        interFileDependencies = false,
        workDoneProgress = true,
        workspaceDiagnostics = false
      },
      documentFormattingProvider = true,
      documentRangeFormattingProvider = true,
      executeCommandProvider = {
        commands = {
          "ruff.applyFormat",
          "ruff.applyAutofix",
          "ruff.applyOrganizeImports",
          "ruff.printDebugInformation"
        },
        workDoneProgress = false
      },
      hoverProvider = true,
      notebookDocumentSync = {
        notebookSelector = {
          {
            cells = {
              { language = "python" }
            }
          }
        },
        save = false
      },
      positionEncoding = "utf-8",
      textDocumentSync = {
        change = 2,
        openClose = true,
        willSave = false,
        willSaveWaitUntil = false
      },
      workspace = {
        workspaceFolders = {
          changeNotifications = true,
          supported = true
        }
      }
    },
    serverInfo = {
      name = "ruff",
      version = "0.14.0"
    }
  }
}
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"
{
  jsonrpc = "2.0",
  method = "initialized",
  params = vim.empty_dict()
}
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"
{
  jsonrpc = "2.0",
  method = "textDocument/didOpen",
  params = {
    textDocument = {
      languageId = "python",
      text = 'print "Hello world!"\n',
      uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py",
      version = 0
    }
  }
}
[INFO][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ruff]"
"server_capabilities"
{
  server_capabilities = {
    codeActionProvider = {
      codeActionKinds = {
        "quickfix",
        "source.fixAll.ruff",
        "source.organizeImports.ruff",
        "notebook.source.fixAll.ruff",
        "notebook.source.organizeImports.ruff"
      },
      resolveProvider = true,
      workDoneProgress = true
    },
    diagnosticProvider = {
      identifier = "Ruff",
      interFileDependencies = false,
      workDoneProgress = true,
      workspaceDiagnostics = false
    },
    documentFormattingProvider = true,
    documentRangeFormattingProvider = true,
    executeCommandProvider = {
      commands = {
        "ruff.applyFormat",
        "ruff.applyAutofix",
        "ruff.applyOrganizeImports",
        "ruff.printDebugInformation"
      },
      workDoneProgress = false
    },
    hoverProvider = true,
    notebookDocumentSync = {
      notebookSelector = {
        {
          cells = {
            { language = "python" }
          }
        }
      },
      save = false
    },
    positionEncoding = "utf-8",
    textDocumentSync = {
      change = 2,
      openClose = true,
      willSave = false,
      willSaveWaitUntil = false
    },
    workspace = {
      workspaceFolders = {
        changeNotifications = true,
        supported = true
      }
    }
  }
}
[ERROR][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ruff"	"stderr"
"2025-10-14 10:29:47.297030000  INFO No workspace options found for file:///Users/lewrus01/projects/ruff_issue/sub, using default options\n2025-10-14 10:29:47.301137000  INFO Registering workspace: /Users/lewrus01/projects/ruff_issue/sub\n"
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"
{
  id = 0,
  jsonrpc = "2.0",
  method = "client/registerCapability",
  params = {
    registrations = {
      {
        id = "ruff-server-watch",
        method = "workspace/didChangeWatchedFiles",
        registerOptions = {
          watchers = {
            { globPattern = "**/.ruff.toml" },
            { globPattern = "**/ruff.toml" },
            { globPattern = "**/pyproject.toml" }
          }
        }
      }
    }
  }
}
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ruff]"
"client.request"
1
"textDocument/diagnostic"
{
  textDocument = {
    uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py"
  }
}
<function 1>
1
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"
{
  id = 2,
  jsonrpc = "2.0",
  method = "textDocument/diagnostic",
  params = {
    textDocument = {
      uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py"
    }
  }
}
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request: callback result"
{
  result = vim.NIL,
  status = true
}
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"
{
  id = 0,
  jsonrpc = "2.0",
  result = vim.NIL
}
[DEBUG][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"
{
  id = 2,
  jsonrpc = "2.0",
  result = {
    items = {},
    kind = "full"
  }
}
[ERROR][2025-10-14 10:29:47] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc"	"ruff"	"stderr"
"2025-10-14 10:29:47.353346000  INFO Configuration file watcher successfully registered\n"
[DEBUG][2025-10-14 10:29:50] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"
{
  jsonrpc = "2.0",
  method = "textDocument/didChange",
  params = {
    contentChanges = {
      {
        range = {
          ["end"] = { character = 1, line = 0 },
          start = { character = 0, line = 0 }
        },
        rangeLength = 1,
        text = ""
      }
    },
    textDocument = {
      uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py",
      version = 4
    }
  }
}
[DEBUG][2025-10-14 10:29:50] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ruff]"
"client.request"
1
"textDocument/diagnostic"
{
  textDocument = {
    uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py"
  }
}
<function 1>
1
[DEBUG][2025-10-14 10:29:50] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"
{
  id = 3,
  jsonrpc = "2.0",
  method = "textDocument/diagnostic",
  params = {
    textDocument = {
      uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py"
    }
  }
}
[DEBUG][2025-10-14 10:29:50] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"
{
  id = 3,
  jsonrpc = "2.0",
  result = {
    items = {},
    kind = "full"
  }
}
[DEBUG][2025-10-14 10:29:50] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"
{
  jsonrpc = "2.0",
  method = "textDocument/didChange",
  params = {
    contentChanges = {
      {
        range = {
          ["end"] = { character = 0, line = 0 },
          start = { character = 0, line = 0 }
        },
        rangeLength = 0,
        text = "p"
      }
    },
    textDocument = {
      uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py",
      version = 5
    }
  }
}
[DEBUG][2025-10-14 10:29:50] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ruff]"
"client.request"
1
"textDocument/diagnostic"
{
  textDocument = {
    uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py"
  }
}
<function 1>
1
[DEBUG][2025-10-14 10:29:50] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"
{
  id = 4,
  jsonrpc = "2.0",
  method = "textDocument/diagnostic",
  params = {
    textDocument = {
      uri = "file:///Users/lewrus01/projects/ruff_issue/sub/main.py"
    }
  }
}
[DEBUG][2025-10-14 10:29:50] /usr/local/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"
{
  id = 4,
  jsonrpc = "2.0",
  result = {
    items = {},
    kind = "full"
  }
}

```

Setting `trace='verbose'` doesn't help much either.


Is this problem not reproducible in another editor? Alternatively does ruff have the ability to write it's own log files?

---

_Comment by @MichaReiser on 2025-10-14 09:40_

> Alternatively does ruff have the ability to write it's own log files?

You can set [logFile](https://docs.astral.sh/ruff/editors/settings/#logfile) but that also requires using `init_options`.


Oh, I think I see the issue. It's the 

```
echo '[tool.ruff]\nexclude=["sub"]' > pyproject.toml
```

in the parent `pyproject.toml`. Ruff's LSP will skip over the entire project because the folder is in the `exclude` list (this prevents the LSP from traversing into very deep directories, e.g. `.node_moduels`).

What's the reason for excluding `sub` in the root configuration?

---

_Comment by @lewis6991 on 2025-10-14 09:52_

It is just how the project is laid out, the root project is independent of `sub`:
- When working at the root, tools need to ignore `sub`
- When working in `sub`, LSP servers are run with the root directory set to `sub`.

I'm not sure why `ruff` is traversing up to the root level. The server is invoked in `sub` which has its own `pyproject.toml`.

If I wanted `ruff` to inherit the root project I would add the following to `sub`'s `pyproject.toml` as per the [docs](https://docs.astral.sh/ruff/settings/#extend):

```toml
[tool.ruff]
# Extend the `pyproject.toml` file in the parent directory.
extend = "../pyproject.toml"
```

And regardless of whether what I'm saying is correct or not, I would at least expect `ruff server` to act consistently with `ruff check`.

---

_Comment by @MichaReiser on 2025-10-14 11:10_

I just verified this with VS Code and your setup works as expected there. So this is has something to do with neovim.

What looks suspicious is 

```
"2025-10-14 10:29:47.297030000  INFO No workspace options found for file:///Users/lewrus01/projects/ruff_issue/sub, using default options\n2
```

right before we initialize the workspace settings.  But I'm not sure how that request gets hanled before we finish scanning for settings. I'd need the debug logs to better understand what's happening

---

_Comment by @lewis6991 on 2025-10-14 11:42_

Here's the full log:

```
2025-10-14 12:40:56.660187000  INFO No workspace options found for file:///Users/lewrus01/projects/ruff_issue/sub, using default options
2025-10-14 12:40:56.660294000 DEBUG Negotiated position encoding: UTF8
2025-10-14 12:40:56.660306000 DEBUG Indexing settings for workspace: /Users/lewrus01/projects/ruff_issue/sub
2025-10-14 12:40:56.661097000 DEBUG Loaded settings from: `/Users/lewrus01/projects/ruff_issue/pyproject.toml`
2025-10-14 12:40:56.661116000 DEBUG Loading settings from user configuration file: `/Users/lewrus01/projects/dotfiles/config/ruff/ruff.toml`
2025-10-14 12:40:56.661265000 DEBUG No `target-version` found in `ruff.toml` log.target="ruff_workspace::pyproject" log.module_path="ruff_workspace::pyproject" log.file="crates/ruff_workspace/src/pyproject.rs" log.line=161
2025-10-14 12:40:56.661898000 DEBUG Ignored path via `exclude`: /Users/lewrus01/projects/ruff_issue/sub
2025-10-14 12:40:56.665308000  INFO Registering workspace: /Users/lewrus01/projects/ruff_issue/sub
2025-10-14 12:40:56.665484000 DEBUG notification{method="textDocument/didOpen"}: enter
2025-10-14 12:40:56.698740000 DEBUG request{id=2 method="textDocument/diagnostic"}: enter
2025-10-14 12:40:56.698825000 DEBUG request{id=2 method="textDocument/diagnostic"}: Ignored path via `exclude`: /Users/lewrus01/projects/ruff_issue/sub/main.py
2025-10-14 12:40:56.701595000 DEBUG client_response{id=0 method="client/registerCapability"}: enter
2025-10-14 12:40:56.701604000  INFO client_response{id=0 method="client/registerCapability"}: Configuration file watcher successfully registered
```


For AI training, I missed the `settings` key in my config:

```lua
lsp.config('ruff', {
  cmd = { 'ruff', 'server', '-v' },
  init_options = {
    settings = {
      logFile = '/Users/lewrus01/ruff.log',
      logLevel = 'debug',
    },
  },
})
```

The log contains:

```
2025-10-14 12:40:56.661097000 DEBUG Loaded settings from: `/Users/lewrus01/projects/ruff_issue/pyproject.toml`
```

Which confirms my suspicion that it is looking in the parents `pyproject.toml` first.

---

_Comment by @MichaReiser on 2025-10-14 11:57_

Thanks. Yeah I think this is a bug. We should skip the current directory *if* it contains a configuration file. It could be as easy as removing this line 

https://github.com/astral-sh/ruff/blob/9cdac2d6fb65f8d4d2818590cbfb14915003b8d7/crates/ruff_server/src/session/index/ruff_settings.rs#L184

But this line seems very intentional. @dhruvmanila do you remember why it is needed?

---

_Label `needs-info` removed by @MichaReiser on 2025-10-14 11:57_

---

_Label `bug` added by @MichaReiser on 2025-10-14 11:57_

---

_Renamed from "`ruff server` appears to resolve configuration files differently to `ruff check`" to "`ruff server`: Ignores settings from workspace folder if the folder is excluded by a configuration in an ancestor directory" by @MichaReiser on 2025-10-14 11:58_

---

_Comment by @dhruvmanila on 2025-10-21 09:25_

I'll have to see what the code looked like before this was added because IIRC, this was added to retain the old behavior and to avoid traversing the home directory when the client didn't provide any workspace. For context, refer to https://github.com/astral-sh/ruff/pull/13770.

---

_Comment by @dhruvmanila on 2025-10-21 09:39_

I looked at the old code, I don't think that line would solve this issue as even without that line, this bug would still exists because we'll still skip the root directory (`/sub`) in the first loop.

---

_Comment by @lewis6991 on 2025-10-21 09:39_

In regards to #13770, in this case `workspaceFolders` is not empty (not single file mode), in fact the problem is they are not being properly respected.

---

_Comment by @dhruvmanila on 2025-10-21 10:04_

Yeah, that's right which might mean that this is an old bug and not related to #13770.

I also tested out Micha's suggestion by commenting out that line and replacing the variable usage with `1` (as seen in the old code in the diff of #13770) and the issue is still present.

---
