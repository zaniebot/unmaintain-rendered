```yaml
number: 282
title: "ty does not respect workspace-level `experimental.completions.enable`"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - server
  - completions
assignees: []
created_at: 2025-05-08T18:18:16Z
updated_at: 2025-10-03T12:20:21Z
url: https://github.com/astral-sh/ty/issues/282
synced_at: 2026-01-12T15:54:22Z
```

# ty does not respect workspace-level `experimental.completions.enable`

---

_@InSyncWithFoo_

Steps to reproduce:

* Install ty's VS Code extension.
* In the workspace-level `.vscode/settings.json`, set `ty.experimental.completions.enable` to `true`.
* Try to trigger completion in a Python file.

Here's the relevant part of the `initialize` request:

```json5
{
    ...,
    "initializationOptions": {
        "settings": [
            { ..., "experimental": { "completions": { "enable": true } } }
        ],
        "globalSettings": { ... }
    },
	...
}
```

And here's the full response (note that there's no `completionProvider`):

```json5
Result: {
    "capabilities": {
        "diagnosticProvider": {
            "identifier": "ty",
            "interFileDependencies": true,
            "workspaceDiagnostics": false
        },
        "hoverProvider": true,
        "inlayHintProvider": {},
        "positionEncoding": "utf-16",
        "textDocumentSync": {
            "change": 2,
            "openClose": true
        },
        "typeDefinitionProvider": true
    },
    "serverInfo": {
        "name": "ty",
        "version": "0.0.0"
    }
}
```

This happens because ty is hardcoded to [check for this setting under `globalSettings`](https://github.com/astral-sh/ruff/blob/981bd70d393fe29324c07c3ffce5ecddd40e6bd6/crates/ty_server/src/server.rs#L57-L58):

```rust
let server_capabilities =
    Self::server_capabilities(position_encoding, global_settings.experimental.as_ref());
```

---

_Comment by @InSyncWithFoo on 2025-05-08 18:19_

I'd even argue that the `settings`/`globalSettings` structure is specific to VS Code and ty should not assume it, if only for the sake of other clients.

---

_Label `server` added by @AlexWaygood on 2025-05-08 19:04_

---

_Comment by @charliermarsh on 2025-05-08 19:22_

What do you think we should change with the settings structure?

---

_Comment by @InSyncWithFoo on 2025-05-08 22:10_

I think ty should use [`workspace/configuration`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_configuration) instead. `initializationOptions` has [vague semantics](https://github.com/microsoft/language-server-protocol/issues/567) and is therefore [not recommended](https://github.com/microsoft/language-server-protocol/issues/567#issuecomment-448538082). For reference, Pyright does this.

---

_Comment by @dhruvmanila on 2025-05-14 16:10_

> I think ty should use [`workspace/configuration`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_configuration) instead. `initializationOptions` has [vague semantics](https://github.com/microsoft/language-server-protocol/issues/567) and is therefore [not recommended](https://github.com/microsoft/language-server-protocol/issues/567#issuecomment-448538082).

Yeah, I've been thinking about this lately. I agree that the server should pull the configuration instead of relying on initialization options. This would be especially important with ty because currently the [VS Code extension restarts the server](https://github.com/astral-sh/ty-vscode/blob/f4c1f5707e5965533a2239772d6e8dbf954a5475/src/extension.ts#L99-L103) every time a client config is updated and that would mean re-indexing the project which could be expensive for large projects.

---

_Comment by @dhruvmanila on 2025-05-14 16:22_

Also, currently, the ty language server doesn't have support for multiple workspaces so the server only stores settings for a single workspace and cannot handle if some settings are configured at a workspace level.

---

_Comment by @dhruvmanila on 2025-06-25 09:53_

This setting has been removed in [0.0.1-alpha.11](https://github.com/astral-sh/ty/releases/tag/0.0.1-alpha.11) but the `python.ty.disableLanguageServices` has a similar issue and I'll be taking this up holistically when working on https://github.com/astral-sh/ty/issues/82. So, I'll mark this issue as resolved as there's nothing to be done here.

---

_Closed by @dhruvmanila on 2025-06-25 09:53_

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:20_

---
