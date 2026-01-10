```yaml
number: 1811
title: "ty extension: The ty language server exited with a panic"
type: issue
state: open
author: daltunay
labels:
  - needs-info
  - server
  - fatal
assignees: []
created_at: 2025-12-08T22:15:50Z
updated_at: 2025-12-31T15:40:37Z
url: https://github.com/astral-sh/ty/issues/1811
synced_at: 2026-01-10T01:56:41Z
```

# ty extension: The ty language server exited with a panic

---

_Issue opened by @daltunay on 2025-12-08 22:15_

First, thank you all for uv, ruff and ty!

### Summary

While working with either Cursor or new [Antigravity IDE](https://antigravity.google/), I often get the following error:
> ```
> The ty language server exited with a panic. See the logs for more details.
> ```

I am not sure exactly what's triggering it, but I feel like this happens while the agent is modifying files.

Here are the logs of the last time it happened to me:

<details><summary>ty output - 1</summary>
<p>

```
2025-12-08 13:52:12.100145000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/pipeline.py` took more than 100ms (109.510792ms)
2025-12-08 13:52:15.179335000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/utils/edi/x12/utils/datetime.py` took more than 100ms (142.548792ms)
2025-12-08 13:53:07.532756000  INFO Defaulting to python-platform `darwin`
2025-12-08 13:53:07.534350000  INFO Python version: Python 3.11, platform: darwin
2025-12-08 13:53:07.564521000  INFO Indexed 3969 file(s) in 0.025s
2025-12-08 13:53:08.809034000  INFO Defaulting to python-platform `darwin`
2025-12-08 13:53:08.812397000  INFO Python version: Python 3.11, platform: darwin
2025-12-08 13:53:09.016086000  INFO Indexed 3969 file(s) in 0.083s
2025-12-08 13:53:13.982949000  INFO Defaulting to python-platform `darwin`
2025-12-08 13:53:13.983778000  INFO Python version: Python 3.11, platform: darwin
2025-12-08 13:53:14.006984000  INFO Indexed 3969 file(s) in 0.021s
2025-12-08 13:53:51.322352000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/invoice_manager/services/invoice_activity_event_service.py` took more than 100ms (194.254625ms)
2025-12-08 13:53:51.565877000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/users/migrations/0141_alter_billing_disputable_amount_cents_limit_squashed_0209_companysettings_isolated_data_alter_company_settings.py` took more than 100ms (137.03375ms)
2025-12-08 13:53:51.582140000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/utils/edi/x12/utils/datetime.py` took more than 100ms (168.828458ms)
2025-12-08 13:53:51.605898000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/common/factory_helpers.py` took more than 100ms (184.362833ms)
2025-12-08 13:54:03.909385000  INFO Defaulting to python-platform `darwin`
2025-12-08 13:54:03.910012000  INFO Python version: Python 3.11, platform: darwin
2025-12-08 13:54:03.935289000  INFO Indexed 3969 file(s) in 0.024s
2025-12-08 13:54:08.456916000  INFO Defaulting to python-platform `darwin`
2025-12-08 13:54:08.458341000  INFO Python version: Python 3.11, platform: darwin
2025-12-08 13:54:08.484814000  INFO Indexed 3969 file(s) in 0.025s
2025-12-08 13:54:39.585661000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/users/constants.py` took more than 100ms (3.480041542s)
2025-12-08 13:54:54.752225000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/pipeline.py` took more than 100ms (158.447083ms)
2025-12-08 13:55:11.960075000  WARN Ignoring request id=71673 method=textDocument/diagnostic because Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/pipeline.py is not open in the session
[Error - 1:55:11 PM] Request textDocument/diagnostic failed.
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/pipeline.py is not open in the session
  Code: -32602 
[Error - 1:55:11 PM] Document pull failed for text document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/pipeline.py
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/pipeline.py is not open in the session
  Code: -32602 
2025-12-08 13:55:19.917146000  WARN Ignoring request id=71690 method=textDocument/diagnostic because Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/create_pydantic_dataset.py is not open in the session
[Error - 1:55:19 PM] Request textDocument/diagnostic failed.
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/create_pydantic_dataset.py is not open in the session
  Code: -32602 
[Error - 1:55:19 PM] Document pull failed for text document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/create_pydantic_dataset.py
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/create_pydantic_dataset.py is not open in the session
  Code: -32602 
2025-12-08 13:55:21.174144000  WARN Ignoring request id=71692 method=textDocument/diagnostic because Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/__init__.py is not open in the session
[Error - 1:55:21 PM] Request textDocument/diagnostic failed.
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/__init__.py is not open in the session
  Code: -32602 
[Error - 1:55:21 PM] Document pull failed for text document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/__init__.py
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/__init__.py is not open in the session
  Code: -32602 
2025-12-08 13:55:23.279805000  WARN Ignoring request id=71694 method=textDocument/diagnostic because Document file:///Users/daltunay/GitHub/bc_core_api/ai/management/commands/evaluate_structured_parsing.py is not open in the session
[Error - 1:55:23 PM] Request textDocument/diagnostic failed.
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/management/commands/evaluate_structured_parsing.py is not open in the session
  Code: -32602 
[Error - 1:55:23 PM] Document pull failed for text document file:///Users/daltunay/GitHub/bc_core_api/ai/management/commands/evaluate_structured_parsing.py
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/management/commands/evaluate_structured_parsing.py is not open in the session
  Code: -32602 
2025-12-08 13:55:26.263357000  INFO Defaulting to python-platform `darwin`
2025-12-08 13:55:26.263777000  INFO Python version: Python 3.11, platform: darwin
2025-12-08 13:55:26.352881000  INFO Indexed 3969 file(s) in 0.088s
2025-12-08 13:55:31.261106000  INFO Defaulting to python-platform `darwin`
2025-12-08 13:55:31.261812000  INFO Python version: Python 3.11, platform: darwin
2025-12-08 13:55:31.279671000  INFO Indexed 3969 file(s) in 0.017s
2025-12-08 13:56:34.962746000  WARN Ignoring request id=71769 method=textDocument/diagnostic because Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/models.py is not open in the session
[Error - 1:56:34 PM] Request textDocument/diagnostic failed.
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/models.py is not open in the session
  Code: -32602 
[Error - 1:56:34 PM] Document pull failed for text document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/models.py
  Message: Document file:///Users/daltunay/GitHub/bc_core_api/ai/structured_invoice_parsing/evaluation/models.py is not open in the session
  Code: -32602 
2025-12-08 13:57:59.017986000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/invoices/parsers/edi_x12_parser.py` took more than 100ms (113.07975ms)
2025-12-08 13:58:01.158066000  WARN Propagating panic for cycle head that panicked in an earlier execution in that revision
2025-12-08 13:58:01.158171000  WARN Propagating panic for cycle head that panicked in an earlier execution in that revision
2025-12-08 13:58:03.452983000  WARN Propagating panic for cycle head that panicked in an earlier execution in that revision
2025-12-08 13:58:15.043033000  INFO Checking file `/Users/daltunay/GitHub/bc_core_api/users/constants.py` took more than 100ms (3.245766166s)
2025-12-08 13:58:43.670668000  WARN Propagating panic for cycle head that panicked in an earlier execution in that revision
2025-12-08 13:58:43.671229000  WARN Propagating panic for cycle head that panicked in an earlier execution in that revision
2025-12-08 13:58:46.195606000  WARN Propagating panic for cycle head that panicked in an earlier execution in that revision
2025-12-08 13:58:46.198208000 ERROR panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/59aa107/src/function/backdate.rs:42:17:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __pthread_cond_wait

panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/59aa107/src/function/backdate.rs:42:17:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __pthread_cond_wait

2025-12-08 13:58:46.220974000 ERROR An error occurred with request ID 71982: request handler panicked at:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
[Error - 1:58:46 PM] Request textDocument/diagnostic failed.
  Message: request handler panicked at:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
  Code: -32603 
[Error - 1:58:46 PM] Workspace diagnostic pull failed.
  Message: request handler panicked at:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
  Code: -32603 
2025-12-08 13:58:48.234624000  WARN Propagating panic for cycle head that panicked in an earlier execution in that revision
2025-12-08 13:58:48.235052000 ERROR panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/59aa107/src/function/backdate.rs:42:17:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __pthread_cond_wait

panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/59aa107/src/function/backdate.rs:42:17:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __pthread_cond_wait

2025-12-08 13:58:48.265699000 ERROR An error occurred with request ID 71983: request handler panicked at:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
[Error - 1:58:48 PM] Request textDocument/diagnostic failed.
  Message: request handler panicked at:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
  Code: -32603 
[Error - 1:58:48 PM] Workspace diagnostic pull failed.
  Message: request handler panicked at:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
  Code: -32603 
2025-12-08 13:58:50.276868000  WARN Propagating panic for cycle head that panicked in an earlier execution in that revision
2025-12-08 13:58:50.277268000 ERROR panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/59aa107/src/function/backdate.rs:42:17:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __pthread_cond_wait

panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/59aa107/src/function/backdate.rs:42:17:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __pthread_cond_wait

2025-12-08 13:58:50.306676000 ERROR An error occurred with request ID 71984: request handler panicked at:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
[Error - 1:58:50 PM] Request textDocument/diagnostic failed.
  Message: request handler panicked at:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
  Code: -32603 
[Error - 1:58:50 PM] Workspace diagnostic pull failed.
  Message: request handler panicked at:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_at
  Code: -32603 
```

</p>
</details> 

Also while writing this and doing some code modifications, the language server crashed with another exception:

<details><summary>ty output - 2</summary>
<p>

```
2025-12-08 14:07:57.417468000 ERROR An error occurred with request ID 72818: request handler panicked at crates/ruff_python_ast/src/token/tokens.rs:182:13:
Offset 1689 is inside a token range 1688..1692
run with `RUST_BACKTRACE=1` environment variable to display a backtrace

[Error - 2:07:57 PM] Request textDocument/codeAction failed.
  Message: request handler panicked at crates/ruff_python_ast/src/token/tokens.rs:182:13:
Offset 1689 is inside a token range 1688..1692
run with `RUST_BACKTRACE=1` environment variable to display a backtrace

  Code: -32603 
2025-12-08 14:07:57.751671000 ERROR An error occurred with request ID 72842: request handler panicked at crates/ruff_python_ast/src/token/tokens.rs:182:13:
Offset 1689 is inside a token range 1688..1692
run with `RUST_BACKTRACE=1` environment variable to display a backtrace

[Error - 2:07:57 PM] Request textDocument/codeAction failed.
  Message: request handler panicked at crates/ruff_python_ast/src/token/tokens.rs:182:13:
Offset 1689 is inside a token range 1688..1692
run with `RUST_BACKTRACE=1` environment variable to display a backtrace

  Code: -32603 
```

</p>
</details> 

VS Code extension version: `2025.61.13371357`
ty version: `ty 0.0.1-alpha.32 (84a188116 2025-12-05)`

I am also providing debug information if this can help:

<details><summary>ty debug</summary>
<p>

```
Client capabilities: ResolvedClientCapabilities(
    WORKSPACE_DIAGNOSTIC_REFRESH | INLAY_HINT_REFRESH | PULL_DIAGNOSTICS | TYPE_DEFINITION_LINK_SUPPORT | DEFINITION_LINK_SUPPORT | DECLARATION_LINK_SUPPORT | PREFER_MARKDOWN_IN_HOVER | SIGNATURE_LABEL_OFFSET_SUPPORT | SIGNATURE_ACTIVE_PARAMETER_SUPPORT | HIERARCHICAL_DOCUMENT_SYMBOL_SUPPORT | WORK_DONE_PROGRESS | FILE_WATCHER_SUPPORT | RELATIVE_FILE_WATCHER_SUPPORT | DIAGNOSTIC_DYNAMIC_REGISTRATION | WORKSPACE_CONFIGURATION | RENAME_DYNAMIC_REGISTRATION | COMPLETION_ITEM_LABEL_DETAILS_SUPPORT,
)
Position encoding: UTF16
Global settings: GlobalSettings {
    diagnostic_mode: Workspace,
    experimental: ExperimentalSettings {
        rename: true,
        auto_import: true,
    },
}
Open text documents: 5

Workspace /Users/daltunay/GitHub/bc_core_api (file:///Users/daltunay/GitHub/bc_core_api)
Settings: WorkspaceSettings {
    disable_language_services: false,
    inlay_hints: InlayHintSettings {
        variable_types: true,
        call_argument_names: true,
    },
    overrides: Some(
        ProjectOptionsOverrides {
            config_file_override: None,
            fallback_python_version: Some(
                PythonVersion {
                    major: 3,
                    minor: 11,
                },
            ),
            fallback_python: Some(
                RelativePathBuf(
                    "/Users/daltunay/GitHub/bc_core_api/.venv",
                ),
            ),
            options: Options {
                environment: None,
                src: None,
                rules: None,
                terminal: None,
                overrides: None,
            },
        },
    ),
}

Project at /Users/daltunay/GitHub/bc_core_api
Settings: Settings {
    rules: {
        "ambiguous-protocol-member": Warning (Default),
        "byte-string-type-annotation": Error (Default),
        "call-non-callable": Error (Default),
        "conflicting-argument-forms": Error (Default),
        "conflicting-declarations": Error (Default),
        "conflicting-metaclass": Error (Default),
        "cyclic-class-definition": Error (Default),
        "cyclic-type-alias-definition": Error (Default),
        "deprecated": Warning (Default),
        "duplicate-base": Error (Default),
        "duplicate-kw-only": Error (Default),
        "escape-character-in-forward-annotation": Error (Default),
        "fstring-type-annotation": Error (Default),
        "ignore-comment-unknown-rule": Warning (Default),
        "implicit-concatenated-string-type-annotation": Error (Default),
        "inconsistent-mro": Error (Default),
        "index-out-of-bounds": Error (Default),
        "instance-layout-conflict": Error (Default),
        "invalid-argument-type": Error (Default),
        "invalid-assignment": Error (Default),
        "invalid-attribute-access": Error (Default),
        "invalid-await": Error (Default),
        "invalid-base": Error (Default),
        "invalid-context-manager": Error (Default),
        "invalid-declaration": Error (Default),
        "invalid-exception-caught": Error (Default),
        "invalid-explicit-override": Error (Default),
        "invalid-generic-class": Error (Default),
        "invalid-ignore-comment": Warning (Default),
        "invalid-key": Error (Default),
        "invalid-legacy-type-variable": Error (Default),
        "invalid-metaclass": Error (Default),
        "invalid-method-override": Error (Default),
        "invalid-named-tuple": Error (Default),
        "invalid-newtype": Error (Default),
        "invalid-overload": Error (Default),
        "invalid-parameter-default": Error (Default),
        "invalid-paramspec": Error (Default),
        "invalid-protocol": Error (Default),
        "invalid-raise": Error (Default),
        "invalid-return-type": Error (Default),
        "invalid-super-argument": Error (Default),
        "invalid-syntax-in-forward-annotation": Error (Default),
        "invalid-type-alias-type": Error (Default),
        "invalid-type-arguments": Error (Default),
        "invalid-type-checking-constant": Error (Default),
        "invalid-type-form": Error (Default),
        "invalid-type-guard-call": Error (Default),
        "invalid-type-guard-definition": Error (Default),
        "invalid-type-variable-constraints": Error (Default),
        "missing-argument": Error (Default),
        "missing-typed-dict-key": Error (Default),
        "no-matching-overload": Error (Default),
        "non-subscriptable": Error (Default),
        "not-iterable": Error (Default),
        "override-of-final-method": Error (Default),
        "parameter-already-assigned": Error (Default),
        "positional-only-parameter-as-kwarg": Error (Default),
        "possibly-missing-attribute": Warning (Default),
        "possibly-missing-implicit-call": Warning (Default),
        "possibly-missing-import": Warning (Default),
        "raw-string-type-annotation": Error (Default),
        "redundant-cast": Warning (Default),
        "static-assert-error": Error (Default),
        "subclass-of-final-class": Error (Default),
        "super-call-in-named-tuple-method": Error (Default),
        "too-many-positional-arguments": Error (Default),
        "type-assertion-failure": Error (Default),
        "unavailable-implicit-super-arguments": Error (Default),
        "undefined-reveal": Warning (Default),
        "unknown-argument": Error (Default),
        "unresolved-attribute": Error (Default),
        "unresolved-global": Warning (Default),
        "unresolved-import": Error (Default),
        "unresolved-reference": Error (Default),
        "unsupported-base": Warning (Default),
        "unsupported-bool-conversion": Error (Default),
        "unsupported-operator": Error (Default),
        "useless-overload-body": Warning (Default),
        "zero-stepsize-in-slice": Error (Default),
    },
    terminal: TerminalSettings {
        output_format: Full,
        error_on_warning: false,
    },
    src: SrcSettings {
        respect_ignore_files: true,
        files: IncludeExcludeFilter {
            include: IncludeFilter(
                [
                    "**",
                ],
                ..
            ),
            exclude: ExcludeFilter {
                ignore: Gitignore(
                    [
                        IgnoreGlob {
                            original: "**/.bzr/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.direnv/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.eggs/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.git/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.git-rewrite/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.hg/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.mypy_cache/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.nox/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.pants.d/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.pytype/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.ruff_cache/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.svn/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.tox/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/.venv/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/__pypackages__/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/_build/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/buck-out/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/dist/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/node_modules/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                        IgnoreGlob {
                            original: "**/venv/",
                            is_allow: false,
                            is_only_dir: true,
                        },
                    ],
                    ..
                ),
            },
        },
    },
    overrides: [],
}

Memory report:
=======SALSA STRUCTS=======
`Definition`                                       metadata=19.61MB  fields=27.45MB  count=490263
`File`                                             metadata=4.29MB   fields=12.68MB  count=66988
`IntersectionType`                                 metadata=5.12MB   fields=12.01MB  count=91349
`FunctionType`                                     metadata=2.17MB   fields=10.37MB  count=38676
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=9.27MB   fields=7.94MB   count=165453
`Type < 'db >::is_redundant_with_::interned_arguments` metadata=13.40MB  fields=7.66MB   count=239308
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=8.52MB   fields=7.31MB   count=152200
`ClassLiteral < 'db >::implicit_attribute_inner_::interned_arguments` metadata=9.14MB   fields=6.53MB   count=163135
`UnionType`                                        metadata=5.28MB   fields=5.74MB   count=94251
`Expression`                                       metadata=11.24MB  fields=5.62MB   count=234096
`CallableType`                                     metadata=1.37MB   fields=5.04MB   count=24542
`Type < 'db >::apply_specialization_::interned_arguments` metadata=9.06MB   fields=3.88MB   count=161855
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=4.31MB   fields=3.69MB   count=76932
`Specialization`                                   metadata=3.38MB   fields=3.58MB   count=60409
`StringLiteralType`                                metadata=2.59MB   fields=2.56MB   count=46317
`FileModule`                                       metadata=1.84MB   fields=2.52MB   count=32775
`TupleType`                                        metadata=1.13MB   fields=1.65MB   count=20203
`OverloadLiteral`                                  metadata=1.31MB   fields=1.62MB   count=23408
`place_by_id::interned_arguments`                  metadata=3.73MB   fields=1.43MB   count=71739
`ScopeId`                                          metadata=2.50MB   fields=1.07MB   count=89254
`BoundMethodType`                                  metadata=2.03MB   fields=0.87MB   count=36313
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=2.61MB   fields=0.75MB   count=46571
`GenericAlias`                                     metadata=2.60MB   fields=0.74MB   count=46362
`ClassLiteral`                                     metadata=0.49MB   fields=0.61MB   count=8681
`GenericContext`                                   metadata=0.36MB   fields=0.53MB   count=6343
`BoundTypeVarInstance`                             metadata=1.74MB   fields=0.50MB   count=31023
`ProtocolInterface`                                metadata=0.18MB   fields=0.40MB   count=3176
`code_generator_of_class::interned_arguments`      metadata=1.34MB   fields=0.38MB   count=23956
`ModuleLiteralType`                                metadata=0.94MB   fields=0.36MB   count=18042
`TypeVarInstance`                                  metadata=0.35MB   fields=0.30MB   count=6214
`Unpack`                                           metadata=0.26MB   fields=0.21MB   count=6460
`ModuleNameIngredient`                             metadata=0.20MB   fields=0.19MB   count=3567
`FunctionLiteral`                                  metadata=1.32MB   fields=0.19MB   count=23546
`StarImportPlaceholderPredicate`                   metadata=0.26MB   fields=0.18MB   count=9225
`TypeVarIdentity`                                  metadata=0.20MB   fields=0.15MB   count=3633
`ExpressionWithContext`                            metadata=0.28MB   fields=0.12MB   count=5053
`UnionTypeInstance`                                metadata=0.05MB   fields=0.08MB   count=849
`EnumLiteralType`                                  metadata=0.12MB   fields=0.08MB   count=2186
`Project`                                          metadata=0.00MB   fields=0.06MB   count=1
`PatternPredicate`                                 metadata=0.02MB   fields=0.05MB   count=561
`PropertyInstanceType`                             metadata=0.06MB   fields=0.03MB   count=1013
`lookup_dunder_new_inner::interned_arguments`      metadata=0.11MB   fields=0.03MB   count=2002
`BoundSuperType`                                   metadata=0.04MB   fields=0.02MB   count=717
`is_equivalent_to_object_inner::interned_arguments` metadata=0.09MB   fields=0.02MB   count=1676
`Program`                                          metadata=0.00MB   fields=0.02MB   count=1
`ClassLiteral < 'db >::fields_::interned_arguments` metadata=0.03MB   fields=0.01MB   count=500
`FieldInstance`                                    metadata=0.01MB   fields=0.01MB   count=231
`InternedType`                                     metadata=0.01MB   fields=0.00MB   count=202
`BytesLiteralType`                                 metadata=0.00MB   fields=0.00MB   count=51
`TypeIsType`                                       metadata=0.00MB   fields=0.00MB   count=33
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=40
`NewType`                                          metadata=0.00MB   fields=0.00MB   count=20
`DataclassParams`                                  metadata=0.00MB   fields=0.00MB   count=19
`ClassLiteral < 'db >::variance_of_::interned_arguments` metadata=0.00MB   fields=0.00MB   count=44
`GenericAlias < 'db >::variance_of_::interned_arguments` metadata=0.00MB   fields=0.00MB   count=19
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=2
`DataclassTransformerParams`                       metadata=0.00MB   fields=0.00MB   count=5
`ManualPEP695TypeAliasType`                        metadata=0.00MB   fields=0.00MB   count=4
`DeprecatedInstance`                               metadata=0.00MB   fields=0.00MB   count=16
`SearchPathIngredient`                             metadata=0.00MB   fields=0.00MB   count=4
`ModuleResolveModeIngredient`                      metadata=0.00MB   fields=0.00MB   count=3
`list_modules::interned_arguments`                 metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
=======SALSA QUERIES=======
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex<'_>`
    metadata=20.37MB  fields=425.36MB count=5349
`source_text -> ruff_db::source::SourceText`
    metadata=1.51MB   fields=198.66MB count=23783
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature<'_>`
    metadata=8.16MB   fields=88.11MB  count=31147
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=85.00MB  fields=57.39MB  count=197488
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference<'_>`
    metadata=24.64MB  fields=53.83MB  count=32740
`infer_expression_types_impl -> ty_python_semantic::types::infer::ExpressionInference<'_>`
    metadata=76.21MB  fields=34.79MB  count=145105
`symbols_for_file_global_only -> ty_ide::symbols::FlatSymbols`
    metadata=2.37MB   fields=11.56MB  count=22961
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.87MB   fields=6.69MB   count=3972
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro<'_>, ty_python_semantic::types::mro::MroError<'_>>`
    metadata=11.14MB  fields=6.45MB   count=46571
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=42.49MB  fields=5.29MB   count=165453
`ClassLiteral < 'db >::implicit_attribute_inner_ -> ty_python_semantic::types::member::Member<'_>`
    metadata=17.52MB  fields=5.22MB   count=163135
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=40.95MB  fields=4.87MB   count=152200
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=0.20MB   fields=4.53MB   count=3774
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference<'_>`
    metadata=4.28MB   fields=3.24MB   count=6090
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type<'_>`
    metadata=15.20MB  fields=2.59MB   count=161855
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers<'_>`
    metadata=6.72MB   fields=2.30MB   count=71739
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=15.78MB  fields=2.05MB   count=25467
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type<'_>, ty_python_semantic::types::AttributeKind)>`
    metadata=38.26MB  fields=1.85MB   count=76930
`FunctionType < 'db >::last_definition_raw_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=1.26MB   fields=1.81MB   count=8729
`all_submodule_names_for_package -> core::option::Option<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>>`
    metadata=2.27MB   fields=1.32MB   count=32469
`overloads_and_implementation_inner -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral<'_>]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral<'_>>)`
    metadata=2.64MB   fields=1.23MB   count=22575
`FunctionType < 'db >::last_definition_signature_ -> ty_python_semantic::types::signatures::Signature<'_>`
    metadata=0.85MB   fields=1.05MB   count=4204
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=4.42MB   fields=0.99MB   count=11360
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata<'_>>`
    metadata=3.33MB   fields=0.66MB   count=7450
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.25MB   fields=0.59MB   count=3969
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult<'_>`
    metadata=1.02MB   fields=0.59MB   count=3255
`infer_expression_type_impl -> ty_python_semantic::types::Type<'_>`
    metadata=2.97MB   fields=0.56MB   count=35279
`ClassLiteral < 'db >::fields_ -> indexmap::map::IndexMap<ruff_python_ast::name::Name, ty_python_semantic::types::class::Field<'_>, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>`
    metadata=0.30MB   fields=0.44MB   count=500
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.19MB   fields=0.43MB   count=3649
`inferable_typevars_inner -> std::collections::hash::set::HashSet<ty_python_semantic::types::BoundTypeVarIdentity<'_>, rustc_hash::FxBuildHasher>`
    metadata=0.25MB   fields=0.42MB   count=4741
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type<'_>, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams<'_>>), ty_python_semantic::types::class::MetaclassError<'_>>`
    metadata=1.73MB   fields=0.38MB   count=7968
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=1.81MB   fields=0.38MB   count=23783
`code_generator_of_class -> core::option::Option<ty_python_semantic::types::class::CodeGeneratorKind<'_>>`
    metadata=3.31MB   fields=0.29MB   count=23956
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.72MB   fields=0.26MB   count=8601
`Type < 'db >::is_redundant_with_ -> bool`
    metadata=23.96MB  fields=0.24MB   count=239308
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.02MB   fields=0.23MB   count=269
`use_def_map -> alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap<'_>>`
    metadata=0.62MB   fields=0.19MB   count=11843
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.99MB   fields=0.17MB   count=19096
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type<'_>]>`
    metadata=0.55MB   fields=0.14MB   count=7511
`BoundMethodType < 'db >::into_callable_type_ -> ty_python_semantic::types::CallableType<'_>`
    metadata=1.53MB   fields=0.09MB   count=11607
`TupleType < 'db >::to_class_type_ -> ty_python_semantic::types::class::ClassType<'_>`
    metadata=1.96MB   fields=0.08MB   count=6342
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.69MB   fields=0.07MB   count=8598
`ClassLiteral < 'db >::inherited_legacy_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext<'_>>`
    metadata=0.67MB   fields=0.07MB   count=8322
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.02MB   fields=0.06MB   count=173
`lookup_dunder_new_inner -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers<'_>>`
    metadata=0.94MB   fields=0.06MB   count=2002
`symbols_for_file -> ty_ide::symbols::FlatSymbols`
    metadata=0.00MB   fields=0.06MB   count=18
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=1.72MB   fields=0.04MB   count=3567
`all_negative_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.08MB   fields=0.04MB   count=395
`all_narrowing_constraints_for_pattern -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type<'_>, rustc_hash::FxBuildHasher>>`
    metadata=0.29MB   fields=0.03MB   count=391
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.29MB   fields=0.03MB   count=3969
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId<'_>`
    metadata=0.20MB   fields=0.03MB   count=3919
`static_expression_truthiness -> ty_python_semantic::types::Truthiness`
    metadata=2.86MB   fields=0.03MB   count=30068
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface<'_>`
    metadata=0.81MB   fields=0.02MB   count=2617
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.98MB   fields=0.01MB   count=8476
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.07MB   fields=0.01MB   count=631
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.74MB   fields=0.01MB   count=7402
`TypeVarInstance < 'db >::lazy_bound_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.02MB   fields=0.00MB   count=199
`ClassType < 'db >::into_callable_ -> ty_python_semantic::types::CallableTypes<'_>`
    metadata=0.02MB   fields=0.00MB   count=70
`is_equivalent_to_object_inner -> bool`
    metadata=0.50MB   fields=0.00MB   count=1676
`TypeVarInstance < 'db >::lazy_default_ -> core::option::Option<ty_python_semantic::types::Type<'_>>`
    metadata=0.01MB   fields=0.00MB   count=65
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`TypeVarInstance < 'db >::lazy_constraints_ -> core::option::Option<ty_python_semantic::types::TypeVarBoundOrConstraints<'_>>`
    metadata=0.00MB   fields=0.00MB   count=22
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=3
`NewType < 'db >::lazy_base_ -> ty_python_semantic::types::newtype::NewTypeBase<'_>`
    metadata=0.00MB   fields=0.00MB   count=11
`list_modules_in -> alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.03MB   fields=0.00MB   count=4
`ClassLiteral < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.01MB   fields=0.00MB   count=44
`list_modules -> alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module<'_>>`
    metadata=0.00MB   fields=0.00MB   count=1
`GenericAlias < 'db >::variance_of_ -> ty_python_semantic::types::variance::TypeVarVariance`
    metadata=0.00MB   fields=0.00MB   count=19
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 1679.69MB
    struct metadata = 134.94MB
    struct fields = 137.25MB
    memo metadata = 479.59MB
    memo fields = 927.91MB
```

</p>
</details> 

It does not prevent me from using ty as it restarts right after the crash.

PS: this has been happening for as long as I remember using ty extension (~ 2 months I believe)

### Version

ty 0.0.1-alpha.32 (84a188116 2025-12-05)

---

_Comment by @MichaReiser on 2025-12-09 11:17_

Thanks for the detailed report. 

The first issue is the same as https://github.com/astral-sh/ty/issues/1565 

I'm surprised by the second one as this is something that should have been fixed in 31 or 32 (don't remember exactly. Any chance you could share the code of the file you were editing when the panic occurred (or the code around the byte offsets  1688..1692)?

---

_Label `bug` added by @MichaReiser on 2025-12-09 11:17_

---

_Label `server` added by @MichaReiser on 2025-12-09 11:17_

---

_Label `bug` removed by @MichaReiser on 2025-12-09 11:17_

---

_Label `fatal` added by @MichaReiser on 2025-12-09 11:17_

---

_Label `needs-info` added by @MichaReiser on 2025-12-31 15:40_

---

_Comment by @MichaReiser on 2025-12-31 15:40_

Have you run into the second issue again? I'm asking because i haven't been able to reproduce it

---
