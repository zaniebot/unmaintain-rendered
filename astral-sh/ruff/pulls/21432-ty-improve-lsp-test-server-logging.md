```yaml
number: 21432
title: "[ty] Improve LSP test server logging"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/test-server-logging
created_at: 2025-11-13T17:09:44Z
updated_at: 2025-11-14T11:30:21Z
url: https://github.com/astral-sh/ruff/pull/21432
synced_at: 2026-01-12T15:57:23Z
```

# [ty] Improve LSP test server logging

---

_@MichaReiser_

## Summary

Improve the test server logging to get to the root cause of why `ty_server::e2e pull_diagnostics::on_did_open` flakes (it seems to take the dynamic registration path but it's unclear to me WHY?).

## Test Plan

```
2025-11-13 18:09:51.614252000 DEBUG Starting test client with capabilities ClientCapabilities {
    workspace: Some(
        WorkspaceClientCapabilities {
            apply_edit: None,
            workspace_edit: None,
            did_change_configuration: None,
            did_change_watched_files: None,
            symbol: None,
            execute_command: None,
            workspace_folders: None,
            configuration: Some(
                true,
            ),
            semantic_tokens: None,
            code_lens: None,
            file_operations: None,
            inline_value: None,
            inlay_hint: None,
            diagnostics: None,
        },
    ),
    text_document: Some(
        TextDocumentClientCapabilities {
            synchronization: None,
            completion: None,
            hover: None,
            signature_help: None,
            references: None,
            document_highlight: None,
            document_symbol: None,
            formatting: None,
            range_formatting: None,
            on_type_formatting: None,
            declaration: None,
            definition: None,
            type_definition: None,
            implementation: None,
            code_action: None,
            code_lens: None,
            document_link: None,
            color_provider: None,
            rename: None,
            publish_diagnostics: Some(
                PublishDiagnosticsClientCapabilities {
                    related_information: None,
                    tag_support: None,
                    version_support: None,
                    code_description_support: None,
                    data_support: None,
                },
            ),
            folding_range: None,
            selection_range: None,
            linked_editing_range: None,
            call_hierarchy: None,
            semantic_tokens: None,
            moniker: None,
            type_hierarchy: None,
            inline_value: None,
            inlay_hint: None,
            diagnostic: Some(
                DiagnosticClientCapabilities {
                    dynamic_registration: None,
                    related_document_support: None,
                },
            ),
            inline_completion: None,
        },
    ),
    notebook_document: None,
    window: None,
    general: None,
    offset_encoding: None,
    experimental: None,
}
2025-11-13 18:09:51.614536000 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-11-13 18:09:51.614655000 DEBUG Client sends request `initialize` with ID 1
2025-11-13 18:09:51.614964000 DEBUG Initialization options: InitializationOptions {
    log_level: None,
    log_file: None,
    options: ClientOptions {
        global: GlobalOptions {
            diagnostic_mode: None,
            experimental: None,
        },
        workspace: WorkspaceOptions {
            disable_language_services: None,
            inlay_hints: None,
            python_extension: None,
        },
        unknown: {},
    },
}
2025-11-13 18:09:51.615064000 DEBUG Resolved client capabilities: ["PULL_DIAGNOSTICS", "WORKSPACE_CONFIGURATION"]
2025-11-13 18:09:51.615123000  INFO Version: Unknown
2025-11-13 18:09:51.615214000 DEBUG Received server response for request 1
2025-11-13 18:09:51.615424000 DEBUG Client sends notification `initialized`
2025-11-13 18:09:51.615536000 DEBUG Requesting workspace configuration for workspaces
2025-11-13 18:09:51.615608000 DEBUG Received server request `workspace/configuration`
2025-11-13 18:09:51.615618000  INFO Retrying to receive `workspace/configuration` request
2025-11-13 18:09:51.615640000 DEBUG No workspace configuration provided for file:///private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src
2025-11-13 18:09:51.615674000 DEBUG Client sends notification `textDocument/didOpen`
2025-11-13 18:09:51.615685000 DEBUG Client sends request `textDocument/diagnostic` with ID 2
2025-11-13 18:09:51.615762000 DEBUG client_response{id=0 method="workspace/configuration"}: Received workspace configurations, initializing workspaces
2025-11-13 18:09:51.615778000 DEBUG client_response{id=0 method="workspace/configuration"}: No workspace options provided for file:///private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src, using default options
2025-11-13 18:09:51.615800000 DEBUG Deferring `textDocument/didOpen` notification until all workspaces are initialized
2025-11-13 18:09:51.615809000 DEBUG Initializing workspace `file:///private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src`
2025-11-13 18:09:51.615857000 DEBUG Searching for a project in '/private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src'
2025-11-13 18:09:51.615927000 DEBUG The ancestor directories contain no `pyproject.toml`. Falling back to a virtual project.
2025-11-13 18:09:51.615947000 DEBUG Searching for a user-level configuration at `/Users/micha/.config/ty/ty.toml`
2025-11-13 18:09:51.618682000  INFO Defaulting to python-platform `darwin`
2025-11-13 18:09:51.618712000 DEBUG Discovering virtual environment in `/private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src`
2025-11-13 18:09:51.618799000 DEBUG Failed to discover ty's environment: Invalid ty environment `/Users/micha/astral/ruff/target/debug/deps/e2e-99930d2b8b0885b2`: does not point to a Python executable or a directory on disk
2025-11-13 18:09:51.618829000 DEBUG No virtual environment found
2025-11-13 18:09:51.618867000 DEBUG Including `.` in `environment.root`
2025-11-13 18:09:51.618914000 DEBUG Adding first-party search path `/private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src`
2025-11-13 18:09:51.618933000 DEBUG Using vendored stdlib
2025-11-13 18:09:51.619642000  INFO Python version: Python 3.14, platform: darwin
2025-11-13 18:09:51.620239000 DEBUG Adding new file root '/private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src' of kind Project
2025-11-13 18:09:51.620319000  WARN Your LSP client doesn't support file watching: You may see stale results when files change outside the editor
2025-11-13 18:09:51.620328000 DEBUG Processing deferred notification `textDocument/didOpen`
2025-11-13 18:09:51.628174000 DEBUG notification{method="textDocument/didOpen"}:apply_changes: Adding file `/private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src/foo.py` to project `src`
2025-11-13 18:09:51.628209000 DEBUG notification{method="textDocument/didOpen"}: Opening file `/private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src/foo.py`
2025-11-13 18:09:51.628219000 DEBUG notification{method="textDocument/didOpen"}: Take open project files
2025-11-13 18:09:51.628255000 DEBUG notification{method="textDocument/didOpen"}:set_open_files{open_files={file(Id(c00))}}: Set open project files (count: 1)
2025-11-13 18:09:51.628411000 DEBUG request{id=2 method="textDocument/diagnostic"}: source text: def foo() -> str:
    return 42

2025-11-13 18:09:51.631450000  INFO request{id=2 method="textDocument/diagnostic"}:check_file{file=file(Id(c00))}:Project::index_files{project=src}: Indexed 1 file(s) in 0.003s
2025-11-13 18:09:51.631640000 DEBUG request{id=2 method="textDocument/diagnostic"}:check_file{file=file(Id(c00))}: Checking file '/private/var/folders/7s/n00zmc2d0qgchs1ns43xq47h0000gn/T/.tmpdi61VF/src/foo.py'
2025-11-13 18:09:51.716922000 DEBUG Received server response for request 2
2025-11-13 18:09:51.754282000 DEBUG Client sends request `shutdown` with ID 3
2025-11-13 18:09:51.754405000 DEBUG request{id=3 method="shutdown"}: Received shutdown request, waiting for exit notification
2025-11-13 18:09:51.754460000 DEBUG Received server response for request 3
2025-11-13 18:09:51.754474000 DEBUG Client sends notification `exit`
2025-11-13 18:09:51.754491000 DEBUG Received exit notification, exiting
test pull_diagnostics::on_did_open ... ok
```


---

_Review requested from @carljm by @MichaReiser on 2025-11-13 17:09_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-13 17:09_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-13 17:09_

---

_Label `testing` added by @MichaReiser on 2025-11-13 17:09_

---

_Label `ty` added by @MichaReiser on 2025-11-13 17:09_

---

_Label `testing` added by @MichaReiser on 2025-11-13 17:09_

---

_Label `ty` added by @MichaReiser on 2025-11-13 17:09_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-13 17:12_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 17:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 17:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Merged by @MichaReiser on 2025-11-13 17:29_

---

_Closed by @MichaReiser on 2025-11-13 17:29_

---

_Branch deleted on 2025-11-13 17:29_

---

_Comment by @dhruvmanila on 2025-11-14 05:16_

> (it seems to take the dynamic registration path but it's unclear to me WHY?).

Where do you see that it's taking the dynamic registration path? I don't see the relevant logs in https://github.com/astral-sh/ruff/actions/runs/19290646588/job/55160349507?pr=21363#step:7:5873

---

_Comment by @MichaReiser on 2025-11-14 07:46_

I can't say for certain but looking at the unclaimed response

```
    Unclaimed responses: {
        RequestId(
            I32(
                2,
            ),
        ): Response {
            id: RequestId(
                I32(
                    2,
                ),
            ),
            result: Some(
                Object {
                    "items": Array [],
                    "kind": String("full"),
                },
            ),
            error: None,
        },
    }
```

I assumed that it is the register capability request for diagnostics. But taking a second look today I think this isn't the case because a) that would be a request and b) it would include the capability name. 

I guess I don't know what it is but the new logging should help us to understand what the request with id 2 was.

---

_Comment by @dhruvmanila on 2025-11-14 11:14_

> I guess I don't know what it is but the new logging should help us to understand what the request with id 2 was.

It was `textDocument/diagnostic` as per the logs (request id 2):

```
2025-11-12 08:18:28.294415000  INFO request{id=2 method="textDocument/diagnostic"}:check_file{file=file(Id(c00))}:Project::index_files{project=src}: Indexed 1 file(s) in 0.011s
```

---

_Comment by @MichaReiser on 2025-11-14 11:23_

That's even weirder... Because we explicitly await that response. Unless the response takes too long, in which case we should panic?

Edit: Oh no, we don't. We return an error, but that means that `std::thread::panicking` won't be set, meaning we swallow the real error - the response taking too long

---

_Comment by @MichaReiser on 2025-11-14 11:30_

I think this is a more general issue with how we use `Result` in the `TestServer` because it's almost guaranteed that we have unprocessed responses if any test exits early because of a receive error. 

I think we should remove the assertion that all messages were consumed in the drop handler and instead expose a method so that test can perform this assertion when they want to. 

The one assertion that we probably want to keep is that the server never sends a response **after** exit 

---
