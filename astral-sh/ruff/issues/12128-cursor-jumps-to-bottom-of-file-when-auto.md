```yaml
number: 12128
title: Cursor jumps to bottom of file when auto-formatting on save
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - server
assignees: []
created_at: 2024-07-01T01:59:52Z
updated_at: 2024-07-04T03:54:08Z
url: https://github.com/astral-sh/ruff/issues/12128
synced_at: 2026-01-12T15:54:51Z
```

# Cursor jumps to bottom of file when auto-formatting on save

---

_@DetachHead_

this is a very strange bug that i can't seem to minify at all. 

to reproduce:

1. set `ruff.nativeServer` to `true`
2. set `editor.formatOnSave` to `true` (it seems to only happen reliably when formatting on save)
3. insert a newline near the top of a large file (in my case i copied the contents of `builtins.pyi` into a random `.py` file in my project)
4. save the file with ctrl+s

https://github.com/astral-sh/ruff/assets/57028336/c030626b-d7ac-48fc-8259-a3c6269de2c4



---

_Comment by @charliermarsh on 2024-07-01 02:01_

Oh wow, thanks for the video.

---

_Label `bug` added by @charliermarsh on 2024-07-01 02:01_

---

_Label `server` added by @charliermarsh on 2024-07-01 02:01_

---

_Renamed from "new language server jumps to the bottom of the file when inserting newline and formatting a file with thousands of lines of code" to "Cursor jumps to bottom of file when auto-formatting on save" by @dhruvmanila on 2024-07-02 04:29_

---

_Comment by @dhruvmanila on 2024-07-02 07:29_

Unfortunately, I'm not able to reproduce this. Can you provide some additional details like:
1. VS Code version
2. Ruff version
3. Ruff VS Code extension version
4. Any relevant VS Code settings related to Python and Ruff
5. If relevant, `pyproject.toml` / `ruff.toml` settings

I've copied the [`builtins.pyi` file](https://github.com/python/typeshed/blob/main/stdlib/builtins.pyi) and following the exact steps as you are:

https://github.com/astral-sh/ruff/assets/67177269/71d46ff6-922b-41a9-8cdc-ad00aad10039

Can you also provide the output of "Ruff: Print Debug Information" command. Here's mine:
```
2024-07-02 12:57:10.422 [info] [Info  - 12:57:10 PM] executable = /Users/dhruv/.local/bin/ruff
version = 0.5.0
encoding = UTF16
open_document_count = 1
active_workspace_count = 1
configuration_files = ["/Users/dhruv/playground/ruff/pyproject.toml"]
capabilities.code_action_deferred_edit_resolution = true
capabilities.apply_edit = true
capabilities.document_changes = true
capabilities.workspace_refresh = true
capabilities.pull_diagnostics = true
```

---

_Added to milestone `Ruff Server: Stable` by @dhruvmanila on 2024-07-02 11:15_

---

_Label `needs-info` added by @dhruvmanila on 2024-07-03 03:36_

---

_Comment by @DetachHead on 2024-07-03 05:01_

sorry, i completely forgot that i'm using [a different version of the builtin stubs with docstrings in them](https://github.com/detachhead/basedpyright?tab=readme-ov-file#docstrings-for-compiled-builtin-modules), and the issue doesn't seem to occur without them.

[here's the version of the file i was running it on](https://github.com/user-attachments/files/16077236/foo.txt) (sorry it's a `.txt` instead of `.py` because github doesn't let you upload python files for some reason)

in case you still aren't able to reproduce it, heres the additional info you requested:

vscode: v1.90.2
ruff: v0.4.10 (also tested on v0.5.0)
ruff vscode extension: v2024.30.0
`pyproject.toml`/`ruff.toml`: N/A, i deleted them and the issue still occurs
vscode settings related to ruff: `{"ruff.nativeServer": true, "[python]": {"editor.formatOnSave": true, "editor.defaultFormatter": "charliermarsh.ruff"}}`

ruff debug info:
```
executable = c:\Users\user\project\.venv\Scripts\ruff.exe
version = 0.4.10
encoding = UTF16
open_document_count = 28
active_workspace_count = 1
configuration_files = ["c:\\Users\\user\\project\\ruff.toml"]
capabilities.code_action_deferred_edit_resolution = true
capabilities.apply_edit = true
capabilities.document_changes = true
capabilities.workspace_refresh = true
capabilities.pull_diagnostics = true
```

---

_Comment by @dhruvmanila on 2024-07-03 05:32_

Thank you for providing the details! I can reproduce it now.

---

_Label `needs-info` removed by @dhruvmanila on 2024-07-03 05:32_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-07-03 09:46_

---

_Comment by @dhruvmanila on 2024-07-03 10:57_

I think this is happening because the replacement algorithm (https://github.com/astral-sh/ruff/blob/88a4cc41f76cbb5d1318d5e689b118ec624b8d69/crates/ruff_server/src/edit/replacement.rs) is different than the one in `ruff-lsp` (https://github.com/astral-sh/ruff-lsp/blob/c51c77c269c3f13be2534d118b81e1c3b9bbfaff/ruff_lsp/server.py#L1430-L1470).

The returned `TextEdit` replacement range is different for the same formatting between `ruff server` and `ruff-lsp`:

```jsonc
// ruff server
2024-07-03 16:26:59.123 [info] Result: [
    {
        "newText": "...",
        "range": {
            "end": {
                "character": 0,
                "line": 5089
            },
            "start": {
                "character": 0,
                "line": 1146
            }
        }
    }
]


// ruff-lsp
2024-07-03 16:26:20.420 [info] Result: [
    {
        "range": {
            "start": {
                "line": 1146,
                "character": 0
            },
            "end": {
                "line": 1149,
                "character": 0
            }
        },
        "newText": ""
    }
]
```

---

_Closed by @dhruvmanila on 2024-07-04 03:54_

---

_Closed by @dhruvmanila on 2024-07-04 03:54_

---
