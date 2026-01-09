---
number: 11545
title: Ruff server not closing with neovim instances - vol 2
type: issue
state: closed
author: danielhollas
labels:
  - bug
  - server
assignees: []
created_at: 2024-05-26T16:13:53Z
updated_at: 2025-02-17T07:59:08Z
url: https://github.com/astral-sh/ruff/issues/11545
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff server not closing with neovim instances - vol 2

---

_Issue opened by @danielhollas on 2024-05-26 16:13_

I am still seeing this problem originally reported (and fixed) in #11207 
I am on ruff 0.4.5 and neovim 0.9.5 on Linux Fedora 39.

Here's the output from Lsp logs

```
[START][2024-05-26 16:20:07] LSP logging initiated
  16021 [ERROR][2024-05-26 16:20:07] .../vim/lsp/rpc.lua:734»   "rpc"»  "/home/hollas/.local/share/nvim/mason/bin/ruff"»"stderr"»       "wa        rning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the follow        ing options in `.venv/lib64/python3.12/site-packages/pandas/pyproject.toml`:\n  - 'ignore' -> 'lint.ignore'\n  - 'select' -> 'lint.        select'\n  - 'typing-modules' -> 'lint.typing-modules'\n  - 'unfixable' -> 'lint.unfixable'\n  - 'per-file-ignores' -> 'lint.per-fi        le-ignores'\n"
  16022 [ERROR][2024-05-26 16:20:07] .../vim/lsp/rpc.lua:734»   "rpc"»  "/home/hollas/.local/share/nvim/mason/bin/ruff"»"stderr"»       "wa        rning: `PGH001` has been remapped to `S307`.\n"
  16023 [ERROR][2024-05-26 16:20:07] .../vim/lsp/rpc.lua:734»   "rpc"»  "/home/hollas/.local/share/nvim/mason/bin/ruff"»"stderr"»       "           0.700874s WARN ruff_server::server LSP client does not support dynamic capability registration - automatic configuration reloading         will not be available.\n"
  16024 [ERROR][2024-05-26 16:20:16] .../vim/lsp/rpc.lua:734»   "rpc"»  "/home/hollas/.local/share/nvim/mason/bin/ruff"»"stderr"»       "           9.789324s INFO ruff_server::server::connection Shutdown request received. Waiting for an exit notification...\n"
```

_Originally posted by @danielhollas in https://github.com/astral-sh/ruff/issues/11207#issuecomment-2128365016_

cc @dhruvmanila 
            

---

_Label `server` added by @AlexWaygood on 2024-05-26 16:15_

---

_Comment by @dhruvmanila on 2024-05-27 10:30_

Hey, thanks for providing the logs. I've a couple of follow-up questions:

1. Are there no other logs apart from this?
2. Do you get any diagnostics in the editor regardless of what the logs are saying?

I think it'll also be useful to provide `INFO` level logs. You'll need to set the log level for the Neovim LSP client to INFO with the following snippet where you setup your LSP configs.
```lua
vim.lsp.set_log_level(vim.lsp.log_levels.INFO)
require('vim.lsp.log').set_format_func(vim.inspect)
```

---

_Comment by @danielhollas on 2024-05-27 12:38_

> Are there no other logs apart from this?

Nope.

> Do you get any diagnostics in the editor regardless of what the logs are saying?

The ruff server functionality (diagnostics / code actions) work as expected AFAIK.

Here are the logs including INFO level.

```
[START][2024-05-27 13:30:49] LSP logging initiated
  [INFO][2024-05-27 13:30:49] .../vim/lsp/rpc.lua:662»    "Starting RPC client"»  {
    args = { "server", "--preview" },
    cmd = "/home/hollas/.local/share/nvim/mason/bin/ruff",
    extra = {
      cwd = "/home/hollas/atmospec/aiida-core"
    }
  }
  [INFO][2024-05-27 13:30:49] .../lua/vim/lsp.lua:1344»   "LSP[ruff]"»    "server_capabilities"»  {
    server_capabilities = {
      codeActionProvider = {
        codeActionKinds = { "quickfix", "source.fixAll.ruff", "source.organizeImports.ruff", "notebook.source.fixAll.ruff", "notebook.sourc        e.organizeImports.ruff" },
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
      hoverProvider = true,
      notebookDocumentSync = {
        notebookSelector = { {
            cells = { {
                language = "python"                                                                                                        
              } }
          } },
        save = false
      },
      positionEncoding = "utf-16",
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
  [ERROR][2024-05-27 13:30:49] .../vim/lsp/rpc.lua:734»   "rpc"»  "/home/hollas/.local/share/nvim/mason/bin/ruff"»"stderr"»       "warning:   The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options   in `.venv/lib64/python3.12/site-packages/pandas/pyproject.toml`:\n  - 'ignore' -> 'lint.ignore'\n  - 'select' -> 'lint.select'\n  - 'typi  ng-modules' -> 'lint.typing-modules'\n  - 'unfixable' -> 'lint.unfixable'\n  - 'per-file-ignores' -> 'lint.per-file-ignores'\n"
  [ERROR][2024-05-27 13:30:49] .../vim/lsp/rpc.lua:734»   "rpc"»  "/home/hollas/.local/share/nvim/mason/bin/ruff"»"stderr"»       "warning:   `PGH001` has been remapped to `S307`.\n"
  [ERROR][2024-05-27 13:30:50] .../vim/lsp/rpc.lua:734»   "rpc"»  "/home/hollas/.local/share/nvim/mason/bin/ruff"»"stderr"»       "   0.270  272s WARN ruff_server::server LSP client does not support dynamic capability registration - automatic configuration reloading will not be   available.\n"
  [INFO][2024-05-27 13:31:24] .../lua/vim/lsp.lua:1875»   "exit_handler"» { {
      _on_attach = <function 1>,
      attached_buffers = { true },
      cancel_request = <function 2>,
      commands = {},
      config = {
        autostart = true,
        capabilities = {
          textDocument = {
            callHierarchy = {
              dynamicRegistration = false
            },
            codeAction = {
              codeActionLiteralSupport = {
                codeActionKind = {
                  valueSet = { "", "quickfix", "refactor", "refactor.extract", "refactor.inline", "refactor.rewrite", "source", "source.org                  anizeImports" }
                }
              },                                                                                                                           
              dataSupport = true,
              dynamicRegistration = false,
              isPreferredSupport = true,
              resolveSupport = {
                properties = { "edit" }
              }
            },
             completion = {
              completionItem = {
                commitCharactersSupport = true,
                deprecatedSupport = true,
                documentationFormat = { "markdown", "plaintext" },
                insertReplaceSupport = true,
                insertTextModeSupport = {
                  valueSet = { 1, 2 }
                },
                labelDetailsSupport = true,
                preselectSupport = true,
                resolveSupport = {
                  properties = { "documentation", "detail", "additionalTextEdits", "sortText", "filterText", "insertText", "textEdit", "ins                  ertTextFormat", "insertTextMode" }
                },
                snippetSupport = true,
                tagSupport = {
                  valueSet = { 1 }
                }
              },
              completionItemKind = {
                valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25 }                   
              },
              completionList = {
                itemDefaults = { "commitCharacters", "editRange", "insertTextFormat", "insertTextMode", "data" }
              },
              contextSupport = true,
              dynamicRegistration = false,
              insertTextMode = 1
            },
            declaration = {
              linkSupport = true
            },
            definition = {                                                                                                                 
              linkSupport = true
            },
            documentHighlight = {
              dynamicRegistration = false
            },
            documentSymbol = {
              dynamicRegistration = false,
              hierarchicalDocumentSymbolSupport = true,
              symbolKind = {
                valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 }
              }
            },
            hover = {
              contentFormat = { "markdown", "plaintext" },
              dynamicRegistration = false
            },
            implementation = {
              linkSupport = true
            },
            publishDiagnostics = {
              relatedInformation = true,
              tagSupport = {
                valueSet = { 1, 2 }
              }
            },
            references = {
              dynamicRegistration = false
            },
            rename = {
              dynamicRegistration = false,
              prepareSupport = true
            },                                                                                                                             
            semanticTokens = {
              augmentsSyntaxTokens = true,
              dynamicRegistration = false,
              formats = { "relative" },
              multilineTokenSupport = false,
              overlappingTokenSupport = true,
              requests = {
                full = {
                  delta = true
                },
              serverCancelSupport = false,
              tokenModifiers = { "declaration", "definition", "readonly", "static", "deprecated", "abstract", "async", "modification", "doc              umentation", "defaultLibrary" },
              tokenTypes = { "namespace", "type", "class", "enum", "interface", "struct", "typeParameter", "parameter", "variable", "proper              ty", "enumMember", "event", "function", "method", "macro", "keyword", "modifier", "comment", "string", "number", "regexp", "o              perator", "decorator" }
            },
            signatureHelp = {
              dynamicRegistration = false,
              signatureInformation = {
                activeParameterSupport = true,
                documentationFormat = { "markdown", "plaintext" },
                parameterInformation = {
                  labelOffsetSupport = true
                }
              }
            },
            synchronization = {
              didSave = true,
              dynamicRegistration = false,
              willSave = true,
              willSaveWaitUntil = true
            },                                                                                                                             
            typeDefinition = {
              linkSupport = true
            }
          },
          window = {
            showDocument = {
              support = true
            },
            showMessage = {
              messageActionItem = {
                additionalPropertiesSupport = false
              }
            },
            workDoneProgress = true
          },
          workspace = {
            applyEdit = true,
            configuration = true,
            didChangeWatchedFiles = {
              dynamicRegistration = false,
              relativePatternSupport = true
            },
            semanticTokens = {
              refreshSupport = true
            },
            symbol = {
              dynamicRegistration = false,
              hierarchicalWorkspaceSymbolSupport = true,
              symbolKind = {
                valueSet = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26 }
              }
            },                                                                                                                             
            workspaceEdit = {
              resourceOperations = { "rename", "create", "delete" }
            },
            workspaceFolders = true
          }
        },
        cmd = { "/home/hollas/.local/share/nvim/mason/bin/ruff", "server", "--preview" },
        cmd_cwd = "/home/hollas/atmospec/aiida-core",
        filetypes = { "python" },
        flags = {},
        get_language_id = <function 3>,
        handlers = <1>{},
        init_options = vim.empty_dict(),
        log_level = 2,
        message_level = 2,
        name = "ruff",
        on_attach = <function 4>,
        on_exit = <function 5>,
        on_init = <function 6>,
        root_dir = "/home/hollas/atmospec/aiida-core",
        settings = {},
        single_file_support = true,
        workspace_folders = <2>{ {
            name = "/home/hollas/atmospec/aiida-core",
            uri = "file:///home/hollas/atmospec/aiida-core"
          } },
        <metatable> = <3>{
          __tostring = <function 7>
        }
      },
      handlers = <table 1>,
      id = 1,                                                                                                                              
      initialized = true,
      is_stopped = <function 8>,
      messages = {
        messages = {},
        name = "ruff",
        progress = {},
        status = {}
      },
      name = "ruff",
      notify = <function 9>,
      offset_encoding = "utf-16",
      request = <function 10>,
      request_sync = <function 11>,
      requests = {},
      rpc = {
        is_closing = <function 12>,
        notify = <function 13>,
        request = <function 14>,
        terminate = <function 15>
      },                                                                                                                                   
      server_capabilities = {
        codeActionProvider = {
          codeActionKinds = { "quickfix", "source.fixAll.ruff", "source.organizeImports.ruff", "notebook.source.fixAll.ruff", "notebook.sou          rce.organizeImports.ruff" },
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
        hoverProvider = true,
        notebookDocumentSync = {
          notebookSelector = { {
              cells = { {
                  language = "python"
                } }
            } },
          save = false
        },
        positionEncoding = "utf-16",
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
      stop = <function 16>,
      supports_method = <function 17>,
      workspace_did_change_configuration = <function 18>,
      workspace_folders = <table 2>
    } }
  [ERROR][2024-05-27 13:31:24] .../vim/lsp/rpc.lua:734»   "rpc"»  "/home/hollas/.local/share/nvim/mason/bin/ruff"»"stderr"»       "  35.157  125s INFO ruff_server::server::connection Shutdown request received. Waiting for an exit notification...\n"
```

---

_Comment by @dhruvmanila on 2024-05-31 07:34_

I'll need some time to get Fedora running in a VM setup but before that can you try running it with the following minimal config: https://gist.github.com/dhruvmanila/1c7afb9825c56c28c8d98a1afd96d05b

In the above gist, there are two files:
1. `minimal.lua` - Neovim setup containing only LSP config for Ruff
2. `test.py` - Test file which will highlight two diagnostic if Ruff is running correctly

Run the above setup with:
```
/path/to/nvim --clean -u /path/to/minimal.lua /path/to/test.py
```

Or, using the `debug` log level:
```
NVIM_LSP_LOG_LEVEL=debug /path/to/nvim --clean -u /path/to/minimal.lua /path/to/test.py
```

> [!IMPORTANT]
>
> **Replace the "/path/to" to the actual path.**

Can you provide the debug logs using this setup?

---

_Comment by @danielhollas on 2024-05-31 09:19_

Will do, thanks! I'll also try to get Neovim 0.10 going to see if it makes any difference. (Fedora didn't yet updated the neovim package, but I'll try to install it from Flathub)

---

_Comment by @dhruvmanila on 2024-05-31 10:41_

If it's available, I could also look at your Neovim config to get the exact setup which might help as well. I see that you're using `mason.nvim` but apart from that I don't know what your setup looks like.

---

_Comment by @danielhollas on 2024-05-31 11:40_

Here's my config (using kickstart.nvim) 
https://github.com/danielhollas/kickstart.nvim/tree/dh

---

_Comment by @dhruvmanila on 2024-05-31 12:39_

Hey @danielhollas, so I've got a VM running with Fedora 38 and I'm using the same setup that you've provided with Neovim 0.9.5 and Ruff 0.4.5 and unfortunately it's working fine on my machine. I'm sorry to ask yet another request but can you provide DEBUG logs?

```lua
vim.lsp.set_log_level(vim.lsp.log_levels.DEBUG)
```

---

_Comment by @danielhollas on 2024-05-31 13:22_

Thanks so much for taking time to investigate this. That's frustrating that you cannot reproduce. How exactly did you install Neovim?

Here's the DEBUG log.

[nvim.log](https://github.com/user-attachments/files/15514236/nvim.log)


---

_Comment by @dhruvmanila on 2024-05-31 14:34_

> How exactly did you install Neovim?

I just installed it via `dnf` and it gave me `0.9.5`

```
sudo dnf install neovim
```

Thanks for the debug logs, I'm looking at it now.

---

_Comment by @dhruvmanila on 2024-05-31 14:44_

The logs doesn't really show anything out of place. How did you determine if the server is not closing with Neovim instance?

I don't really have anything else. I'm sorry that you're facing that. Is this making Ruff unusable i.e., is every instance of Ruff doesn't stop at all? 

We can keep this issue open to let others provide any additional information which can help debug the issue.

---

_Comment by @danielhollas on 2024-06-02 06:54_

> I don't really have anything else. I'm sorry that you're facing that. Is this making Ruff unusable i.e., is every instance of Ruff doesn't stop at all?

Yeah, it's every time I open and close with, 100% reproducible AFAICT. I see them with `pgrep ruff` and have to kill them all from time to time with `killall ruff`. I'll investigate some more and report back.

---

_Comment by @dhruvmanila on 2024-06-03 10:16_

Ok, I can reproduce it now. I'm not sure why it was not showing up earlier.

---

_Label `bug` added by @dhruvmanila on 2024-06-03 10:16_

---

_Comment by @dhruvmanila on 2024-06-03 10:29_

Although, I'm not reliably able to reproduce this. Most of the times the process remains alive but sometimes the server does shutdown.

---

_Comment by @danielhollas on 2024-06-03 12:02_

I noticed that I cannot reproduce with Neovim 0.10 installed via Flatpack.

---

_Comment by @dhruvmanila on 2024-06-03 12:20_

Interesting, let me try that.

---

_Comment by @dhruvmanila on 2024-06-03 12:29_

@danielhollas How are you running it via flatpak? I'm still seeing that the server doesn't quit.

I followed the instruction as mentioned at https://flathub.org/apps/io.neovim.nvim:
```
flatpak install neovim
flatpak run io.neovim.nvim
```

---

_Comment by @danielhollas on 2024-06-03 12:45_

Yes, that's how I run it. (nvim config is located in Flatpack specific dir elsewhere as you probably know)

Heh, seems like we have the opposite problem wrt reproduction 😅

---

_Comment by @danielhollas on 2024-06-03 13:04_

I should note just in case it is relevant that I've upgraded to ruff 0.4.7 in the meantime.

---

_Comment by @dhruvmanila on 2024-06-05 12:51_

I dug a bit deeper here as you mentioned that it's not occurring on the latest version. Using `git bisect`, it seems that commit ded010cf9c9eb91f6c981cbafeb051fdd6ce6774 has solved this problem. I am consistently seeing that `ruff` process remains alive even after exiting Neovim on the parent commit (436dc18b15b6a6c27bbc22a9b4c17be822970eeb) but not on ded010cf9c9eb91f6c981cbafeb051fdd6ce6774. 

I'm a bit confused because the commit hasn't been released yet and somehow you're not seeing that behavior while I'm able to see it consistently 😅 

I'm going to keep this issue open for a while, feel free to ping me if you encounter this issue again. I'll probably look into `tracing-tree` as to what must've caused this bug.

---

_Comment by @MichaReiser on 2024-06-05 12:55_

Maybe https://github.com/davidbarsky/tracing-tree/pull/82 ?

---

_Comment by @dhruvmanila on 2024-06-05 12:57_

Yeah, that could be it although not sure how to confirm.

---

_Comment by @snowsignal on 2024-06-05 18:46_

https://github.com/astral-sh/ruff/pull/11747 will remove `tracing-tree`, just FYI.

---

_Referenced in [astral-sh/ruff#11747](../../astral-sh/ruff/pulls/11747.md) on 2024-06-09 23:04_

---

_Comment by @dhruvmanila on 2024-06-11 02:13_

This should be resolved by https://github.com/astral-sh/ruff/commit/ded010cf9c9eb91f6c981cbafeb051fdd6ce6774 which was released v0.4.8. Feel free to comment or open a new issue if anyone faces this issue again.

---

_Closed by @dhruvmanila on 2024-06-11 02:13_

---

_Comment by @twmht on 2025-02-17 05:52_

@dhruvmanila 

i am still facing these issues with lazyvim, a lot of ruff instance not closed. you can reproduce it via a fresh lazyvim  installation. And then install python language from lazyextra.

---

_Comment by @dhruvmanila on 2025-02-17 07:59_

@twmht Can you open a new issue with a way to reproduce this and other details like version and logs?

---
