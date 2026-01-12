```yaml
number: 1727
title: "`vim.diagnostic.setloclist`: Duplicate diagnostics shown after buffer save"
type: issue
state: closed
author: woniulol
labels:
  - bug
  - editor
assignees: []
created_at: 2025-12-02T18:31:52Z
updated_at: 2025-12-04T07:12:06Z
url: https://github.com/astral-sh/ty/issues/1727
synced_at: 2026-01-12T15:54:25Z
```

# `vim.diagnostic.setloclist`: Duplicate diagnostics shown after buffer save

---

_@woniulol_

### Summary

### Issue
`vim.diagnostic.setloclist()` produces duplicated entries after saving the buffer (:w).

This will cause vim.diagnostic.config virtual text to be duplicated, and prevent usage of the `vim.diagnostic.setloclist()`.

<img width="1198" height="99" alt="Image" src="https://github.com/user-attachments/assets/9114145c-b254-46a7-a3cc-648f1a59acb9" />

### Reproduces the issue

A minimal nvim config file:
```
-- init.lua

local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not (vim.uv or vim.loop).fs_stat(lazypath) then
    local lazyrepo = "https://github.com/folke/lazy.nvim.git"
    local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
    if vim.v.shell_error ~= 0 then
        vim.api.nvim_echo({
            { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
            { out,                            "WarningMsg" },
            { "\nPress any key to exit..." },
        }, true, {})
        vim.fn.getchar()
        os.exit(1)
    end
end
vim.opt.rtp:prepend(lazypath)

require("lazy").setup(
    {
        {
            "neovim/nvim-lspconfig",
            dependencies = {
                { "mason-org/mason.nvim" },
                { "mason-org/mason-lspconfig.nvim" },
            },
            config = function()
                require("mason").setup({})
                require("mason-lspconfig").setup({
                    automatic_enable = false,
                    ensure_installed = {
                        "lua_ls",
                        "ty",
                    },
                })
                -- Ignore other nvim lspconfig fields for this example.
                vim.lsp.enable("ty")
            end
        }
    }
)
```

`.py` script to test against
```
# main.py

def my_func(s: str) -> None:
    print(s)

a: int = my_func()
```
1. open the  with nvim
2. check `:lua vim.diagnostic.setloclist()` (working as expected)
3. write the buffer `:w` (with or without any modification)
4. check `:lua vim.diagnostic.setloclist()`, diagnostic messages will shown as duplicated, e.g.
```
main.py|4 col 10-22 error| Object of type `str` is not assignable to `int`
main.py|4 col 10-22 error| Object of type `str` is not assignable to `int`
```

### Specs
- macOS 26.1 (25B78)
- NVIM v0.11.5
- mason.nvim v2.1.0 
- ty installed version 0.0.1a29
- lazy.nvim 11.17.5

### Additional Info
- `pyright`, `lua_ls` with same config does not result any duplicates.
- No luck after setting `disableLanguageServices = true`

### Version

0.0.1a29

---

_Label `server` added by @carljm on 2025-12-02 18:46_

---

_Label `server` removed by @MichaReiser on 2025-12-02 19:10_

---

_Label `editor` added by @MichaReiser on 2025-12-02 19:10_

---

_Renamed from "Duplicate diagnostics shown after buffer save" to "`vim.diagnostic.setloclist`: Duplicate diagnostics shown after buffer save" by @MichaReiser on 2025-12-02 19:11_

---

_Comment by @MichaReiser on 2025-12-02 20:26_

Any chance you're also using Ruff? Do you see the same issue with Ruff?

---

_Comment by @dhruvmanila on 2025-12-03 04:38_

I'm able to reproduce this using the config that you've provided. There's no need to invoke the `vim.diagnostic.setloclist`, it even occurs just by saving the buffer.

https://github.com/user-attachments/assets/4769d4c4-4fd3-481a-a44e-6dd2f0f508a1

Here are the trace logs:
```
[START][2025-12-03 10:04:44] LSP logging initiated
[INFO][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"Starting RPC client"	{ cmd = { "ty", "server" }, extra = {} }
[TRACE][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ty]"	"init_params"	{ capabilities = { general = { positionEncodings = { "utf-8", "utf-16", "utf-32" } }, textDocument = { callHierarchy = { dynamicRegistration = false }, codeAction = { codeActionLiteralSupport = { codeActionKind = { valueSet = { "", "quickfix", "refactor", "refactor.extract", "refactor.inline", "refactor.rewrite", "source", "source.organizeImports" } } }, dataSupport = true, disabledSupport = true, dynamicRegistration = true, honorsChangeAnnotations = true, isPreferredSupport = true, resolveSupport = { properties = { "edit", "command" } } }, codeLens = { dynamicRegistration = false, resolveSupport = { properties = { "command" } } }, colorProvider = { dynamicRegistration = true }, completion = { completionItem = { commitCharactersSupport = false, deprecatedSupport = true, documentationFormat = { "markdown", "plaintext" }, insertReplaceSupport = true, preselectSupport = false, resolveSupport = { properties = { "additionalTextEdits", "command" } }, snippetSupport = true, tagSupport = { valueSet = { 1 } } }, completionItemKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25 } }, completionList = { itemDefaults = { "editRange", "insertTextFormat", "insertTextMode", "data" } }, contextSupport = true, dynamicRegistration = false }, declaration = { linkSupport = true }, definition = { dynamicRegistration = true, linkSupport = true }, diagnostic = { dataSupport = true, dynamicRegistration = false, relatedDocumentSupport = true, relatedInformation = true, tagSupport = { valueSet = { 1, 2 } } }, documentHighlight = { dynamicRegistration = false }, documentSymbol = { dynamicRegistration = false, hierarchicalDocumentSymbolSupport = true, symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } }, tagSupport = { valueSet = { 1 } } }, foldingRange = { dynamicRegistration = false, foldingRange = { collapsedText = true }, foldingRangeKind = { valueSet = { "comment", "imports", "region" } }, lineFoldingOnly = true }, formatting = { dynamicRegistration = true }, hover = { contentFormat = { "markdown", "plaintext" }, dynamicRegistration = true }, implementation = { linkSupport = true }, inlayHint = { dynamicRegistration = true, resolveSupport = { properties = { "textEdits", "tooltip", "location", "command" } } }, inlineCompletion = { dynamicRegistration = false }, linkedEditingRange = { dynamicRegistration = false }, onTypeFormatting = { dynamicRegistration = false }, publishDiagnostics = { dataSupport = true, relatedInformation = true, tagSupport = { valueSet = { 1, 2 } }, versionSupport = true }, rangeFormatting = { dynamicRegistration = true, rangesSupport = true }, references = { dynamicRegistration = false }, rename = { dynamicRegistration = true, honorsChangeAnnotations = true, prepareSupport = true }, selectionRange = { dynamicRegistration = false }, semanticTokens = { augmentsSyntaxTokens = true, dynamicRegistration = false, formats = { "relative" }, multilineTokenSupport = true, overlappingTokenSupport = true, requests = { full = { delta = true }, range = true }, serverCancelSupport = false, tokenModifiers = { "declaration", "definition", "readonly", "static", "deprecated", "abstract", "async", "modification", "documentation", "defaultLibrary" }, tokenTypes = { "namespace", "type", "class", "enum", "interface", "struct", "typeParameter", "parameter", "variable", "property", "enumMember", "event", "function", "method", "macro", "keyword", "modifier", "comment", "string", "number", "regexp", "operator", "decorator" } }, signatureHelp = { dynamicRegistration = false, signatureInformation = { activeParameterSupport = true, documentationFormat = { "markdown", "plaintext" }, noActiveParameterSupport = true, parameterInformation = { labelOffsetSupport = true } } }, synchronization = { didSave = true, dynamicRegistration = false, willSave = true, willSaveWaitUntil = true }, typeDefinition = { linkSupport = true } }, window = { showDocument = { support = true }, showMessage = { messageActionItem = { additionalPropertiesSupport = true } }, workDoneProgress = true }, workspace = { applyEdit = true, configuration = true, diagnostics = { refreshSupport = false }, didChangeConfiguration = { dynamicRegistration = false }, didChangeWatchedFiles = { dynamicRegistration = true, relativePatternSupport = true }, inlayHint = { refreshSupport = true }, semanticTokens = { refreshSupport = true }, symbol = { dynamicRegistration = false, symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } } }, workspaceEdit = { changeAnnotationSupport = { groupsOnLabel = true }, normalizesLineEndings = true, resourceOperations = { "rename", "create", "delete" } }, workspaceFolders = true } }, clientInfo = { name = "Neovim", version = "0.12.0-dev+g6d875a03bb" }, initializationOptions = { logFile = "/Users/dhruv/dotfiles/ty.log", logLevel = "debug" }, processId = 51119, rootPath = "/Users/dhruv/dotfiles", rootUri = "file:///Users/dhruv/dotfiles", trace = "off", workDoneToken = "1", workspaceFolders = { { name = "/Users/dhruv/dotfiles", uri = "file:///Users/dhruv/dotfiles" } } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 1, jsonrpc = "2.0", method = "initialize", params = { capabilities = { general = { positionEncodings = { "utf-8", "utf-16", "utf-32" } }, textDocument = { callHierarchy = { dynamicRegistration = false }, codeAction = { codeActionLiteralSupport = { codeActionKind = { valueSet = { "", "quickfix", "refactor", "refactor.extract", "refactor.inline", "refactor.rewrite", "source", "source.organizeImports" } } }, dataSupport = true, disabledSupport = true, dynamicRegistration = true, honorsChangeAnnotations = true, isPreferredSupport = true, resolveSupport = { properties = { "edit", "command" } } }, codeLens = { dynamicRegistration = false, resolveSupport = { properties = { "command" } } }, colorProvider = { dynamicRegistration = true }, completion = { completionItem = { commitCharactersSupport = false, deprecatedSupport = true, documentationFormat = { "markdown", "plaintext" }, insertReplaceSupport = true, preselectSupport = false, resolveSupport = { properties = { "additionalTextEdits", "command" } }, snippetSupport = true, tagSupport = { valueSet = { 1 } } }, completionItemKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25 } }, completionList = { itemDefaults = { "editRange", "insertTextFormat", "insertTextMode", "data" } }, contextSupport = true, dynamicRegistration = false }, declaration = { linkSupport = true }, definition = { dynamicRegistration = true, linkSupport = true }, diagnostic = { dataSupport = true, dynamicRegistration = false, relatedDocumentSupport = true, relatedInformation = true, tagSupport = { valueSet = { 1, 2 } } }, documentHighlight = { dynamicRegistration = false }, documentSymbol = { dynamicRegistration = false, hierarchicalDocumentSymbolSupport = true, symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } }, tagSupport = { valueSet = { 1 } } }, foldingRange = { dynamicRegistration = false, foldingRange = { collapsedText = true }, foldingRangeKind = { valueSet = { "comment", "imports", "region" } }, lineFoldingOnly = true }, formatting = { dynamicRegistration = true }, hover = { contentFormat = { "markdown", "plaintext" }, dynamicRegistration = true }, implementation = { linkSupport = true }, inlayHint = { dynamicRegistration = true, resolveSupport = { properties = { "textEdits", "tooltip", "location", "command" } } }, inlineCompletion = { dynamicRegistration = false }, linkedEditingRange = { dynamicRegistration = false }, onTypeFormatting = { dynamicRegistration = false }, publishDiagnostics = { dataSupport = true, relatedInformation = true, tagSupport = { valueSet = { 1, 2 } }, versionSupport = true }, rangeFormatting = { dynamicRegistration = true, rangesSupport = true }, references = { dynamicRegistration = false }, rename = { dynamicRegistration = true, honorsChangeAnnotations = true, prepareSupport = true }, selectionRange = { dynamicRegistration = false }, semanticTokens = { augmentsSyntaxTokens = true, dynamicRegistration = false, formats = { "relative" }, multilineTokenSupport = true, overlappingTokenSupport = true, requests = { full = { delta = true }, range = true }, serverCancelSupport = false, tokenModifiers = { "declaration", "definition", "readonly", "static", "deprecated", "abstract", "async", "modification", "documentation", "defaultLibrary" }, tokenTypes = { "namespace", "type", "class", "enum", "interface", "struct", "typeParameter", "parameter", "variable", "property", "enumMember", "event", "function", "method", "macro", "keyword", "modifier", "comment", "string", "number", "regexp", "operator", "decorator" } }, signatureHelp = { dynamicRegistration = false, signatureInformation = { activeParameterSupport = true, documentationFormat = { "markdown", "plaintext" }, noActiveParameterSupport = true, parameterInformation = { labelOffsetSupport = true } } }, synchronization = { didSave = true, dynamicRegistration = false, willSave = true, willSaveWaitUntil = true }, typeDefinition = { linkSupport = true } }, window = { showDocument = { support = true }, showMessage = { messageActionItem = { additionalPropertiesSupport = true } }, workDoneProgress = true }, workspace = { applyEdit = true, configuration = true, diagnostics = { refreshSupport = false }, didChangeConfiguration = { dynamicRegistration = false }, didChangeWatchedFiles = { dynamicRegistration = true, relativePatternSupport = true }, inlayHint = { refreshSupport = true }, semanticTokens = { refreshSupport = true }, symbol = { dynamicRegistration = false, symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } } }, workspaceEdit = { changeAnnotationSupport = { groupsOnLabel = true }, normalizesLineEndings = true, resourceOperations = { "rename", "create", "delete" } }, workspaceFolders = true } }, clientInfo = { name = "Neovim", version = "0.12.0-dev+g6d875a03bb" }, initializationOptions = { logFile = "/Users/dhruv/dotfiles/ty.log", logLevel = "debug" }, processId = 51119, rootPath = "/Users/dhruv/dotfiles", rootUri = "file:///Users/dhruv/dotfiles", trace = "off", workDoneToken = "1", workspaceFolders = { { name = "/Users/dhruv/dotfiles", uri = "file:///Users/dhruv/dotfiles" } } } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 1, jsonrpc = "2.0", result = { capabilities = { codeActionProvider = { codeActionKinds = { "quickfix" } }, completionProvider = { triggerCharacters = { "." } }, declarationProvider = true, definitionProvider = true, diagnosticProvider = { identifier = "ty", interFileDependencies = true, workDoneProgress = true, workspaceDiagnostics = true }, documentHighlightProvider = true, documentSymbolProvider = true, executeCommandProvider = { commands = { "ty.printDebugInformation" }, workDoneProgress = false }, hoverProvider = true, inlayHintProvider = vim.empty_dict(), notebookDocumentSync = { notebookSelector = { { cells = { { language = "python" } } } }, save = false }, positionEncoding = "utf-8", referencesProvider = true, selectionRangeProvider = true, semanticTokensProvider = { full = true, legend = { tokenModifiers = { "definition", "readonly", "async" }, tokenTypes = { "namespace", "class", "parameter", "selfParameter", "clsParameter", "variable", "property", "function", "method", "keyword", "string", "number", "decorator", "builtinConstant", "typeParameter" } }, range = true }, signatureHelpProvider = { retriggerCharacters = { ")" }, triggerCharacters = { "(", "," } }, textDocumentSync = { change = 2, openClose = true }, typeDefinitionProvider = true, workspaceSymbolProvider = true }, serverInfo = { name = "ty", version = "0.0.1-alpha.29 (0c3cae494 2025-11-28)" } } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ jsonrpc = "2.0", method = "initialized", params = vim.empty_dict() }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ jsonrpc = "2.0", method = "textDocument/didOpen", params = { textDocument = { languageId = "python", text = 'def foo(x: str) -> str:\n    return x\n\na: int = foo("")\n', uri = "file:///Users/dhruv/dotfiles/t.py", version = 0 } } }
[INFO][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ty]"	"server_capabilities"	{ server_capabilities = { codeActionProvider = { codeActionKinds = { "quickfix" } }, completionProvider = { triggerCharacters = { "." } }, declarationProvider = true, definitionProvider = true, diagnosticProvider = { identifier = "ty", interFileDependencies = true, workDoneProgress = true, workspaceDiagnostics = true }, documentHighlightProvider = true, documentSymbolProvider = true, executeCommandProvider = { commands = { "ty.printDebugInformation" }, workDoneProgress = false }, hoverProvider = true, inlayHintProvider = vim.empty_dict(), notebookDocumentSync = { notebookSelector = { { cells = { { language = "python" } } } }, save = false }, positionEncoding = "utf-8", referencesProvider = true, selectionRangeProvider = true, semanticTokensProvider = { full = true, legend = { tokenModifiers = { "definition", "readonly", "async" }, tokenTypes = { "namespace", "class", "parameter", "selfParameter", "clsParameter", "variable", "property", "function", "method", "keyword", "string", "number", "decorator", "builtinConstant", "typeParameter" } }, range = true }, signatureHelpProvider = { retriggerCharacters = { ")" }, triggerCharacters = { "(", "," } }, textDocumentSync = { change = 2, openClose = true }, typeDefinitionProvider = true, workspaceSymbolProvider = true } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 0, jsonrpc = "2.0", method = "workspace/configuration", params = { items = { { scopeUri = "file:///Users/dhruv/dotfiles", section = "ty" } } } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ty]"	"client.request"	1	"textDocument/diagnostic"	{ textDocument = { uri = "file:///Users/dhruv/dotfiles/t.py" } }	<function 1>	1
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 2, jsonrpc = "2.0", method = "textDocument/diagnostic", params = { textDocument = { uri = "file:///Users/dhruv/dotfiles/t.py" } } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ty]"	"client.request"	1	"textDocument/semanticTokens/range"	{ range = { ["end"] = { character = 0, line = 4 }, start = { character = 0, line = 0 } }, textDocument = { uri = "file:///Users/dhruv/dotfiles/t.py" } }	<function 1>	1
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 3, jsonrpc = "2.0", method = "textDocument/semanticTokens/range", params = { range = { ["end"] = { character = 0, line = 4 }, start = { character = 0, line = 0 } }, textDocument = { uri = "file:///Users/dhruv/dotfiles/t.py" } } }
[TRACE][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request"	"workspace/configuration"	{ items = { { scopeUri = "file:///Users/dhruv/dotfiles", section = "ty" } } }
[TRACE][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request: found handler for"	"workspace/configuration"
[TRACE][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"default_handler"	"workspace/configuration"	{ ctx = '{\n  client_id = 1,\n  method = "workspace/configuration"\n}', result = { items = { { scopeUri = "file:///Users/dhruv/dotfiles", section = "ty" } } } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request: callback result"	{ result = { vim.NIL }, status = true }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 0, jsonrpc = "2.0", result = { vim.NIL } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 1, jsonrpc = "2.0", method = "client/registerCapability", params = { registrations = { { id = "ty/workspace/didChangeWatchedFiles", method = "workspace/didChangeWatchedFiles", registerOptions = { watchers = { { globPattern = { baseUri = "file:///Users/dhruv/dotfiles", pattern = "**" }, kind = 7 } } } } } } }
[TRACE][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request"	"client/registerCapability"	{ registrations = { { id = "ty/workspace/didChangeWatchedFiles", method = "workspace/didChangeWatchedFiles", registerOptions = { watchers = { { globPattern = { baseUri = "file:///Users/dhruv/dotfiles", pattern = "**" }, kind = 7 } } } } } }
[TRACE][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request: found handler for"	"client/registerCapability"
[TRACE][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"default_handler"	"client/registerCapability"	{ ctx = '{\n  client_id = 1,\n  method = "client/registerCapability"\n}', result = { registrations = { { id = "ty/workspace/didChangeWatchedFiles", method = "workspace/didChangeWatchedFiles", registerOptions = { watchers = { { globPattern = { baseUri = "file:///Users/dhruv/dotfiles", pattern = "**" }, kind = 7 } } } } } } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request: callback result"	{ result = vim.NIL, status = true }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 1, jsonrpc = "2.0", result = vim.NIL }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 3, jsonrpc = "2.0", result = { data = { 0, 4, 3, 7, 1, 0, 4, 1, 2, 1, 0, 3, 3, 1, 0, 0, 8, 3, 1, 0, 1, 11, 1, 2, 0, 2, 0, 1, 5, 1, 0, 3, 3, 1, 0, 0, 6, 3, 7, 0, 0, 4, 2, 10, 0 } } }
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 2, jsonrpc = "2.0", result = { items = { { code = "invalid-assignment", codeDescription = { href = "https://ty.dev/rules#invalid-assignment" }, data = vim.NIL, message = "Object of type `str` is not assignable to `int`", range = { ["end"] = { character = 16, line = 3 }, start = { character = 9, line = 3 } }, relatedInformation = { { location = { range = { ["end"] = { character = 6, line = 3 }, start = { character = 3, line = 3 } }, uri = "file:///Users/dhruv/dotfiles/t.py" }, message = "Declared type" } }, severity = 1, source = "ty" } }, kind = "full", resultId = "5f5f3c955f80e275" } }
[ERROR][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"LSP[ty]"	"Cannot find request with id 3 whilst attempting to cancel"
[DEBUG][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ jsonrpc = "2.0", method = "$/cancelRequest", params = { id = 3 } }
[TRACE][2025-12-03 10:04:44] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"default_handler"	"textDocument/diagnostic"	{ ctx = '{\n  bufnr = 1,\n  client_id = 1,\n  method = "textDocument/diagnostic",\n  params = {\n    textDocument = {\n      uri = "file:///Users/dhruv/dotfiles/t.py"\n    }\n  },\n  version = 0\n}', result = { items = { { code = "invalid-assignment", codeDescription = { href = "https://ty.dev/rules#invalid-assignment" }, data = vim.NIL, message = "Object of type `str` is not assignable to `int`", range = { ["end"] = { character = 16, line = 3 }, start = { character = 9, line = 3 } }, relatedInformation = { { location = { range = { ["end"] = { character = 6, line = 3 }, start = { character = 3, line = 3 } }, uri = "file:///Users/dhruv/dotfiles/t.py" }, message = "Declared type" } }, severity = 1, source = "ty" } }, kind = "full", resultId = "5f5f3c955f80e275" } }
[DEBUG][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ jsonrpc = "2.0", method = "workspace/didChangeWatchedFiles", params = { changes = { { type = 3, uri = "file:///Users/dhruv/dotfiles/4913" }, { type = 1, uri = "file:///Users/dhruv/dotfiles/t.py" }, { type = 3, uri = "file:///Users/dhruv/dotfiles/t.py~" } } } }
[DEBUG][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ jsonrpc = "2.0", method = "textDocument/publishDiagnostics", params = { diagnostics = { { code = "invalid-assignment", codeDescription = { href = "https://ty.dev/rules#invalid-assignment" }, data = vim.NIL, message = "Object of type `str` is not assignable to `int`", range = { ["end"] = { character = 16, line = 3 }, start = { character = 9, line = 3 } }, relatedInformation = { { location = { range = { ["end"] = { character = 6, line = 3 }, start = { character = 3, line = 3 } }, uri = "file:///Users/dhruv/dotfiles/t.py" }, message = "Declared type" } }, severity = 1, source = "ty" } }, uri = "file:///Users/dhruv/dotfiles/t.py", version = 0 } }
[DEBUG][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 2, jsonrpc = "2.0", method = "workspace/inlayHint/refresh" }
[TRACE][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"notification"	"textDocument/publishDiagnostics"	{ diagnostics = { { code = "invalid-assignment", codeDescription = { href = "https://ty.dev/rules#invalid-assignment" }, data = vim.NIL, message = "Object of type `str` is not assignable to `int`", range = { ["end"] = { character = 16, line = 3 }, start = { character = 9, line = 3 } }, relatedInformation = { { location = { range = { ["end"] = { character = 6, line = 3 }, start = { character = 3, line = 3 } }, uri = "file:///Users/dhruv/dotfiles/t.py" }, message = "Declared type" } }, severity = 1, source = "ty" } }, uri = "file:///Users/dhruv/dotfiles/t.py", version = 0 }
[TRACE][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"default_handler"	"textDocument/publishDiagnostics"	{ ctx = '{\n  client_id = 1,\n  method = "textDocument/publishDiagnostics"\n}', result = { diagnostics = { { code = "invalid-assignment", codeDescription = { href = "https://ty.dev/rules#invalid-assignment" }, data = vim.NIL, message = "Object of type `str` is not assignable to `int`", range = { ["end"] = { character = 16, line = 3 }, start = { character = 9, line = 3 } }, relatedInformation = { { location = { range = { ["end"] = { character = 6, line = 3 }, start = { character = 3, line = 3 } }, uri = "file:///Users/dhruv/dotfiles/t.py" }, message = "Declared type" } }, severity = 1, source = "ty" } }, uri = "file:///Users/dhruv/dotfiles/t.py", version = 0 } }
[TRACE][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request"	"workspace/inlayHint/refresh"	nil
[TRACE][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request: found handler for"	"workspace/inlayHint/refresh"
[TRACE][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"default_handler"	"workspace/inlayHint/refresh"	{ ctx = '{\n  client_id = 1,\n  method = "workspace/inlayHint/refresh"\n}' }
[DEBUG][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"server_request: callback result"	{ result = vim.NIL, status = true }
[DEBUG][2025-12-03 10:04:46] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 2, jsonrpc = "2.0", result = vim.NIL }
[INFO][2025-12-03 10:04:47] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"exit_handler"	{ <1>{ _enabled_capabilities = {}, _is_stopping = false, _log_prefix = "LSP[ty]", _on_attach_cbs = {}, _on_exit_cbs = {}, _on_init_cbs = {}, _trace = "off", attached_buffers = { true }, cancel_request = <function 1>, capabilities = { general = { positionEncodings = { "utf-8", "utf-16", "utf-32" } }, textDocument = { callHierarchy = { dynamicRegistration = false }, codeAction = { codeActionLiteralSupport = { codeActionKind = { valueSet = { "", "quickfix", "refactor", "refactor.extract", "refactor.inline", "refactor.rewrite", "source", "source.organizeImports" } } }, dataSupport = true, disabledSupport = true, dynamicRegistration = true, honorsChangeAnnotations = true, isPreferredSupport = true, resolveSupport = { properties = { "edit", "command" } } }, codeLens = { dynamicRegistration = false, resolveSupport = { properties = { "command" } } }, colorProvider = { dynamicRegistration = true }, completion = { completionItem = { commitCharactersSupport = false, deprecatedSupport = true, documentationFormat = { "markdown", "plaintext" }, insertReplaceSupport = true, preselectSupport = false, resolveSupport = { properties = { "additionalTextEdits", "command" } }, snippetSupport = true, tagSupport = { valueSet = { 1 } } }, completionItemKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25 } }, completionList = { itemDefaults = { "editRange", "insertTextFormat", "insertTextMode", "data" } }, contextSupport = true, dynamicRegistration = false }, declaration = { linkSupport = true }, definition = { dynamicRegistration = true, linkSupport = true }, diagnostic = { dataSupport = true, dynamicRegistration = false, relatedDocumentSupport = true, relatedInformation = true, tagSupport = { valueSet = { 1, 2 } } }, documentHighlight = { dynamicRegistration = false }, documentSymbol = { dynamicRegistration = false, hierarchicalDocumentSymbolSupport = true, symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } }, tagSupport = { valueSet = { 1 } } }, foldingRange = { dynamicRegistration = false, foldingRange = { collapsedText = true }, foldingRangeKind = { valueSet = { "comment", "imports", "region" } }, lineFoldingOnly = true }, formatting = { dynamicRegistration = true }, hover = { contentFormat = { "markdown", "plaintext" }, dynamicRegistration = true }, implementation = { linkSupport = true }, inlayHint = { dynamicRegistration = true, resolveSupport = { properties = { "textEdits", "tooltip", "location", "command" } } }, inlineCompletion = { dynamicRegistration = false }, linkedEditingRange = { dynamicRegistration = false }, onTypeFormatting = { dynamicRegistration = false }, publishDiagnostics = { dataSupport = true, relatedInformation = true, tagSupport = { valueSet = { 1, 2 } }, versionSupport = true }, rangeFormatting = { dynamicRegistration = true, rangesSupport = true }, references = { dynamicRegistration = false }, rename = { dynamicRegistration = true, honorsChangeAnnotations = true, prepareSupport = true }, selectionRange = { dynamicRegistration = false }, semanticTokens = { augmentsSyntaxTokens = true, dynamicRegistration = false, formats = { "relative" }, multilineTokenSupport = true, overlappingTokenSupport = true, requests = { full = { delta = true }, range = true }, serverCancelSupport = false, tokenModifiers = { "declaration", "definition", "readonly", "static", "deprecated", "abstract", "async", "modification", "documentation", "defaultLibrary" }, tokenTypes = { "namespace", "type", "class", "enum", "interface", "struct", "typeParameter", "parameter", "variable", "property", "enumMember", "event", "function", "method", "macro", "keyword", "modifier", "comment", "string", "number", "regexp", "operator", "decorator" } }, signatureHelp = { dynamicRegistration = false, signatureInformation = { activeParameterSupport = true, documentationFormat = { "markdown", "plaintext" }, noActiveParameterSupport = true, parameterInformation = { labelOffsetSupport = true } } }, synchronization = { didSave = true, dynamicRegistration = false, willSave = true, willSaveWaitUntil = true }, typeDefinition = { linkSupport = true } }, window = { showDocument = { support = true }, showMessage = { messageActionItem = { additionalPropertiesSupport = true } }, workDoneProgress = true }, workspace = { applyEdit = true, configuration = true, diagnostics = { refreshSupport = false }, didChangeConfiguration = { dynamicRegistration = false }, didChangeWatchedFiles = { dynamicRegistration = true, relativePatternSupport = true }, inlayHint = { refreshSupport = true }, semanticTokens = { refreshSupport = true }, symbol = { dynamicRegistration = false, symbolKind = { valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 } } }, workspaceEdit = { changeAnnotationSupport = { groupsOnLabel = true }, normalizesLineEndings = true, resourceOperations = { "rename", "create", "delete" } }, workspaceFolders = true } }, commands = {}, config = { cmd = { "ty", "server" }, filetypes = { "python" }, init_options = { logFile = "/Users/dhruv/dotfiles/ty.log", logLevel = "debug" }, name = "ty", root_dir = "/Users/dhruv/dotfiles", root_markers = { "ty.toml", "pyproject.toml", "setup.py", "setup.cfg", "requirements.txt", ".git" } }, dynamic_capabilities = { capabilities = <2>{ ["workspace/didChangeWatchedFiles"] = { { id = "ty/workspace/didChangeWatchedFiles", method = "workspace/didChangeWatchedFiles", registerOptions = { watchers = { { globPattern = { baseUri = "file:///Users/dhruv/dotfiles", pattern = "**" }, kind = 7 } } } } } }, client_id = 1, get = <function 2>, register = <function 3>, supports = <function 4>, supports_registration = <function 5>, unregister = <function 6> }, exit_timeout = 3000, flags = {}, get_language_id = <function 7>, handlers = {}, id = 1, initialized = true, is_stopped = <function 8>, messages = { messages = {}, name = "ty", progress = {}, status = {} }, name = "ty", notify = <function 9>, offset_encoding = "utf-8", on_attach = <function 10>, progress = { _idx_read = 0, _idx_write = 0, _items = {}, _size = 51, pending = {}, <metatable> = { __call = <function 11>, __index = { clear = <function 12>, peek = <function 13>, pop = <function 14>, push = <function 15> } } }, registrations = <table 2>, request = <function 16>, request_sync = <function 17>, requests = {}, root_dir = "/Users/dhruv/dotfiles", rpc = { is_closing = <function 18>, notify = <function 19>, request = <function 20>, terminate = <function 21> }, server_capabilities = { codeActionProvider = { codeActionKinds = { "quickfix" } }, completionProvider = { triggerCharacters = { "." } }, declarationProvider = true, definitionProvider = true, diagnosticProvider = { identifier = "ty", interFileDependencies = true, workDoneProgress = true, workspaceDiagnostics = true }, documentHighlightProvider = true, documentSymbolProvider = true, executeCommandProvider = { commands = { "ty.printDebugInformation" }, workDoneProgress = false }, hoverProvider = true, inlayHintProvider = vim.empty_dict(), notebookDocumentSync = { notebookSelector = { { cells = { { language = "python" } } } }, save = false }, positionEncoding = "utf-8", referencesProvider = true, selectionRangeProvider = true, semanticTokensProvider = { full = true, legend = { tokenModifiers = { "definition", "readonly", "async" }, tokenTypes = { "namespace", "class", "parameter", "selfParameter", "clsParameter", "variable", "property", "function", "method", "keyword", "string", "number", "decorator", "builtinConstant", "typeParameter" } }, range = true }, signatureHelpProvider = { retriggerCharacters = { ")" }, triggerCharacters = { "(", "," } }, textDocumentSync = { change = 2, openClose = true }, typeDefinitionProvider = true, workspaceSymbolProvider = true }, server_info = { name = "ty", version = "0.0.1-alpha.29 (0c3cae494 2025-11-28)" }, settings = {}, stop = <function 22>, supports_method = <function 23>, workspace_folders = { { name = "/Users/dhruv/dotfiles", uri = "file:///Users/dhruv/dotfiles" } }, <metatable> = <3>{ __index = <table 3>, _add_workspace_folder = <function 24>, _all = { <table 1> }, _get_language_id = <function 25>, _get_registration = <function 26>, _notification = <function 27>, _on_detach = <function 28>, _on_error = <function 29>, _on_exit = <function 30>, _process_request = <function 31>, _process_static_registrations = <function 32>, _register = <function 33>, _register_dynamic = <function 34>, _remove_workspace_folder = <function 35>, _resolve_handler = <function 36>, _run_callbacks = <function 37>, _server_request = <function 38>, _supports_registration = <function 39>, _text_document_did_open_handler = <function 40>, _unregister = <function 41>, _unregister_dynamic = <function 42>, cancel_request = <function 43>, create = <function 44>, exec_cmd = <function 45>, initialize = <function 46>, is_stopped = <function 47>, notify = <function 48>, on_attach = <function 49>, request = <function 50>, request_sync = <function 51>, stop = <function 52>, supports_method = <function 53>, write_error = <function 54> } } }
[DEBUG][2025-12-03 10:04:47] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ id = 4, jsonrpc = "2.0", method = "shutdown" }
[DEBUG][2025-12-03 10:04:47] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.receive"	{ id = 4, jsonrpc = "2.0", result = vim.NIL }
[DEBUG][2025-12-03 10:04:47] /Users/dhruv/neovim/share/nvim/runtime/lua/vim/lsp/log.lua:151	"rpc.send"	{ jsonrpc = "2.0", method = "exit" }
```

I think the issue is that ty is using both pull and publish diagnostics as seen in the logs. The editor is sending the `textDocument/diagnostic` request which is first populated in the buffer. But, when I save the file, it sends the `workspace/didChangeWatchedFiles` notification to the server and in response ty sends the `textDocument/publishDiagnostics` notification which contains the same diagnostics. And, I think Neovim uses separate namespace that contains these diagnostics which means that they're shown twice. Here are the two diagnostic namespace which I presume corresponds to the pull and push part:

```lua
:=vim.diagnostic.get_namespaces()
{                                                                                                                   
  [8] = {                                                                                                           
    name = "nvim.lsp.ty.1",                                                                                         
    opts = {},                                                                                                      
    user_data = {                                                                                                   
      location_ns = 15,                                                                                             
      sign_ns = 17,                                                                                                 
      underline_ns = 16                                                                                             
    }                                                                                                               
  },                                                                                                                
  [11] = {                                                                                                          
    name = "nvim.lsp.ty.1.ty",                                                                                      
    opts = {},                                                                                                      
    user_data = {                                                                                                   
      location_ns = 12,                                                                                             
      sign_ns = 14,                                                                                                 
      underline_ns = 13                                                                                             
    }                                                                                                               
  }                                                                                                                 
}
```

---

_Label `bug` added by @dhruvmanila on 2025-12-03 04:39_

---

_Comment by @dhruvmanila on 2025-12-03 04:43_

Hmm, but I'm unable to reproduce this in my own config.

---

_Comment by @woniulol on 2025-12-03 06:12_

> Any chance you're also using Ruff? Do you see the same issue with Ruff?

I have tried to changed the `ty` in my minimum config to `ruff` and replicated the test against the following `main.py`

```
def my_func() -> None:
   a: int = 123 
   return 
```

Warning line `main.py|2 col 4-5 warning| Local variable `a` is assigned to but never used` is not duplicated.

I can ensure that i was only using `ty` in my first example.

```
- LSP log level : WARN
- Log path: /Users/xxx/.local/state/nvim/lsp.log
- Log size: 151 KB

vim.lsp: Active Clients ~
- ty (id: 1)
  - Version: 0.0.1-alpha.29 (0c3cae494 2025-11-28)
  - Root directory: ~/develop/temp/try-ruff-lsp
  - Command: { "ty", "server" }
  - Settings: {}
  - Attached buffers: 1

vim.lsp: Enabled Configurations ~
- ty:
  - cmd: { "ty", "server" }
  - filetypes: python
  - root_markers: { "ty.toml", "pyproject.toml", "setup.py", "setup.cfg", "requirements.txt", ".git" }


vim.lsp: File Watcher ~
- File watch backend: libuv-watch

vim.lsp: Position Encodings ~
- No buffers contain mixed position encodings
```

---

_Comment by @MichaReiser on 2025-12-03 07:26_

Thanks @dhruvmanila The logs are very helpful and the fix should be easy. It's interesting that vim sends a `didChangeWatchedFiles` diagnostic for this file, given that it is an open file. 

I get the impression that this could break our versioning because we now have new content on disk but we also have the unsaved in-editor content that neovim never notified us that it has changed (or that the file was closed). 

---

_Closed by @MichaReiser on 2025-12-04 07:12_

---
