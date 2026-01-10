```yaml
number: 19200
title: "Server: Consider `root_uri`/`root_path` as a fallback to detect workspace root"
type: issue
state: closed
author: lunik1
labels:
  - server
assignees: []
created_at: 2025-07-08T09:36:21Z
updated_at: 2025-11-24T23:16:15Z
url: https://github.com/astral-sh/ruff/issues/19200
synced_at: 2026-01-10T11:09:59Z
```

# Server: Consider `root_uri`/`root_path` as a fallback to detect workspace root

---

_Issue opened by @lunik1 on 2025-07-08 09:36_

### Summary

When running the ruff LSP server via `ruff server` I am not seeing diagnostics I have enabled using a `pyproject.toml` in a subdirectory. I _do_ see the correct diagnostics when running `ruff check` or the standalone `ruff-lsp`.

I have a mwe of this bug here: https://github.com/lunik1/ruff-server-bug-mwe

The top-level `pyproject.toml` contains a basic ruff configuration:
```toml
[tool.ruff]
target-version = "py311"

[tool.ruff.lint]
```

which is then extended by `ruff-mwe/pyproject.toml` to enable the `RUF` linting rules

```toml
[tool.ruff]
extend = "../pyproject.toml"

[tool.ruff.lint]
extend-select = ["RUF"]
```

`main.py` contains some code that violates `F401` (a default rule) and `RUF005`
```python
import math

def main():
    foo = [2, 3, 4]
    bar = [1] + foo + [5, 6]

    print(bar)
```

which is correctly detected by `ruff check`
```
ruff-mwe/main.py:1:8: F401 [*] `math` imported but unused
  |
1 | import math
  |        ^^^^ F401
2 |
3 | def main():
  |
  = help: Remove unused import: `math`

ruff-mwe/main.py:5:11: RUF005 Consider `[1, *foo, 5, 6]` instead of concatenation
  |
3 | def main():
4 |     foo = [2, 3, 4]
5 |     bar = [1] + foo + [5, 6]
  |           ^^^^^^^^^^^^^^^^^^ RUF005
6 |
7 |     print(bar)
  |
  = help: Replace with `[1, *foo, 5, 6]`

Found 2 errors.
[*] 1 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

however, only the `F401` is displayed in my editor (emacs + lsp-mode) using `ruff server`

<img width="464" height="170" alt="Image" src="https://github.com/user-attachments/assets/e7d75252-1059-4e18-a14d-146c1db814ec" />

If I switch my server command to be `ruff-lsp`, I see both diagnostics:

<img width="668" height="183" alt="Image" src="https://github.com/user-attachments/assets/1675258e-c82b-4e03-8664-68dbe5adc9e1" />

### Version

ruff 0.12.2 (9bee8376a 2025-07-03)

---

_Comment by @ntBre on 2025-07-08 19:56_

Thanks for the nice example!

I can reproduce this in Emacs, but I can't reproduce it when opening the same workspace in VS Code. cc @dhruvmanila 

<details><summary>Emacs logs</summary>

```
2025-07-08 15:47:43.090006744  INFO No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /tmp/ruff-server-bug-mwe-master
2025-07-08 15:47:43.090077074  INFO No workspace options found for file:///tmp/ruff-server-bug-mwe-master, using default options
2025-07-08 15:47:43.090082242 DEBUG Negotiated position encoding: UTF32
2025-07-08 15:47:43.090094464 DEBUG Indexing settings for workspace: /tmp/ruff-server-bug-mwe-master
2025-07-08 15:47:43.091025450 DEBUG Loaded settings from: `/tmp/ruff-server-bug-mwe-master/pyproject.toml`
2025-07-08 15:47:43.091172956  INFO Registering workspace: /tmp/ruff-server-bug-mwe-master
2025-07-08 15:47:43.091926684 DEBUG notification{method="textDocument/didOpen"}: enter
2025-07-08 15:47:43.102397310 DEBUG request{id=2 method="textDocument/diagnostic"}: enter
2025-07-08 15:47:43.102458351 DEBUG request{id=2 method="textDocument/diagnostic"}: Included path via `include`: /tmp/ruff-server-bug-mwe-master/ruff-mwe/main.py
2025-07-08 15:47:43.115683383 DEBUG request{id=3 method="textDocument/codeAction"}: enter
2025-07-08 15:47:43.120755267 DEBUG client_response{id=0 method="client/registerCapability"}: enter
2025-07-08 15:47:43.120765394  INFO client_response{id=0 method="client/registerCapability"}: Configuration file watcher successfully registered
2025-07-08 15:47:43.590841378 DEBUG request{id=4 method="textDocument/codeAction"}: enter
```

</details>

<details><summary>VS Code logs</summary>

```
2025-07-08 15:55:00.100977666 DEBUG Negotiated position encoding: UTF16
2025-07-08 15:55:00.101040593 DEBUG Indexing settings for workspace: /tmp/ruff-server-bug-mwe-master
2025-07-08 15:55:00.104332844 DEBUG Loaded settings from: `/tmp/ruff-server-bug-mwe-master/pyproject.toml` for `/tmp/ruff-server-bug-mwe-master`
2025-07-08 15:55:00.104847155 DEBUG Loaded settings from: `/tmp/ruff-server-bug-mwe-master/ruff-mwe/pyproject.toml` for `/tmp/ruff-server-bug-mwe-master/ruff-mwe`
2025-07-08 15:55:00.107996580  INFO Registering workspace: /tmp/ruff-server-bug-mwe-master
2025-07-08 15:55:00.108315266 DEBUG notification{method="textDocument/didOpen"}: enter
2025-07-08 15:55:00.108422961 DEBUG request{id=1 method="textDocument/diagnostic"}: enter
2025-07-08 15:55:00.108511101 DEBUG request{id=1 method="textDocument/diagnostic"}: Included path via `include`: /tmp/ruff-server-bug-mwe-master/ruff-mwe/main.py
2025-07-08 15:55:00.109651819 DEBUG client_response{id=0 method="client/registerCapability"}: enter
2025-07-08 15:55:00.109659921  INFO client_response{id=0 method="client/registerCapability"}: Configuration file watcher successfully registered
```

</details>

It seems like ruff never reads the nested config file in Emacs for some reason.

---

_Label `question` added by @ntBre on 2025-07-08 19:56_

---

_Comment by @ntBre on 2025-07-08 20:03_

I also can't reproduce this with eglot, so I think this might be an issue with lsp-mode rather than Ruff.

---

_Comment by @lunik1 on 2025-07-08 22:03_

Interesting, thanks for the repro and checking with VSCode. I'll open a report in lsp-mode.

---

_Comment by @MichaReiser on 2025-07-09 07:01_

What I see from the logs is that Emacs loads the settings going through

https://github.com/astral-sh/ruff/blob/015222900f16ae3100b3415e1b6be44aed867e4b/crates/ruff_server/src/session/index/ruff_settings.rs#L197-L207

(It logs: "Loaded settings from" without the "for")

And I suspect we early exit in 

https://github.com/astral-sh/ruff/blob/015222900f16ae3100b3415e1b6be44aed867e4b/crates/ruff_server/src/session/index/ruff_settings.rs#L245-L254

VS code skips most of that code and loads the settings as part of:

https://github.com/astral-sh/ruff/blob/015222900f16ae3100b3415e1b6be44aed867e4b/crates/ruff_server/src/session/index/ruff_settings.rs#L321-L334

@dhruvmanila do you know if this is expected.

---

_Comment by @dhruvmanila on 2025-07-09 08:39_

I think this is because lsp-mode isn't providing the workspace folders to the server, so Ruff is using the current working directory as the default workspace. The default workspace means that the server is running in single-file mode and has different behavior w.r.t. indexing the settings (ref https://github.com/astral-sh/ruff/pull/13770).

```
2025-07-08 15:47:43.090006744  INFO No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /tmp/ruff-server-bug-mwe-master
2025-07-08 15:47:43.090077074  INFO No workspace options found for file:///tmp/ruff-server-bug-mwe-master, using default options
...
```

---

_Comment by @dhruvmanila on 2025-07-09 08:42_

@lunik1 / @ntBre Will you be able to provide the initialization options that the client sends to the server?

You could possibly do this and check the client logs which hopefully captures the stderr:
```diff
diff --git a/crates/ruff_server/src/server.rs b/crates/ruff_server/src/server.rs
index 19e0d75a23..108199333a 100644
--- a/crates/ruff_server/src/server.rs
+++ b/crates/ruff_server/src/server.rs
@@ -56,6 +56,8 @@ impl Server {
     ) -> crate::Result<Self> {
         let (id, init_params) = connection.initialize_start()?;
 
+        eprintln!("{init_params:#?}");
+
         let client_capabilities = init_params.capabilities;
         let position_encoding = Self::find_best_position_encoding(&client_capabilities);
 
```

---

_Comment by @ntBre on 2025-07-09 15:27_

<details><summary>Emacs init params</summary>

```
InitializeParams {
    process_id: Some(
        96169,
    ),
    root_path: Some(
        "/tmp/ruff-server-bug-mwe-master",
    ),
    root_uri: Some(
        Url {
            scheme: "file",
            cannot_be_a_base: false,
            username: "",
            password: None,
            host: None,
            port: None,
            path: "/tmp/ruff-server-bug-mwe-master",
            query: None,
            fragment: None,
        },
    ),
    initialization_options: Some(
        Object {
            "settings": Object {
                "fixAll": Bool(true),
                "importStrategy": String("fromEnvironment"),
                "logLevel": String("debug"),
                "organizeImports": Bool(true),
                "showNotifications": String("off"),
            },
        },
    ),
    capabilities: ClientCapabilities {
        workspace: Some(
            WorkspaceClientCapabilities {
                apply_edit: Some(
                    true,
                ),
                workspace_edit: Some(
                    WorkspaceEditClientCapabilities {
                        document_changes: Some(
                            true,
                        ),
                        resource_operations: Some(
                            [
                                Create,
                                Rename,
                                Delete,
                            ],
                        ),
                        failure_handling: None,
                        normalizes_line_endings: None,
                        change_annotation_support: None,
                    },
                ),
                did_change_configuration: None,
                did_change_watched_files: Some(
                    DidChangeWatchedFilesClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        relative_pattern_support: None,
                    },
                ),
                symbol: Some(
                    WorkspaceSymbolClientCapabilities {
                        dynamic_registration: None,
                        symbol_kind: Some(
                            SymbolKindCapability {
                                value_set: Some(
                                    [
                                        File,
                                        Module,
                                        Namespace,
                                        Package,
                                        Class,
                                        Method,
                                        Property,
                                        Field,
                                        Constructor,
                                        Enum,
                                        Interface,
                                        Function,
                                        Variable,
                                        Constant,
                                        String,
                                        Number,
                                        Boolean,
                                        Array,
                                        Object,
                                        Key,
                                        Null,
                                        EnumMember,
                                        Struct,
                                        Event,
                                        Operator,
                                        TypeParameter,
                                    ],
                                ),
                            },
                        ),
                        tag_support: None,
                        resolve_support: None,
                    },
                ),
                execute_command: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            false,
                        ),
                    },
                ),
                workspace_folders: Some(
                    true,
                ),
                configuration: Some(
                    true,
                ),
                semantic_tokens: None,
                code_lens: Some(
                    CodeLensWorkspaceClientCapabilities {
                        refresh_support: Some(
                            true,
                        ),
                    },
                ),
                file_operations: Some(
                    WorkspaceFileOperationsClientCapabilities {
                        dynamic_registration: None,
                        did_create: Some(
                            false,
                        ),
                        will_create: Some(
                            false,
                        ),
                        did_rename: Some(
                            true,
                        ),
                        will_rename: Some(
                            true,
                        ),
                        did_delete: Some(
                            false,
                        ),
                        will_delete: Some(
                            false,
                        ),
                    },
                ),
                inline_value: None,
                inlay_hint: None,
                diagnostics: Some(
                    DiagnosticWorkspaceClientCapabilities {
                        refresh_support: Some(
                            false,
                        ),
                    },
                ),
            },
        ),
        text_document: Some(
            TextDocumentClientCapabilities {
                synchronization: Some(
                    TextDocumentSyncClientCapabilities {
                        dynamic_registration: None,
                        will_save: Some(
                            true,
                        ),
                        will_save_wait_until: Some(
                            true,
                        ),
                        did_save: Some(
                            true,
                        ),
                    },
                ),
                completion: Some(
                    CompletionClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        completion_item: Some(
                            CompletionItemCapability {
                                snippet_support: Some(
                                    false,
                                ),
                                commit_characters_support: None,
                                documentation_format: Some(
                                    [
                                        Markdown,
                                        PlainText,
                                    ],
                                ),
                                deprecated_support: Some(
                                    true,
                                ),
                                preselect_support: None,
                                tag_support: None,
                                insert_replace_support: Some(
                                    true,
                                ),
                                resolve_support: Some(
                                    CompletionItemCapabilityResolveSupport {
                                        properties: [
                                            "documentation",
                                            "detail",
                                            "additionalTextEdits",
                                            "command",
                                        ],
                                    },
                                ),
                                insert_text_mode_support: Some(
                                    InsertTextModeSupport {
                                        value_set: [
                                            AsIs,
                                            AdjustIndentation,
                                        ],
                                    },
                                ),
                                label_details_support: Some(
                                    true,
                                ),
                            },
                        ),
                        completion_item_kind: None,
                        context_support: Some(
                            true,
                        ),
                        insert_text_mode: None,
                        completion_list: None,
                    },
                ),
                hover: Some(
                    HoverClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        content_format: Some(
                            [
                                Markdown,
                                PlainText,
                            ],
                        ),
                    },
                ),
                signature_help: Some(
                    SignatureHelpClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        signature_information: Some(
                            SignatureInformationSettings {
                                documentation_format: None,
                                parameter_information: Some(
                                    ParameterInformationSettings {
                                        label_offset_support: Some(
                                            true,
                                        ),
                                    },
                                ),
                                active_parameter_support: Some(
                                    true,
                                ),
                            },
                        ),
                        context_support: None,
                    },
                ),
                references: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                document_highlight: None,
                document_symbol: Some(
                    DocumentSymbolClientCapabilities {
                        dynamic_registration: None,
                        symbol_kind: Some(
                            SymbolKindCapability {
                                value_set: Some(
                                    [
                                        File,
                                        Module,
                                        Namespace,
                                        Package,
                                        Class,
                                        Method,
                                        Property,
                                        Field,
                                        Constructor,
                                        Enum,
                                        Interface,
                                        Function,
                                        Variable,
                                        Constant,
                                        String,
                                        Number,
                                        Boolean,
                                        Array,
                                        Object,
                                        Key,
                                        Null,
                                        EnumMember,
                                        Struct,
                                        Event,
                                        Operator,
                                        TypeParameter,
                                    ],
                                ),
                            },
                        ),
                        hierarchical_document_symbol_support: Some(
                            true,
                        ),
                        tag_support: None,
                    },
                ),
                formatting: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                range_formatting: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                on_type_formatting: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                declaration: Some(
                    GotoCapability {
                        dynamic_registration: Some(
                            true,
                        ),
                        link_support: Some(
                            true,
                        ),
                    },
                ),
                definition: Some(
                    GotoCapability {
                        dynamic_registration: Some(
                            true,
                        ),
                        link_support: Some(
                            true,
                        ),
                    },
                ),
                type_definition: Some(
                    GotoCapability {
                        dynamic_registration: Some(
                            true,
                        ),
                        link_support: Some(
                            true,
                        ),
                    },
                ),
                implementation: Some(
                    GotoCapability {
                        dynamic_registration: Some(
                            true,
                        ),
                        link_support: Some(
                            true,
                        ),
                    },
                ),
                code_action: Some(
                    CodeActionClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        code_action_literal_support: Some(
                            CodeActionLiteralSupport {
                                code_action_kind: CodeActionKindLiteralSupport {
                                    value_set: [
                                        "",
                                        "quickfix",
                                        "refactor",
                                        "refactor.extract",
                                        "refactor.inline",
                                        "refactor.rewrite",
                                        "source",
                                        "source.organizeImports",
                                    ],
                                },
                            },
                        ),
                        is_preferred_support: Some(
                            true,
                        ),
                        disabled_support: None,
                        data_support: Some(
                            true,
                        ),
                        resolve_support: Some(
                            CodeActionCapabilityResolveSupport {
                                properties: [
                                    "edit",
                                    "command",
                                ],
                            },
                        ),
                        honors_change_annotations: None,
                    },
                ),
                code_lens: None,
                document_link: Some(
                    DocumentLinkClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        tooltip_support: Some(
                            true,
                        ),
                    },
                ),
                color_provider: None,
                rename: Some(
                    RenameClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        prepare_support: Some(
                            true,
                        ),
                        prepare_support_default_behavior: None,
                        honors_change_annotations: None,
                    },
                ),
                publish_diagnostics: Some(
                    PublishDiagnosticsClientCapabilities {
                        related_information: Some(
                            true,
                        ),
                        tag_support: Some(
                            TagSupport {
                                value_set: [
                                    Unnecessary,
                                    Deprecated,
                                ],
                            },
                        ),
                        version_support: Some(
                            true,
                        ),
                        code_description_support: None,
                        data_support: None,
                    },
                ),
                folding_range: Some(
                    FoldingRangeClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        range_limit: None,
                        line_folding_only: None,
                        folding_range_kind: None,
                        folding_range: None,
                    },
                ),
                selection_range: Some(
                    SelectionRangeClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                linked_editing_range: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                call_hierarchy: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            false,
                        ),
                    },
                ),
                semantic_tokens: None,
                moniker: None,
                type_hierarchy: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                inline_value: None,
                inlay_hint: None,
                diagnostic: Some(
                    DiagnosticClientCapabilities {
                        dynamic_registration: Some(
                            false,
                        ),
                        related_document_support: Some(
                            false,
                        ),
                    },
                ),
                inline_completion: None,
            },
        ),
        notebook_document: None,
        window: Some(
            WindowClientCapabilities {
                work_done_progress: Some(
                    true,
                ),
                show_message: None,
                show_document: Some(
                    ShowDocumentClientCapabilities {
                        support: true,
                    },
                ),
            },
        ),
        general: Some(
            GeneralClientCapabilities {
                regular_expressions: None,
                markdown: None,
                stale_request_support: None,
                position_encodings: Some(
                    [
                        PositionEncodingKind(
                            "utf-32",
                        ),
                        PositionEncodingKind(
                            "utf-16",
                        ),
                    ],
                ),
            },
        ),
        offset_encoding: None,
        experimental: None,
    },
    trace: None,
    workspace_folders: None,
    client_info: Some(
        ClientInfo {
            name: "emacs",
            version: Some(
                "GNU Emacs 31.0.50 (build 1, x86_64-pc-linux-gnu, cairo version 1.18.4)\n of 2025-06-29",
            ),
        },
    ),
    locale: None,
    work_done_progress_params: WorkDoneProgressParams {
        work_done_token: Some(
            String(
                "1",
            ),
        ),
    },
}
2025-07-09 11:18:35.866223665  INFO No workspace(s) were provided during initialization. Using the current working directory as a default workspace: /tmp/ruff-server-bug-mwe-master
2025-07-09 11:18:35.866380739  INFO No workspace options found for file:///tmp/ruff-server-bug-mwe-master, using default options
2025-07-09 11:18:35.866395475 DEBUG Negotiated position encoding: UTF32
2025-07-09 11:18:35.866425996 DEBUG Indexing settings for workspace: /tmp/ruff-server-bug-mwe-master
2025-07-09 11:18:35.873467129 DEBUG Loaded settings from: `/tmp/ruff-server-bug-mwe-master/pyproject.toml`
2025-07-09 11:18:35.874970326  INFO Registering workspace: /tmp/ruff-server-bug-mwe-master
2025-07-09 11:18:35.876230615 DEBUG notification{method="textDocument/didOpen"}: enter
2025-07-09 11:18:35.893975505 DEBUG request{id=2 method="textDocument/diagnostic"}: enter
2025-07-09 11:18:35.894303830 DEBUG request{id=2 method="textDocument/diagnostic"}: Included path via `include`: /tmp/ruff-server-bug-mwe-master/ruff-mwe/main.py
2025-07-09 11:18:35.906733997 DEBUG notification{method="$/cancelRequest"}: enter
2025-07-09 11:18:35.906792664 DEBUG notification{method="$/cancelRequest"}: Cancelled request id=3 method=textDocument/codeAction
2025-07-09 11:18:35.906858943 DEBUG request{id=3 method="textDocument/codeAction"}: enter
2025-07-09 11:18:35.907035712 DEBUG request{id=4 method="textDocument/codeAction"}: enter
2025-07-09 11:18:35.911939139 DEBUG client_response{id=0 method="client/registerCapability"}: enter
2025-07-09 11:18:35.911967285  INFO client_response{id=0 method="client/registerCapability"}: Configuration file watcher successfully registered
2025-07-09 11:18:35.920926473 DEBUG request{id=5 method="textDocument/codeAction"}: enter
2025-07-09 11:18:50.708845760 DEBUG request{id=6 method="textDocument/codeAction"}: enter
2025-07-09 11:18:50.708908268 DEBUG request{id=7 method="textDocument/hover"}: enter
```

</details>

<details><summary>VS Code init params</summary>

```
InitializeParams {
    process_id: Some(
        96649,
    ),
    root_path: Some(
        "/tmp/ruff-server-bug-mwe-master",
    ),
    root_uri: Some(
        Url {
            scheme: "file",
            cannot_be_a_base: false,
            username: "",
            password: None,
            host: None,
            port: None,
            path: "/tmp/ruff-server-bug-mwe-master",
            query: None,
            fragment: None,
        },
    ),
    initialization_options: Some(
        Object {
            "globalSettings": Object {
                "codeAction": Object {
                    "disableRuleComment": Object {
                        "enable": Bool(true),
                    },
                    "fixViolation": Object {
                        "enable": Bool(true),
                    },
                },
                "configuration": Null,
                "configurationPreference": String("editorFirst"),
                "cwd": String("/tmp/ruff-server-bug-mwe-master"),
                "enable": Bool(true),
                "fixAll": Bool(true),
                "format": Object {
                    "args": Array [],
                },
                "ignoreStandardLibrary": Bool(true),
                "importStrategy": String("fromEnvironment"),
                "interpreter": Array [],
                "lint": Object {
                    "args": Array [],
                    "enable": Bool(true),
                    "run": String("onType"),
                },
                "logLevel": String("debug"),
                "nativeServer": String("on"),
                "organizeImports": Bool(true),
                "path": Array [
                    String("/home/brent/astral/ruff/target/debug/ruff"),
                ],
                "showNotifications": String("off"),
                "showSyntaxErrors": Bool(true),
                "workspace": String("/tmp/ruff-server-bug-mwe-master"),
            },
            "settings": Array [
                Object {
                    "codeAction": Object {
                        "disableRuleComment": Object {
                            "enable": Bool(true),
                        },
                        "fixViolation": Object {
                            "enable": Bool(true),
                        },
                    },
                    "configuration": Null,
                    "configurationPreference": String("editorFirst"),
                    "cwd": String("/tmp/ruff-server-bug-mwe-master"),
                    "enable": Bool(true),
                    "exclude": Null,
                    "fixAll": Bool(true),
                    "format": Object {
                        "args": Array [],
                        "preview": Null,
                    },
                    "ignoreStandardLibrary": Bool(true),
                    "importStrategy": String("fromEnvironment"),
                    "interpreter": Array [
                        String("/bin/python"),
                    ],
                    "lineLength": Null,
                    "lint": Object {
                        "args": Array [],
                        "enable": Bool(true),
                        "extendSelect": Null,
                        "ignore": Null,
                        "preview": Null,
                        "run": String("onType"),
                        "select": Null,
                    },
                    "logFile": Null,
                    "logLevel": String("debug"),
                    "nativeServer": String("on"),
                    "organizeImports": Bool(true),
                    "path": Array [
                        String("/home/brent/astral/ruff/target/debug/ruff"),
                    ],
                    "showNotifications": String("off"),
                    "showSyntaxErrors": Bool(true),
                    "workspace": String("file:///tmp/ruff-server-bug-mwe-master"),
                },
            ],
        },
    ),
    capabilities: ClientCapabilities {
        workspace: Some(
            WorkspaceClientCapabilities {
                apply_edit: Some(
                    true,
                ),
                workspace_edit: Some(
                    WorkspaceEditClientCapabilities {
                        document_changes: Some(
                            true,
                        ),
                        resource_operations: Some(
                            [
                                Create,
                                Rename,
                                Delete,
                            ],
                        ),
                        failure_handling: Some(
                            TextOnlyTransactional,
                        ),
                        normalizes_line_endings: Some(
                            true,
                        ),
                        change_annotation_support: Some(
                            ChangeAnnotationWorkspaceEditClientCapabilities {
                                groups_on_label: Some(
                                    true,
                                ),
                            },
                        ),
                    },
                ),
                did_change_configuration: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                did_change_watched_files: Some(
                    DidChangeWatchedFilesClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        relative_pattern_support: Some(
                            true,
                        ),
                    },
                ),
                symbol: Some(
                    WorkspaceSymbolClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        symbol_kind: Some(
                            SymbolKindCapability {
                                value_set: Some(
                                    [
                                        File,
                                        Module,
                                        Namespace,
                                        Package,
                                        Class,
                                        Method,
                                        Property,
                                        Field,
                                        Constructor,
                                        Enum,
                                        Interface,
                                        Function,
                                        Variable,
                                        Constant,
                                        String,
                                        Number,
                                        Boolean,
                                        Array,
                                        Object,
                                        Key,
                                        Null,
                                        EnumMember,
                                        Struct,
                                        Event,
                                        Operator,
                                        TypeParameter,
                                    ],
                                ),
                            },
                        ),
                        tag_support: Some(
                            TagSupport {
                                value_set: [
                                    Deprecated,
                                ],
                            },
                        ),
                        resolve_support: Some(
                            WorkspaceSymbolResolveSupportCapability {
                                properties: [
                                    "location.range",
                                ],
                            },
                        ),
                    },
                ),
                execute_command: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                workspace_folders: Some(
                    true,
                ),
                configuration: Some(
                    true,
                ),
                semantic_tokens: Some(
                    SemanticTokensWorkspaceClientCapabilities {
                        refresh_support: Some(
                            true,
                        ),
                    },
                ),
                code_lens: Some(
                    CodeLensWorkspaceClientCapabilities {
                        refresh_support: Some(
                            true,
                        ),
                    },
                ),
                file_operations: Some(
                    WorkspaceFileOperationsClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        did_create: Some(
                            true,
                        ),
                        will_create: Some(
                            true,
                        ),
                        did_rename: Some(
                            true,
                        ),
                        will_rename: Some(
                            true,
                        ),
                        did_delete: Some(
                            true,
                        ),
                        will_delete: Some(
                            true,
                        ),
                    },
                ),
                inline_value: Some(
                    InlineValueWorkspaceClientCapabilities {
                        refresh_support: Some(
                            true,
                        ),
                    },
                ),
                inlay_hint: Some(
                    InlayHintWorkspaceClientCapabilities {
                        refresh_support: Some(
                            true,
                        ),
                    },
                ),
                diagnostics: Some(
                    DiagnosticWorkspaceClientCapabilities {
                        refresh_support: Some(
                            true,
                        ),
                    },
                ),
            },
        ),
        text_document: Some(
            TextDocumentClientCapabilities {
                synchronization: Some(
                    TextDocumentSyncClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        will_save: Some(
                            true,
                        ),
                        will_save_wait_until: Some(
                            true,
                        ),
                        did_save: Some(
                            true,
                        ),
                    },
                ),
                completion: Some(
                    CompletionClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        completion_item: Some(
                            CompletionItemCapability {
                                snippet_support: Some(
                                    true,
                                ),
                                commit_characters_support: Some(
                                    true,
                                ),
                                documentation_format: Some(
                                    [
                                        Markdown,
                                        PlainText,
                                    ],
                                ),
                                deprecated_support: Some(
                                    true,
                                ),
                                preselect_support: Some(
                                    true,
                                ),
                                tag_support: Some(
                                    TagSupport {
                                        value_set: [
                                            Deprecated,
                                        ],
                                    },
                                ),
                                insert_replace_support: Some(
                                    true,
                                ),
                                resolve_support: Some(
                                    CompletionItemCapabilityResolveSupport {
                                        properties: [
                                            "documentation",
                                            "detail",
                                            "additionalTextEdits",
                                        ],
                                    },
                                ),
                                insert_text_mode_support: Some(
                                    InsertTextModeSupport {
                                        value_set: [
                                            AsIs,
                                            AdjustIndentation,
                                        ],
                                    },
                                ),
                                label_details_support: Some(
                                    true,
                                ),
                            },
                        ),
                        completion_item_kind: Some(
                            CompletionItemKindCapability {
                                value_set: Some(
                                    [
                                        Text,
                                        Method,
                                        Function,
                                        Constructor,
                                        Field,
                                        Variable,
                                        Class,
                                        Interface,
                                        Module,
                                        Property,
                                        Unit,
                                        Value,
                                        Enum,
                                        Keyword,
                                        Snippet,
                                        Color,
                                        File,
                                        Reference,
                                        Folder,
                                        EnumMember,
                                        Constant,
                                        Struct,
                                        Event,
                                        Operator,
                                        TypeParameter,
                                    ],
                                ),
                            },
                        ),
                        context_support: Some(
                            true,
                        ),
                        insert_text_mode: Some(
                            AdjustIndentation,
                        ),
                        completion_list: Some(
                            CompletionListCapability {
                                item_defaults: Some(
                                    [
                                        "commitCharacters",
                                        "editRange",
                                        "insertTextFormat",
                                        "insertTextMode",
                                        "data",
                                    ],
                                ),
                            },
                        ),
                    },
                ),
                hover: Some(
                    HoverClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        content_format: Some(
                            [
                                Markdown,
                                PlainText,
                            ],
                        ),
                    },
                ),
                signature_help: Some(
                    SignatureHelpClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        signature_information: Some(
                            SignatureInformationSettings {
                                documentation_format: Some(
                                    [
                                        Markdown,
                                        PlainText,
                                    ],
                                ),
                                parameter_information: Some(
                                    ParameterInformationSettings {
                                        label_offset_support: Some(
                                            true,
                                        ),
                                    },
                                ),
                                active_parameter_support: Some(
                                    true,
                                ),
                            },
                        ),
                        context_support: Some(
                            true,
                        ),
                    },
                ),
                references: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                document_highlight: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                document_symbol: Some(
                    DocumentSymbolClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        symbol_kind: Some(
                            SymbolKindCapability {
                                value_set: Some(
                                    [
                                        File,
                                        Module,
                                        Namespace,
                                        Package,
                                        Class,
                                        Method,
                                        Property,
                                        Field,
                                        Constructor,
                                        Enum,
                                        Interface,
                                        Function,
                                        Variable,
                                        Constant,
                                        String,
                                        Number,
                                        Boolean,
                                        Array,
                                        Object,
                                        Key,
                                        Null,
                                        EnumMember,
                                        Struct,
                                        Event,
                                        Operator,
                                        TypeParameter,
                                    ],
                                ),
                            },
                        ),
                        hierarchical_document_symbol_support: Some(
                            true,
                        ),
                        tag_support: Some(
                            TagSupport {
                                value_set: [
                                    Deprecated,
                                ],
                            },
                        ),
                    },
                ),
                formatting: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                range_formatting: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                on_type_formatting: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                declaration: Some(
                    GotoCapability {
                        dynamic_registration: Some(
                            true,
                        ),
                        link_support: Some(
                            true,
                        ),
                    },
                ),
                definition: Some(
                    GotoCapability {
                        dynamic_registration: Some(
                            true,
                        ),
                        link_support: Some(
                            true,
                        ),
                    },
                ),
                type_definition: Some(
                    GotoCapability {
                        dynamic_registration: Some(
                            true,
                        ),
                        link_support: Some(
                            true,
                        ),
                    },
                ),
                implementation: Some(
                    GotoCapability {
                        dynamic_registration: Some(
                            true,
                        ),
                        link_support: Some(
                            true,
                        ),
                    },
                ),
                code_action: Some(
                    CodeActionClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        code_action_literal_support: Some(
                            CodeActionLiteralSupport {
                                code_action_kind: CodeActionKindLiteralSupport {
                                    value_set: [
                                        "",
                                        "quickfix",
                                        "refactor",
                                        "refactor.extract",
                                        "refactor.inline",
                                        "refactor.rewrite",
                                        "source",
                                        "source.organizeImports",
                                    ],
                                },
                            },
                        ),
                        is_preferred_support: Some(
                            true,
                        ),
                        disabled_support: Some(
                            true,
                        ),
                        data_support: Some(
                            true,
                        ),
                        resolve_support: Some(
                            CodeActionCapabilityResolveSupport {
                                properties: [
                                    "edit",
                                ],
                            },
                        ),
                        honors_change_annotations: Some(
                            true,
                        ),
                    },
                ),
                code_lens: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                document_link: Some(
                    DocumentLinkClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        tooltip_support: Some(
                            true,
                        ),
                    },
                ),
                color_provider: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                rename: Some(
                    RenameClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        prepare_support: Some(
                            true,
                        ),
                        prepare_support_default_behavior: Some(
                            Identifier,
                        ),
                        honors_change_annotations: Some(
                            true,
                        ),
                    },
                ),
                publish_diagnostics: Some(
                    PublishDiagnosticsClientCapabilities {
                        related_information: Some(
                            true,
                        ),
                        tag_support: Some(
                            TagSupport {
                                value_set: [
                                    Unnecessary,
                                    Deprecated,
                                ],
                            },
                        ),
                        version_support: Some(
                            false,
                        ),
                        code_description_support: Some(
                            true,
                        ),
                        data_support: Some(
                            true,
                        ),
                    },
                ),
                folding_range: Some(
                    FoldingRangeClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        range_limit: Some(
                            5000,
                        ),
                        line_folding_only: Some(
                            true,
                        ),
                        folding_range_kind: Some(
                            FoldingRangeKindCapability {
                                value_set: Some(
                                    [
                                        Comment,
                                        Imports,
                                        Region,
                                    ],
                                ),
                            },
                        ),
                        folding_range: Some(
                            FoldingRangeCapability {
                                collapsed_text: Some(
                                    false,
                                ),
                            },
                        ),
                    },
                ),
                selection_range: Some(
                    SelectionRangeClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                linked_editing_range: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                call_hierarchy: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                semantic_tokens: Some(
                    SemanticTokensClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        requests: SemanticTokensClientCapabilitiesRequests {
                            range: Some(
                                true,
                            ),
                            full: Some(
                                Delta {
                                    delta: Some(
                                        true,
                                    ),
                                },
                            ),
                        },
                        token_types: [
                            SemanticTokenType(
                                "namespace",
                            ),
                            SemanticTokenType(
                                "type",
                            ),
                            SemanticTokenType(
                                "class",
                            ),
                            SemanticTokenType(
                                "enum",
                            ),
                            SemanticTokenType(
                                "interface",
                            ),
                            SemanticTokenType(
                                "struct",
                            ),
                            SemanticTokenType(
                                "typeParameter",
                            ),
                            SemanticTokenType(
                                "parameter",
                            ),
                            SemanticTokenType(
                                "variable",
                            ),
                            SemanticTokenType(
                                "property",
                            ),
                            SemanticTokenType(
                                "enumMember",
                            ),
                            SemanticTokenType(
                                "event",
                            ),
                            SemanticTokenType(
                                "function",
                            ),
                            SemanticTokenType(
                                "method",
                            ),
                            SemanticTokenType(
                                "macro",
                            ),
                            SemanticTokenType(
                                "keyword",
                            ),
                            SemanticTokenType(
                                "modifier",
                            ),
                            SemanticTokenType(
                                "comment",
                            ),
                            SemanticTokenType(
                                "string",
                            ),
                            SemanticTokenType(
                                "number",
                            ),
                            SemanticTokenType(
                                "regexp",
                            ),
                            SemanticTokenType(
                                "operator",
                            ),
                            SemanticTokenType(
                                "decorator",
                            ),
                        ],
                        token_modifiers: [
                            SemanticTokenModifier(
                                "declaration",
                            ),
                            SemanticTokenModifier(
                                "definition",
                            ),
                            SemanticTokenModifier(
                                "readonly",
                            ),
                            SemanticTokenModifier(
                                "static",
                            ),
                            SemanticTokenModifier(
                                "deprecated",
                            ),
                            SemanticTokenModifier(
                                "abstract",
                            ),
                            SemanticTokenModifier(
                                "async",
                            ),
                            SemanticTokenModifier(
                                "modification",
                            ),
                            SemanticTokenModifier(
                                "documentation",
                            ),
                            SemanticTokenModifier(
                                "defaultLibrary",
                            ),
                        ],
                        formats: [
                            TokenFormat(
                                "relative",
                            ),
                        ],
                        overlapping_token_support: Some(
                            false,
                        ),
                        multiline_token_support: Some(
                            false,
                        ),
                        server_cancel_support: Some(
                            true,
                        ),
                        augments_syntax_tokens: Some(
                            true,
                        ),
                    },
                ),
                moniker: None,
                type_hierarchy: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                inline_value: Some(
                    DynamicRegistrationClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                    },
                ),
                inlay_hint: Some(
                    InlayHintClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        resolve_support: Some(
                            InlayHintResolveClientCapabilities {
                                properties: [
                                    "tooltip",
                                    "textEdits",
                                    "label.tooltip",
                                    "label.location",
                                    "label.command",
                                ],
                            },
                        ),
                    },
                ),
                diagnostic: Some(
                    DiagnosticClientCapabilities {
                        dynamic_registration: Some(
                            true,
                        ),
                        related_document_support: Some(
                            false,
                        ),
                    },
                ),
                inline_completion: None,
            },
        ),
        notebook_document: Some(
            NotebookDocumentClientCapabilities {
                synchronization: NotebookDocumentSyncClientCapabilities {
                    dynamic_registration: Some(
                        true,
                    ),
                    execution_summary_report: None,
                },
            },
        ),
        window: Some(
            WindowClientCapabilities {
                work_done_progress: Some(
                    true,
                ),
                show_message: Some(
                    ShowMessageRequestClientCapabilities {
                        message_action_item: Some(
                            MessageActionItemCapabilities {
                                additional_properties_support: Some(
                                    true,
                                ),
                            },
                        ),
                    },
                ),
                show_document: Some(
                    ShowDocumentClientCapabilities {
                        support: true,
                    },
                ),
            },
        ),
        general: Some(
            GeneralClientCapabilities {
                regular_expressions: Some(
                    RegularExpressionsClientCapabilities {
                        engine: "ECMAScript",
                        version: Some(
                            "ES2020",
                        ),
                    },
                ),
                markdown: Some(
                    MarkdownClientCapabilities {
                        parser: "marked",
                        version: Some(
                            "1.1.0",
                        ),
                        allowed_tags: None,
                    },
                ),
                stale_request_support: Some(
                    StaleRequestSupportClientCapabilities {
                        cancel: true,
                        retry_on_content_modified: [
                            "textDocument/semanticTokens/full",
                            "textDocument/semanticTokens/range",
                            "textDocument/semanticTokens/full/delta",
                        ],
                    },
                ),
                position_encodings: Some(
                    [
                        PositionEncodingKind(
                            "utf-16",
                        ),
                    ],
                ),
            },
        ),
        offset_encoding: None,
        experimental: None,
    },
    trace: Some(
        Messages,
    ),
    workspace_folders: Some(
        [
            WorkspaceFolder {
                uri: Url {
                    scheme: "file",
                    cannot_be_a_base: false,
                    username: "",
                    password: None,
                    host: None,
                    port: None,
                    path: "/tmp/ruff-server-bug-mwe-master",
                    query: None,
                    fragment: None,
                },
                name: "ruff-server-bug-mwe-master",
            },
        ],
    ),
    client_info: Some(
        ClientInfo {
            name: "Visual Studio Code",
            version: Some(
                "1.101.2",
            ),
        },
    ),
    locale: Some(
        "en",
    ),
    work_done_progress_params: WorkDoneProgressParams {
        work_done_token: None,
    },
}
2025-07-09 11:21:07.118097593 DEBUG Negotiated position encoding: UTF16
2025-07-09 11:21:07.118287003 DEBUG Indexing settings for workspace: /tmp/ruff-server-bug-mwe-master
2025-07-09 11:21:07.131506239 DEBUG Loaded settings from: `/tmp/ruff-server-bug-mwe-master/pyproject.toml` for `/tmp/ruff-server-bug-mwe-master`
2025-07-09 11:21:07.134581217 DEBUG Loaded settings from: `/tmp/ruff-server-bug-mwe-master/ruff-mwe/pyproject.toml` for `/tmp/ruff-server-bug-mwe-master/ruff-mwe`
2025-07-09 11:21:07.135359041 DEBUG Ignored path via `exclude`: /tmp/ruff-server-bug-mwe-master/ruff-mwe/.mypy_cache
2025-07-09 11:21:07.136630225  INFO Registering workspace: /tmp/ruff-server-bug-mwe-master
2025-07-09 11:21:07.137233515 DEBUG notification{method="textDocument/didOpen"}: enter
2025-07-09 11:21:07.137635662 DEBUG client_response{id=0 method="client/registerCapability"}: enter
2025-07-09 11:21:07.137568195 DEBUG request{id=1 method="textDocument/diagnostic"}: enter
2025-07-09 11:21:07.137661923  INFO client_response{id=0 method="client/registerCapability"}: Configuration file watcher successfully registered
2025-07-09 11:21:07.137814596 DEBUG request{id=1 method="textDocument/diagnostic"}: Included path via `include`: /tmp/ruff-server-bug-mwe-master/ruff-mwe/main.py

```

</details>

I also tried setting the `lsp-log-io` variable in Emacs and got this additional log, if it means anything:

```
Command "/home/brent/astral/ruff/target/debug/ruff server" is present on the path.
Command "/home/brent/astral/ruff/target/debug/ruff server" is present on the path.
Found the following clients for /tmp/ruff-server-bug-mwe-master/ruff-mwe/main.py: (server-id ruff, priority -2)
The following clients were selected based on priority: (server-id ruff, priority -2)
Creating watchers for following 2 folders:
  /tmp/ruff-server-bug-mwe-master
  /tmp/ruff-server-bug-mwe-master/ruff-mwe
```

---

_Comment by @dhruvmanila on 2025-07-10 06:35_

Thank you! That's very helpful

>     root_path: Some(
>         "/tmp/ruff-server-bug-mwe-master",
>     ),
>     root_uri: Some(
>         Url {
>             scheme: "file",
>             cannot_be_a_base: false,
>             username: "",
>             password: None,
>             host: None,
>             port: None,
>             path: "/tmp/ruff-server-bug-mwe-master",
>             query: None,
>             fragment: None,
>         },
>     ),

I figured it might be this. They're using the `root_path` and `root_uri` to provide the workspace root, both of which are deprecated in favor of `workspace_folders` which is not provided. Both Ruff and ty doesn't support this. I wonder if we should fallback to using this information as it seems that not all clients have moved to the `workspace_folders` and it doesn't seem too difficult to add support.

What this would look like is that if `workspace_folders` is empty, then we'd first try `root_uri` and if that is `null` then we'd try `root_path` and then fallback to our current behavior which is to use the current working directory. For Ruff, we can rename `Workspaces::from_workspace_folders` to `Workspaces::from_initialization_params` which looks at `workspace_folders`, `root_uri`, and `root_path` from the initialization params.

---

_Renamed from "`ruff server` not respecting heirarchical configuration files" to "Server: Consider `root_uri`/`root_path` as a fallback to detect workspace root" by @dhruvmanila on 2025-07-10 06:37_

---

_Label `question` removed by @dhruvmanila on 2025-07-10 06:37_

---

_Label `server` added by @dhruvmanila on 2025-07-10 06:37_

---

_Comment by @lunik1 on 2025-10-30 10:40_

I have a workaround for this, `ruff` seems to behave correctly when you tell lsp-mode it is multi root:
```elisp
(oset (gethash 'ruff lsp-clients) multi-root t)
```

I think it makes sense to make this change to lsp-mode upstream?

---

_Comment by @lunik1 on 2025-11-24 17:00_

Upstream MR opened https://github.com/emacs-lsp/lsp-mode/pull/4921

---

_Closed by @lunik1 on 2025-11-24 23:15_

---

_Comment by @lunik1 on 2025-11-24 23:16_

Fixed in `lsp-mode`. `ruff` is now treated as multi-workspace.

---
