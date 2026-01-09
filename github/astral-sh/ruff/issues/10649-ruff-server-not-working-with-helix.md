---
number: 10649
title: "`ruff server` not working with `helix`"
type: issue
state: closed
author: ghost
labels:
  - bug
  - server
assignees: []
created_at: 2024-03-28T20:13:01Z
updated_at: 2024-04-05T00:41:51Z
url: https://github.com/astral-sh/ruff/issues/10649
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff server` not working with `helix`

---

_Issue opened by @ghost on 2024-03-28 20:13_

I've been using `ruff-lsp` with `helix` and I wanted to try out the new `ruff server`, but I can't get it to work. I have the following configuration in my `~/.config/helix/languages.toml`

```toml
[language-server.ruff]
command = "ruff"
args = ["server", "--preview", "--verbose"]
config = { settings = { run = "onSave" } }

[[language]]
name = "python"
language-servers = ["ruff"]
auto-format = true
```

<details>
<summary>Helix log</summary>

```
2024-03-28T11:32:02.361 helix_view::editor [DEBUG] editor status: Loaded 1 file.
2024-03-28T11:32:02.361 helix_lsp::client [INFO] Using custom LSP config: {"settings":{"run":"onSave"}}
2024-03-28T11:32:02.361 helix_lsp::transport [INFO] ruff -> {"jsonrpc":"2.0","method":"initialize","params":{"capabilities":{"general":{"positionEncodings":["utf-8","utf-32","utf-16"]},"textDocument":{"codeAction":{"codeActionLiteralSupport":{"codeActionKind":{"valueSet":["","quickfix","refactor","refactor.extract","refactor.inline","refactor.rewrite","source","source.organizeImports"]}},"dataSupport":true,"disabledSupport":true,"isPreferredSupport":true,"resolveSupport":{"properties":["edit","command"]}},"completion":{"completionItem":{"deprecatedSupport":true,"insertReplaceSupport":true,"resolveSupport":{"properties":["documentation","detail","additionalTextEdits"]},"snippetSupport":true,"tagSupport":{"valueSet":[1]}},"completionItemKind":{}},"hover":{"contentFormat":["markdown"]},"inlayHint":{"dynamicRegistration":false},"publishDiagnostics":{"tagSupport":{"valueSet":[1,2]},"versionSupport":true},"rename":{"dynamicRegistration":false,"honorsChangeAnnotations":false,"prepareSupport":true},"signatureHelp":{"signatureInformation":{"activeParameterSupport":true,"documentationFormat":["markdown"],"parameterInformation":{"labelOffsetSupport":true}}}},"window":{"workDoneProgress":true},"workspace":{"applyEdit":true,"configuration":true,"didChangeConfiguration":{"dynamicRegistration":false},"didChangeWatchedFiles":{"dynamicRegistration":true,"relativePatternSupport":false},"executeCommand":{"dynamicRegistration":false},"fileOperations":{"didRename":true,"willRename":true},"inlayHint":{"refreshSupport":false},"symbol":{"dynamicRegistration":false},"workspaceEdit":{"documentChanges":true,"failureHandling":"abort","normalizesLineEndings":false,"resourceOperations":["create","rename","delete"]},"workspaceFolders":true}},"clientInfo":{"name":"helix","version":"23.10 (05f3d0ea)"},"initializationOptions":{"settings":{"run":"onSave"}},"processId":21722,"rootPath":"/tmp/demo","rootUri":null,"workspaceFolders":[]},"id":0}
2024-03-28T11:32:02.361 helix_tui::backend::crossterm [DEBUG] The keyboard enhancement protocol is supported in this terminal (checked in 176.348µs)
2024-03-28T11:32:02.361 helix_view::document [DEBUG] id 1 modified - last saved: 0, current: 0
2024-03-28T11:32:02.364 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::msg][DEBUG] < {\"jsonrpc\":\"2.0\",\"method\":\"initialize\",\"params\":{\"capabilities\":{\"general\":{\"positionEncodings\":[\"utf-8\",\"utf-32\",\"utf-16\"]},\"textDocument\":{\"codeAction\":{\"codeActionLiteralSupport\":{\"codeActionKind\":{\"valueSet\":[\"\",\"quickfix\",\"refactor\",\"refactor.extract\",\"refactor.inline\",\"refactor.rewrite\",\"source\",\"source.organizeImports\"]}},\"dataSupport\":true,\"disabledSupport\":true,\"isPreferredSupport\":true,\"resolveSupport\":{\"properties\":[\"edit\",\"command\"]}},\"completion\":{\"completionItem\":{\"deprecatedSupport\":true,\"insertReplaceSupport\":true,\"resolveSupport\":{\"properties\":[\"documentation\",\"detail\",\"additionalTextEdits\"]},\"snippetSupport\":true,\"tagSupport\":{\"valueSet\":[1]}},\"completionItemKind\":{}},\"hover\":{\"contentFormat\":[\"markdown\"]},\"inlayHint\":{\"dynamicRegistration\":false},\"publishDiagnostics\":{\"tagSupport\":{\"valueSet\":[1,2]},\"versionSupport\":true},\"rename\":{\"dynamicRegistration\":false,\"honorsChangeAnnotations\":false,\"prepareSupport\":true},\"signatureHelp\":{\"signatureInformation\":{\"activeParameterSupport\":true,\"documentationFormat\":[\"markdown\"],\"parameterInformation\":{\"labelOffsetSupport\":true}}}},\"window\":{\"workDoneProgress\":true},\"workspace\":{\"applyEdit\":true,\"configuration\":true,\"didChangeConfiguration\":{\"dynamicRegistration\":false},\"didChangeWatchedFiles\":{\"dynamicRegistration\":true,\"relativePatternSupport\":false},\"executeCommand\":{\"dynamicRegistration\":false},\"fileOperations\":{\"didRename\":true,\"willRename\":true},\"inlayHint\":{\"refreshSupport\":false},\"symbol\":{\"dynamicRegistration\":false},\"workspaceEdit\":{\"documentChanges\":true,\"failureHandling\":\"abort\",\"normalizesLineEndings\":false,\"resourceOperations\":[\"create\",\"rename\",\"delete\"]},\"workspaceFolders\":true}},\"clientInfo\":{\"name\":\"helix\",\"version\":\"23.10 (05f3d0ea)\"},\"initializationOptions\":{\"settings\":{\"run\":\"onSave\"}},\"processId\":21722,\"rootPath\":\"/tmp/demo\",\"rootUri\":null,\"workspaceFolders\":[]},\"id\":0}\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::stdio][DEBUG] sending message Request(\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "    Request {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "        id: RequestId(\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "            I32(\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                0,\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "            ),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "        ),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "        method: \"initialize\",\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "        params: Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "            \"capabilities\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                \"general\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"positionEncodings\": Array [\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        String(\"utf-8\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        String(\"utf-32\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        String(\"utf-16\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    ],\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                \"textDocument\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"codeAction\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"codeActionLiteralSupport\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"codeActionKind\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                \"valueSet\": Array [\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"quickfix\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"refactor\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"refactor.extract\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"refactor.inline\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"refactor.rewrite\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"source\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"source.organizeImports\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                ],\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"dataSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"disabledSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"isPreferredSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"resolveSupport\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"properties\": Array [\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                String(\"edit\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                String(\"command\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            ],\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"completion\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"completionItem\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"deprecatedSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"insertReplaceSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"resolveSupport\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                \"properties\": Array [\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"documentation\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"detail\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    String(\"additionalTextEdits\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                ],\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"snippetSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"tagSupport\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                \"valueSet\": Array [\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                    Number(1),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                ],\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"completionItemKind\": Object {},\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"hover\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"contentFormat\": Array [\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            String(\"markdown\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        ],\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"inlayHint\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"dynamicRegistration\": Bool(false),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"publishDiagnostics\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"tagSupport\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"valueSet\": Array [\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                Number(1),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                Number(2),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            ],\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"versionSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"rename\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"dynamicRegistration\": Bool(false),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"honorsChangeAnnotations\": Bool(false),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"prepareSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"signatureHelp\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"signatureInformation\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"activeParameterSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"documentationFormat\": Array [\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                String(\"markdown\"),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            ],\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            \"parameterInformation\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                                \"labelOffsetSupport\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                            },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                \"window\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"workDoneProgress\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                \"workspace\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"applyEdit\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"configuration\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"didChangeConfiguration\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"dynamicRegistration\": Bool(false),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"didChangeWatchedFiles\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"dynamicRegistration\": Bool(true),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"relativePatternSupport\": Bool(false),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    \"executeCommand\": Object {\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                        \"dynamicRegistration\": Bool(false),\n"
2024-03-28T11:32:02.365 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    \"fileOperations\": Object {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                        \"didRename\": Bool(true),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                        \"willRename\": Bool(true),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    \"inlayHint\": Object {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                        \"refreshSupport\": Bool(false),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    \"symbol\": Object {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                        \"dynamicRegistration\": Bool(false),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    \"workspaceEdit\": Object {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                        \"documentChanges\": Bool(true),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                        \"failureHandling\": String(\"abort\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                        \"normalizesLineEndings\": Bool(false),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                        \"resourceOperations\": Array [\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                            String(\"create\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                            String(\"rename\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                            String(\"delete\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                        ],\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    \"workspaceFolders\": Bool(true),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            \"clientInfo\": Object {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                \"name\": String(\"helix\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                \"version\": String(\"23.10 (05f3d0ea)\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            \"initializationOptions\": Object {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                \"settings\": Object {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                    \"run\": String(\"onSave\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            \"processId\": Number(21722),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            \"rootPath\": String(\"/tmp/demo\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            \"rootUri\": Null,\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            \"workspaceFolders\": Array [],\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "        },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "    },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- ")\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::msg][DEBUG] > {\"jsonrpc\":\"2.0\",\"id\":0,\"result\":{\"capabilities\":{\"codeActionProvider\":{\"codeActionKinds\":[\"quickfix\",\"source.organizeImports\"],\"resolveProvider\":false,\"workDoneProgress\":true},\"diagnosticProvider\":{\"identifier\":\"Ruff\",\"interFileDependencies\":false,\"workDoneProgress\":true,\"workspaceDiagnostics\":false},\"documentFormattingProvider\":true,\"documentRangeFormattingProvider\":true,\"positionEncoding\":\"utf-8\",\"textDocumentSync\":{\"change\":2,\"openClose\":true,\"willSave\":false,\"willSaveWaitUntil\":false},\"workspace\":{\"workspaceFolders\":{\"changeNotifications\":true,\"supported\":true}}},\"serverInfo\":{\"name\":\"ruff\",\"version\":\"0.3.4\"}}}\n"
2024-03-28T11:32:02.366 helix_lsp::transport [INFO] ruff <- {"jsonrpc":"2.0","id":0,"result":{"capabilities":{"codeActionProvider":{"codeActionKinds":["quickfix","source.organizeImports"],"resolveProvider":false,"workDoneProgress":true},"diagnosticProvider":{"identifier":"Ruff","interFileDependencies":false,"workDoneProgress":true,"workspaceDiagnostics":false},"documentFormattingProvider":true,"documentRangeFormattingProvider":true,"positionEncoding":"utf-8","textDocumentSync":{"change":2,"openClose":true,"willSave":false,"willSaveWaitUntil":false},"workspace":{"workspaceFolders":{"changeNotifications":true,"supported":true}}},"serverInfo":{"name":"ruff","version":"0.3.4"}}}
2024-03-28T11:32:02.366 helix_lsp::transport [INFO] ruff <- {"capabilities":{"codeActionProvider":{"codeActionKinds":["quickfix","source.organizeImports"],"resolveProvider":false,"workDoneProgress":true},"diagnosticProvider":{"identifier":"Ruff","interFileDependencies":false,"workDoneProgress":true,"workspaceDiagnostics":false},"documentFormattingProvider":true,"documentRangeFormattingProvider":true,"positionEncoding":"utf-8","textDocumentSync":{"change":2,"openClose":true,"willSave":false,"willSaveWaitUntil":false},"workspace":{"workspaceFolders":{"changeNotifications":true,"supported":true}}},"serverInfo":{"name":"ruff","version":"0.3.4"}}
2024-03-28T11:32:02.366 helix_lsp::transport [INFO] ruff -> {"jsonrpc":"2.0","method":"initialized","params":{}}
2024-03-28T11:32:02.366 helix_term::application [DEBUG] received editor event: LanguageServerMessage((0, Notification(Notification { jsonrpc: None, method: "initialized", params: None })))
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::msg][DEBUG] < {\"jsonrpc\":\"2.0\",\"method\":\"initialized\",\"params\":{}}\n"
2024-03-28T11:32:02.366 helix_lsp::transport [INFO] ruff -> {"jsonrpc":"2.0","method":"textDocument/didOpen","params":{"textDocument":{"languageId":"python","text":"","uri":"file:///tmp/demo/main.py","version":0}}}
2024-03-28T11:32:02.366 helix_lsp::transport [INFO] ruff -> {"jsonrpc":"2.0","method":"workspace/didChangeConfiguration","params":{"settings":{"settings":{"run":"onSave"}}}}
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::stdio][DEBUG] sending message Notification(\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "    Notification {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "        method: \"initialized\",\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "        params: Object {},\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "    },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- ")\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::msg][DEBUG] < {\"jsonrpc\":\"2.0\",\"method\":\"textDocument/didOpen\",\"params\":{\"textDocument\":{\"languageId\":\"python\",\"text\":\"\",\"uri\":\"file:///tmp/demo/main.py\",\"version\":0}}}\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::stdio][DEBUG] sending message Notification(\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "    Notification {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "        method: \"textDocument/didOpen\",\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "        params: Object {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            \"textDocument\": Object {\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                \"languageId\": String(\"python\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                \"text\": String(\"\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                \"uri\": String(\"file:///tmp/demo/main.py\"),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "                \"version\": Number(0),\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "            },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "        },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- "    },\n"
2024-03-28T11:32:02.366 helix_lsp::transport [ERROR] ruff err <- ")\n"
2024-03-28T11:32:02.367 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::msg][DEBUG] > {\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"client/registerCapability\",\"params\":{\"registrations\":[{\"id\":\"ruff-server-watch\",\"method\":\"workspace/didChangeWatchedFiles\",\"registerOptions\":{\"watchers\":[{\"globPattern\":\"**/.?ruff.toml\"},{\"globPattern\":\"**/pyproject.toml\"}]}}]}}\n"
2024-03-28T11:32:02.368 helix_lsp::transport [INFO] ruff <- {"jsonrpc":"2.0","id":1,"method":"client/registerCapability","params":{"registrations":[{"id":"ruff-server-watch","method":"workspace/didChangeWatchedFiles","registerOptions":{"watchers":[{"globPattern":"**/.?ruff.toml"},{"globPattern":"**/pyproject.toml"}]}}]}}
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::msg][DEBUG] < {\"jsonrpc\":\"2.0\",\"method\":\"workspace/didChangeConfiguration\",\"params\":{\"settings\":{\"settings\":{\"run\":\"onSave\"}}}}\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::stdio][DEBUG] sending message Notification(\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "    Notification {\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "        method: \"workspace/didChangeConfiguration\",\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "        params: Object {\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "            \"settings\": Object {\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "                \"settings\": Object {\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "                    \"run\": String(\"onSave\"),\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "                },\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "            },\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "        },\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "    },\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- ")\n"
2024-03-28T11:32:02.368 helix_term::application [DEBUG] received editor event: LanguageServerMessage((0, MethodCall(MethodCall { jsonrpc: Some(V2), method: "client/registerCapability", params: Map({"registrations": Array [Object {"id": String("ruff-server-watch"), "method": String("workspace/didChangeWatchedFiles"), "registerOptions": Object {"watchers": Array [Object {"globPattern": String("**/.?ruff.toml")}, Object {"globPattern": String("**/pyproject.toml")}]}}]}), id: Num(1) })))
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "┐ruff_server::server::api::notifications::did_open::run{file=file:///tmp/demo/main.py}\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "┘\n"
2024-03-28T11:32:02.368 helix_lsp::file_event [DEBUG] Registering didChangeWatchedFiles for client '0' with id 'ruff-server-watch'
2024-03-28T11:32:02.368 helix_lsp::transport [INFO] ruff -> {"jsonrpc":"2.0","result":null,"id":1}
2024-03-28T11:32:02.368 globset [DEBUG] built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::msg][DEBUG] < {\"jsonrpc\":\"2.0\",\"result\":null,\"id\":1}\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "[2024-03-28][11:32:02][lsp_server::stdio][DEBUG] sending message Response(\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "    Response {\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "        id: RequestId(\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "            I32(\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "                1,\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "            ),\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "        ),\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "        result: None,\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "        error: None,\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "    },\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- ")\n"
2024-03-28T11:32:02.368 helix_lsp::transport [ERROR] ruff err <- "   0.003885s INFO ruff_server::server Configuration file watcher successfully registered\n"
```
</details>

It looks like the data in this line is sent to `ruff`
```
2024-03-28T11:32:02.361 helix_lsp::transport [INFO] ruff -> {"jsonrpc":"2.0","method":"initialize","params":{"capabilities":{"general":{"positionEncodings":["utf-8","utf-32","utf-16"]},"textDocument":{"codeAction":{"codeActionLiteralSupport":{"codeActionKind":{"valueSet":["","quickfix","refactor","refactor.extract","refactor.inline","refactor.rewrite","source","source.organizeImports"]}},"dataSupport":true,"disabledSupport":true,"isPreferredSupport":true,"resolveSupport":{"properties":["edit","command"]}},"completion":{"completionItem":{"deprecatedSupport":true,"insertReplaceSupport":true,"resolveSupport":{"properties":["documentation","detail","additionalTextEdits"]},"snippetSupport":true,"tagSupport":{"valueSet":[1]}},"completionItemKind":{}},"hover":{"contentFormat":["markdown"]},"inlayHint":{"dynamicRegistration":false},"publishDiagnostics":{"tagSupport":{"valueSet":[1,2]},"versionSupport":true},"rename":{"dynamicRegistration":false,"honorsChangeAnnotations":false,"prepareSupport":true},"signatureHelp":{"signatureInformation":{"activeParameterSupport":true,"documentationFormat":["markdown"],"parameterInformation":{"labelOffsetSupport":true}}}},"window":{"workDoneProgress":true},"workspace":{"applyEdit":true,"configuration":true,"didChangeConfiguration":{"dynamicRegistration":false},"didChangeWatchedFiles":{"dynamicRegistration":true,"relativePatternSupport":false},"executeCommand":{"dynamicRegistration":false},"fileOperations":{"didRename":true,"willRename":true},"inlayHint":{"refreshSupport":false},"symbol":{"dynamicRegistration":false},"workspaceEdit":{"documentChanges":true,"failureHandling":"abort","normalizesLineEndings":false,"resourceOperations":["create","rename","delete"]},"workspaceFolders":true}},"clientInfo":{"name":"helix","version":"23.10 (05f3d0ea)"},"initializationOptions":{"settings":{"run":"onSave"}},"processId":21722,"rootPath":"/tmp/demo","rootUri":null,"workspaceFolders":[]},"id":0}
```
and then it responds with an escaped version back as an error a few lines later, and then it responds again over multiple lines where the output is indented and has type information added, like Object, String, and Bool.

I'm using `ruff 0.3.4 (a0263ab4)` with `helix 23.10 (05f3d0ea)`.



---

_Assigned to @snowsignal by @MichaReiser on 2024-03-28 20:21_

---

_Label `bug` added by @dhruvmanila on 2024-03-29 03:55_

---

_Label `server` added by @dhruvmanila on 2024-03-29 03:55_

---

_Comment by @charliermarsh on 2024-03-29 15:31_

\cc @snowsignal 

---

_Comment by @snowsignal on 2024-04-04 22:39_

@jlhamilton777 I believe the version of Ruff you used (`v0.3.4`) had a bug that broke editor integration (see https://github.com/astral-sh/ruff/issues/10618). Could you try using the latest version of Ruff? (`v0.3.5` at the time of writing)

---

_Comment by @ghost on 2024-04-04 23:21_

I tried again with `v0.3.5` and the new `helix 24.3` and it seems that only code formatting is working. I can't remember if that worked before. But it doesn't do other things like autocomplete, code actions, etc.

---

_Comment by @ghost on 2024-04-05 00:15_

I've been using `ruff-lsp` with `pyright` and I misunderstood which features were being provided by each one, and got confused because I couldn't organize imports. I just tried again with the latest changes to `ruff server` and the "Organize imports" and "Fix all" features work now 👍. I will just close this then.

---

_Closed by @ghost on 2024-04-05 00:15_

---
