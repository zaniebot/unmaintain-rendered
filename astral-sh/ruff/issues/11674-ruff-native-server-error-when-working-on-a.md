```yaml
number: 11674
title: ruff native server error when working on a untitled buffer with VsCode
type: issue
state: closed
author: rainzee
labels:
  - server
assignees: []
created_at: 2024-06-01T05:18:03Z
updated_at: 2024-06-13T06:09:37Z
url: https://github.com/astral-sh/ruff/issues/11674
synced_at: 2026-01-10T11:09:53Z
```

# ruff native server error when working on a untitled buffer with VsCode

---

_Issue opened by @rainzee on 2024-06-01 05:18_

### Abstract

When working in an untitled editor buffer with a local native Ruff server turned on, it consistently leads to an error.

### Steps to Reproduction

To reproduce, create a new untitled editor buffer in Visual Studio Code (VSCode), choose the language Python, and write any Python code. This will result in the following error message: `Ruff encountered a problem. Check the logs for more details.`

```
1742.246534s ERROR ruff_server::server::api An error occurred while running textDocument/didChange: Tried to open unavailable document `untitled:Untitled-1`

2024-06-01 13:04:50.434 [info] [Trace - 13:04:50] Received notification 'window/showMessage'.
2024-06-01 13:04:50.436 [info] [Trace - 13:04:50] Sending notification '$/cancelRequest'.
2024-06-01 13:04:50.437 [info] ┐ruff_server::server::api::notifications::cancel::run{}
┘

2024-06-01 13:04:50.698 [info] [Trace - 13:04:50] Sending request 'textDocument/codeAction - (40)'.
2024-06-01 13:04:50.699 [info] 1742.511805s WARN ruff_server::session::workspace Workspace not found for untitled:Untitled-1. Global settings will be used for this document

2024-06-01 13:04:50.960 [info] [Trace - 13:04:50] Sending request 'textDocument/codeAction - (41)'.
2024-06-01 13:04:50.961 [info] [Trace - 13:04:50] Sending notification '$/cancelRequest'.
2024-06-01 13:04:50.962 [info] 1742.774422s WARN ruff_server::session::workspace Workspace not found for untitled:Untitled-1. Global settings will be used for this document

2024-06-01 13:04:50.962 [info] ┐ruff_server::server::api::notifications::cancel::run{}
┘
```

### Reference

This issue was originally provided by @yanyongyu and has been consistently reproduced in my environment as well. Additionally, there was a related issue report #9870, which has been the last hurdle in our workflow. Unfortunately, that issue has been on hold for a while. Could you please investigate further or let us know if more information is needed?

Thanks for such great work!


---

_Comment by @dhruvmanila on 2024-06-01 06:17_

This has been fixed in `0.4.7` which was released around 8 hours ago. Can you upgrade `ruff` and try again? The VS Code version was updated as well which now bundles with Ruff `0.4.7`.

---

_Comment by @rainzee on 2024-06-01 08:21_

> This has been fixed in `0.4.7` which was released around 8 hours ago. Can you upgrade `ruff` and try again? The VS Code version was updated as well which now bundles with Ruff `0.4.7`.

This is test in 0.4.7

---

_Label `server` added by @MichaReiser on 2024-06-01 08:23_

---

_Comment by @MichaReiser on 2024-06-01 08:33_

Hmm, I can't reproduce this in the latest version with the given reproduction steps:

That's the logs that I get

```
2024-06-01 10:30:17.442 [info] [Trace - 10:30:17 AM] Sending notification 'textDocument/didOpen'.
2024-06-01 10:30:17.442 [info] [Trace - 10:30:17 AM] Sending request 'textDocument/diagnostic - (4)'.
2024-06-01 10:30:17.442 [info]    7.962899s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-1'.
   7.962910s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-1'.

2024-06-01 10:30:17.443 [info] [Trace - 10:30:17 AM] Sending request 'textDocument/codeAction - (5)'.
2024-06-01 10:30:17.443 [info] [Trace - 10:30:17 AM] Received response 'textDocument/diagnostic - (4)' in 1ms.
2024-06-01 10:30:17.444 [info]    7.964077s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-1'.
   7.964090s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-1'.

2024-06-01 10:30:17.444 [info] [Trace - 10:30:17 AM] Received response 'textDocument/codeAction - (5)' in 1ms.
```

Can you try to find the initialization message in the log output. It should be somewhere at the top:

```
2024-06-01 10:30:09.493 [info] Result: {
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
        "positionEncoding": "utf-16",
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
        "version": "0.4.7"
    }
}

```

---

_Comment by @yanyongyu on 2024-06-02 09:07_

This still happens in ruff extension `2024.24.0` with **native server** config. Note that this only happens when using native server.

@MichaReiser I reload my window and cannot find the initialization message you provide in the log output. How can i find the init message? Here is the full log from init to error:

```log
2024-06-02 17:03:39.548 [info] Name: Ruff
2024-06-02 17:03:39.548 [info] Module: ruff
2024-06-02 17:03:39.548 [info] Python extension loading
2024-06-02 17:03:39.548 [info] Waiting for interpreter from python extension.
2024-06-02 17:03:39.548 [info] Python extension loaded
2024-06-02 17:03:39.548 [info] Server run command: /workspaces/xxxx/.venv/bin/python /home/vscode/.vscode-remote/extensions/charliermarsh.ruff-2024.24.0-linux-x64/bundled/tool/ruff_server.py server --preview
2024-06-02 17:03:39.548 [info] Server: Start requested.
2024-06-02 17:03:39.548 [info] [Trace - 5:03:38 PM] Received request 'client/registerCapability - (1)'.
2024-06-02 17:03:39.548 [info] [Trace - 5:03:38 PM] Sending response 'client/registerCapability - (1)'. Processing request took 1ms
2024-06-02 17:03:39.548 [info]    0.647509s INFO ruff_server::server Configuration file watcher successfully registered

2024-06-02 17:03:39.548 [info]    1.209940s WARN ruff_server::server::api Received notification $/setTrace which does not have a handler.

2024-06-02 17:03:40.901 [info]    2.613193s WARN ruff_server::server::api Received notification $/setTrace which does not have a handler.

2024-06-02 17:03:59.076 [info]   20.796641s WARN ruff_server::server::api Received notification $/setTrace which does not have a handler.

2024-06-02 17:05:03.369 [info]   84.984414s ERROR ruff_server::server::api An error occurred while running textDocument/didOpen: expected document URI untitled:Untitled-1 to be a valid file pat
```

---

_Comment by @rainzee on 2024-06-02 12:12_

```
2024-06-01 12:35:49.134 [info] Name: Ruff
2024-06-01 12:35:49.135 [info] Module: ruff
2024-06-01 12:35:49.135 [info] Python extension loading
2024-06-01 12:35:49.135 [info] Waiting for interpreter from python extension.
2024-06-01 12:35:49.135 [info] Python extension loaded
2024-06-01 12:35:49.135 [info] Server run command: c:\Users\rainzee\Documents\Work In Progress\darenmao\.venv\Scripts\python.exe c:\Users\rainzee\.vscode\extensions\charliermarsh.ruff-2024.24.0-win32-x64\bundled\tool\ruff_server.py server --preview
2024-06-01 12:35:49.135 [info] Server: Start requested.
2024-06-01 12:35:49.170 [info] [Trace - 12:35:49] Sending request 'textDocument/codeAction - (2)'.
2024-06-01 12:35:49.343 [info] ┐ruff_server::server::api::notifications::did_open::run{file=file:///c%3A/Users/rainzee/Documents/Work%20In%20Progress/darenmao/darenmao/app/view/component/interface.py}
┘
```

```
2024-06-01 12:48:53.583 [info] [Trace - 12:48:53] Sending request 'textDocument/diagnostic - (24)'.
2024-06-01 12:48:53.583 [info] [Trace - 12:48:53] Sending request 'textDocument/codeAction - (25)'.
2024-06-01 12:48:53.584 [info]  785.396756s WARN ruff_server::session::workspace Workspace not found for untitled:Untitled-1. Global settings will be used for this document

2024-06-01 12:48:53.584 [info]  785.397018s WARN ruff_server::session::workspace Workspace not found for untitled:Untitled-1. Global settings will be used for this document

2024-06-01 12:48:54.497 [info] [Trace - 12:48:54] Sending notification 'textDocument/didChange'.
2024-06-01 12:48:54.498 [info] [Trace - 12:48:54] Sending notification '$/cancelRequest'.
2024-06-01 12:48:54.498 [info] ┐ruff_server::server::api::notifications::did_change::run{file=untitled:Untitled-1}
┘
 786.310474s ERROR ruff_server::server::api An error occurred while running textDocument/didChange: Tried to open unavailable document `untitled:Untitled-1`

2024-06-01 12:48:54.498 [info] ┐ruff_server::server::api::notifications::cancel::run{}
┘
```

---

_Comment by @charliermarsh on 2024-06-02 18:01_

Just to clarify, the VS Code extension version is only _maybe_ relevant. What's more important is the Ruff executable version, which has to be later than v0.4.7.

---

_Comment by @rainzee on 2024-06-03 02:24_

> Just to clarify, the VS Code extension version is only _maybe_ relevant. What's more important is the Ruff executable version, which has to be later than v0.4.7.

I located the ruff plugin directly in .vscode, and then checked the version of bundled ruff.exe under the bin in it, and it was 0.4.7 for sure

---

_Comment by @charliermarsh on 2024-06-03 02:33_

The plugin only uses the bundled exe if it cannot discover a Ruff installed elsewhere on your machine. That's why we'd need the `serverInfo` information that Micha mentioned above to be certain.

---

_Comment by @yanyongyu on 2024-06-03 03:25_

After updating the ruff installed in virtualenv to 0.4.7 , this seems fixed. Thanks for your explanation. 

But the server info is not showed in the log when enable the native server. I can find the info only when native server disabled.

```log
2024-06-03 11:26:33.254 [info] Found ruff 0.4.7 at /workspaces/xxx/.venv/bin/ruff
```

Maybe ruff extension could provide a command to show current server info? This will be helpful to debugging.

---

_Comment by @MichaReiser on 2024-06-03 06:43_

> But the server info is not showed in the log when enable the native server. I can find the info only when native server disabled.

it might be necessary to change `Ruff > Trace: Server` to `messages` or `verbose. 

> Maybe ruff extension could provide a command to show current server info? This will be helpful to debugging.

I agree, a version or debug command that collects debugging relevant information would be helpful

---

_Comment by @rainzee on 2024-06-03 09:41_

> The plugin only uses the bundled exe if it cannot discover a Ruff installed elsewhere on your machine. That's why we'd need the `serverInfo` information that Micha mentioned above to be certain.

I have a global install of ruff that is 0.4.7 and I don't have ruff in my current virtual environment(Obviously, all my projects use the globally installed), so at this point will the ruff plugin use the globally installed  or the one that comes with the plugin? The problem after installing ruff in the current virtual environment is indeed solved, but the logic is not clear to me, because the binary version that comes with the ruff plugin is also 0.4.7

---

_Comment by @T-256 on 2024-06-03 10:51_

@rainzee by default Ruff extension will use globally or founded ruff binary at PATH. if it unable to find ruff binary, it will use bundled binary. you can change this behavior and force it to use bundled one via `ruff.importStrategy` in settings.

---

_Closed by @rainzee on 2024-06-03 11:36_

---

_Comment by @pep-sanwer on 2024-06-12 21:35_

Apologies for commenting on a closed ticket, but it looks to be related:
There seems to be a similar issue when using a VSCode interactive session (VSCode > `Run Current File in Interactive Window` or `Create Interactive Window`:

My local Ruff is on `0.4.8` 
```
2024-06-12 16:23:08.468 [info] Name: Ruff
2024-06-12 16:23:08.468 [info] Module: ruff
2024-06-12 16:23:08.469 [info] Python extension loading
2024-06-12 16:23:08.469 [info] Waiting for interpreter from python extension.
2024-06-12 16:23:08.469 [info] Python extension loaded
2024-06-12 16:23:08.469 [info] Server run command: $HOME/.local/bin/ruff server --preview
2024-06-12 16:23:08.469 [info] Server: Start requested.
2024-06-12 16:23:08.567 [info]    0.843883s INFO ruff_server::server Configuration file watcher successfully registered

2024-06-12 16:23:26.985 [info]   19.307727s WARN ruff_server::session::index No settings available for untitled:/Interactive-1.interactive - falling back to default settings
  19.308549s DEBUG ruff_server::lint Ignored path via `exclude`: /Interactive-1.interactive

2024-06-12 16:23:28.934 [info]   21.293249s WARN ruff_server::session::index No settings available for untitled:/Interactive-1.interactive - falling back to default settings

2024-06-12 16:23:28.934 [info]   21.293530s DEBUG ruff_server::lint Ignored path via `exclude`: /Interactive-1.interactive

2024-06-12 16:23:29.143 [info]   21.488888s WARN ruff_server::session::index No settings available for untitled:/Interactive-1.interactive - falling back to default settings
  21.489236s DEBUG ruff_server::lint Ignored path via `exclude`: /Interactive-1.interactive
  21.489315s WARN ruff_server::session::index No settings available for untitled:/Interactive-1.interactive - falling back to default settings
  21.489531s DEBUG ruff_server::lint Ignored path via `exclude`: /Interactive-1.interactive

2024-06-12 16:23:29.225 [info]   21.576600s WARN ruff_server::session::index No settings available for untitled:/Interactive-1.interactive - falling back to default settings
  21.578133s DEBUG ruff_server::lint Ignored path via `exclude`: /Interactive-1.interactive
  21.578433s WARN ruff_server::session::index No settings available for untitled:/Interactive-1.interactive - falling back to default settings
  21.578733s DEBUG ruff_server::lint Ignored path via `exclude`: /Interactive-1.interactive

2024-06-12 16:23:30.044 [info]   22.403670s WARN ruff_server::session::index No settings available for untitled:/Interactive-1.interactive - falling back to default settings

2024-06-12 16:23:30.045 [info]   22.404434s DEBUG ruff_server::lint Ignored path via `exclude`: /Interactive-1.interactive
  22.404564s WARN ruff_server::session::index No settings available for untitled:/Interactive-1.interactive - falling back to default settings

2024-06-12 16:23:30.045 [info]   22.404794s DEBUG ruff_server::lint Ignored path via `exclude`: /Interactive-1.interactive
```

I've attempted excluding the interactive file type via my user settings like the below, but warnings still loop:
```json
{
...
    "ruff.path": ["$HOME/.local/bin/ruff"],
    "ruff.exclude": ["*.interactive"],
    "ruff.nativeServer": true,
}
```

What the logs look like when `ruff.nativeServer` is disabled:
```
2024-06-12 16:31:11.131 [info] Name: Ruff
2024-06-12 16:31:11.131 [info] Module: ruff
2024-06-12 16:31:11.131 [info] Python extension loading
2024-06-12 16:31:11.131 [info] Waiting for interpreter from python extension.
2024-06-12 16:31:11.131 [info] Python extension loaded
2024-06-12 16:31:11.131 [info] Server run command: $HOME/.micromamba/envs/seg/bin/python $HOME/.vscode-server/extensions/charliermarsh.ruff-2024.26.0-linux-x64/bundled/tool/server.py
2024-06-12 16:31:11.131 [info] Server: Start requested.
2024-06-12 16:31:11.360 [info] 2024-06-12 16:31:11,285 INFO Starting IO server

2024-06-12 16:31:11.431 [info] Workspace settings: [
    {
        "nativeServer": false,
        "cwd": "$HOME/Dev/projects/seg-2023/seg_2023",
        "workspace": "file://$HOME/Dev/projects/seg-2023/seg_2023",
        "path": [
            "$HOME/.local/bin/ruff"
        ],
        "ignoreStandardLibrary": true,
        "interpreter": [
            "$HOME/.micromamba/envs/seg/bin/python"
        ],
        "configuration": null,
        "importStrategy": "fromEnvironment",
        "codeAction": {
            "fixViolation": {
                "enable": true
            },
            "disableRuleComment": {
                "enable": true
            }
        },
        "lint": {
            "enable": true,
            "run": "onType",
            "args": [],
            "preview": null,
            "select": null,
            "extendSelect": null,
            "ignore": null
        },
        "format": {
            "args": [],
            "preview": null
        },
        "enable": true,
        "organizeImports": true,
        "fixAll": true,
        "showNotifications": "off",
        "exclude": null,
        "lineLength": null,
        "configurationPreference": "editorFirst"
    }
]
2024-06-12 16:31:11.433 [info] Global settings: {
    "nativeServer": false,
    "cwd": "$HOME/AppData/Local/Programs/Microsoft VS Code",
    "workspace": "$HOME/AppData/Local/Programs/Microsoft VS Code",
    "path": [
        "$HOME/.local/bin/ruff"
    ],
    "ignoreStandardLibrary": true,
    "interpreter": [],
    "configuration": null,
    "importStrategy": "fromEnvironment",
    "codeAction": {
        "fixViolation": {
            "enable": true
        },
        "disableRuleComment": {
            "enable": true
        }
    },
    "lint": {
        "enable": true,
        "run": "onType",
        "args": []
    },
    "format": {
        "args": []
    },
    "enable": true,
    "organizeImports": true,
    "fixAll": true,
    "showNotifications": "off",
    "configurationPreference": "editorFirst"
}
2024-06-12 16:31:11.576 [info] Using 'path' setting: $HOME/.local/bin/ruff
2024-06-12 16:31:11.601 [info] Inferred version 0.4.8 for: $HOME/.local/bin/ruff
2024-06-12 16:31:11.614 [info] Found ruff 0.4.8 at $HOME/.local/bin/ruff
2024-06-12 16:31:11.617 [info] Running Ruff with: $HOME/.local/bin/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '$HOME/Dev/projects/seg-2023/seg_2023/src/seg_2023/config.py']
2024-06-12 16:31:31.659 [info] Using 'path' setting: $HOME/.local/bin/ruff
2024-06-12 16:31:31.673 [info] Found ruff 0.4.8 at $HOME/.local/bin/ruff
2024-06-12 16:31:31.676 [info] Running Ruff with: $HOME/.local/bin/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '/Interactive-1.interactive']
2024-06-12 16:31:33.781 [info] Using 'path' setting: $HOME/.local/bin/ruff
2024-06-12 16:31:33.804 [info] Found ruff 0.4.8 at $HOME/.local/bin/ruff
2024-06-12 16:31:33.836 [info] Running Ruff with: $HOME/.local/bin/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '/Interactive-1.interactive']
2024-06-12 16:31:33.842 [info] Using 'path' setting: $HOME/.local/bin/ruff
2024-06-12 16:31:33.842 [info] Found ruff 0.4.8 at $HOME/.local/bin/ruff
2024-06-12 16:31:33.842 [info] Running Ruff with: $HOME/.local/bin/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '/Interactive-1.interactive']
2024-06-12 16:31:33.843 [info] 2024-06-12 16:31:33,842 WARNING Cancel notification for unknown message id "6"

2024-06-12 16:31:33.844 [info] Using 'path' setting: $HOME/.local/bin/ruff
2024-06-12 16:31:33.844 [info] Found ruff 0.4.8 at $HOME/.local/bin/ruff
2024-06-12 16:31:33.844 [info] Running Ruff with: $HOME/.local/bin/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '/Interactive-1.interactive']
2024-06-12 16:31:33.845 [info] Using 'path' setting: $HOME/.local/bin/ruff
2024-06-12 16:31:33.845 [info] Found ruff 0.4.8 at $HOME/.local/bin/ruff
2024-06-12 16:31:33.845 [info] Running Ruff with: $HOME/.local/bin/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '/Interactive-1.interactive']
2024-06-12 16:31:33.846 [info] Using 'path' setting: $HOME/.local/bin/ruff
2024-06-12 16:31:33.846 [info] Found ruff 0.4.8 at $HOME/.local/bin/ruff
2024-06-12 16:31:33.846 [info] Running Ruff with: $HOME/.local/bin/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '/Interactive-1.interactive']
2024-06-12 16:31:34.037 [info] 2024-06-12 16:31:34,036 WARNING Cancel notification for unknown message id "8"

2024-06-12 16:31:34.176 [info] Using 'path' setting: $HOME/.local/bin/ruff
2024-06-12 16:31:34.177 [info] Found ruff 0.4.8 at $HOME/.local/bin/ruff
2024-06-12 16:31:34.177 [info] Running Ruff with: $HOME/.local/bin/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '/Interactive-1.interactive']
2024-06-12 16:31:34.183 [info] Using 'path' setting: $HOME/.local/bin/ruff
2024-06-12 16:31:34.184 [info] Found ruff 0.4.8 at $HOME/.local/bin/ruff
2024-06-12 16:31:34.185 [info] Running Ruff with: $HOME/.local/bin/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '/Interactive-1.interactive']
``` 

---

_Comment by @MichaReiser on 2024-06-13 06:09_

I created a new issue for this. It seems similar, but isn't the same.

---
