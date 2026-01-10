```yaml
number: 1362
title: Problem running the server from WSL
type: issue
state: closed
author: klonuo
labels:
  - server
  - windows
assignees: []
created_at: 2025-10-15T14:47:06Z
updated_at: 2025-10-15T15:28:54Z
url: https://github.com/astral-sh/ty/issues/1362
synced_at: 2026-01-10T02:06:25Z
```

# Problem running the server from WSL

---

_Issue opened by @klonuo on 2025-10-15 14:47_

### Summary

Hi,

I'm working on a project in WSL, but my Windows editor reported crushes on `ty` LSP server, so I tried to set the project to use `ty` as LSP server from within the WSL, but it fails too.

Let me walk you quickly:

Executing `ruff` and `ty` checks through `wsl` command (`wsl --shell-type login -- <command>`) works correctly, and `ty` has the right context from the project file and venv:
```powershell
Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\Ubuntu-20.04\home\klo\projects\api (main)
❯ dir

    Directory: \\wsl.localhost\Ubuntu-20.04\home\klo\projects\api

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----          2025-10-15    16:09                .git
d----          2025-10-15    15:57                .ruff_cache
d----          2025-10-15    13:14                .venv
d----          2025-10-15    13:31                .zed
d----          2025-10-15    14:10                utils
-----          2025-10-15    14:07            429 .env
-----          2025-10-15    14:14             52 .env.example
-----          2025-10-15    13:12            109 .gitignore
-----          2025-10-15    13:12              5 .python-version
-----          2025-10-15    14:37           1433 fastapi.sublime-project
-----          2025-10-15    14:38          39007 fastapi.sublime-workspace
-----          2025-10-15    14:14           4159 main.py
-----          2025-10-15    14:13            325 pyproject.toml
-----          2025-10-15    14:14           1768 README.md
-----          2025-10-15    14:58           2167 tmp.py
-----          2025-10-15    13:14          65640 uv.lock

Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\Ubuntu-20.04\home\klo\projects\api (main)
❯ wsl --shell-type login -- ruff check -v
[2025-10-15][16:09:58][ruff::resolve][DEBUG] Using Ruff default settings
[2025-10-15][16:09:58][ruff_workspace::pyproject][DEBUG] Detected minimum supported `requires-python` version: 3.12
[2025-10-15][16:09:58][ruff::resolve][DEBUG] Derived `target-version` from found `requires-python`: Py312
[2025-10-15][16:09:58][ignore::gitignore][DEBUG] opened gitignore file: /home/klo/.config/git/ignore
[2025-10-15][16:09:58][ignore::gitignore][DEBUG] opened gitignore file: /home/klo/projects/api/.gitignore
[2025-10-15][16:09:58][ignore::gitignore][DEBUG] opened gitignore file: /home/klo/projects/api/.git/info/exclude
[2025-10-15][16:09:58][ignore::walk][DEBUG] ignoring /home/klo/projects/api/.venv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/klo/projects/api/.gitignore"), original: ".venv", actual: "**/.venv", is_whitelist: false, is_only_dir: false })))
[2025-10-15][16:09:58][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/klo/projects/api/utils/qwen_inference.py"
[2025-10-15][16:09:58][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/klo/projects/api/.git"
[2025-10-15][16:09:58][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/klo/projects/api/pyproject.toml"
[2025-10-15][16:09:58][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/klo/projects/api/tmp.py"
[2025-10-15][16:09:58][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/klo/projects/api/main.py"
[2025-10-15][16:09:58][ignore::gitignore][DEBUG] opened gitignore file: /home/klo/projects/api/.ruff_cache/.gitignore
[2025-10-15][16:09:58][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/klo/projects/api/.ruff_cache"
[2025-10-15][16:09:58][ruff::commands::check][DEBUG] Identified files to lint in: 14.991101ms
[2025-10-15][16:09:58][ruff::diagnostics][DEBUG] Checking: /home/klo/projects/api/pyproject.toml
[2025-10-15][16:09:58][ruff::commands::check][DEBUG] Checked 4 files in: 1.19529ms
All checks passed!
Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\Ubuntu-20.04\home\klo\projects\api (main)
❯ wsl --shell-type login -- ty check -q
Found 4 diagnostics
```

When I set it as LSP server this way, I get this info from LSP logs:

First:
```py
# --> ty initialize (1):
{
    "processId": 10276,
    "clientInfo": {"name": "Sublime Text LSP", "version": "2.6.0"},
    "rootUri": "file://wsl.localhost/Ubuntu-20.04/home/klo/projects/api",
    "rootPath": "\\\\wsl.localhost\\Ubuntu-20.04\\home\\klo\\projects\\api",
    "workspaceFolders": [
        {
            "name": "api",
            "uri": "file://wsl.localhost/Ubuntu-20.04/home/klo/projects/api",
        }
    ],
    # ...
}
```

then:
```py
# <<< ty (1) (duration: 461ms):
{
    "capabilities": {
        "completionProvider": {"triggerCharacters": ["."]},
        "declarationProvider": True,
        "definitionProvider": True,
        "documentHighlightProvider": True,
        "documentSymbolProvider": True,
        "executeCommandProvider": {
            "commands": ["ty.printDebugInformation"],
            "workDoneProgress": False,
        },
        "hoverProvider": True,
        "inlayHintProvider": {},
        "positionEncoding": "utf-16",
        "referencesProvider": True,
        "selectionRangeProvider": True,
        "semanticTokensProvider": {
            "full": True,
            "legend": {
                "tokenModifiers": ["definition", "readonly", "async"],
                "tokenTypes": [
                    "namespace",
                    "class",
                    "parameter",
                    "selfParameter",
                    "clsParameter",
                    "variable",
                    "property",
                    "function",
                    "method",
                    "keyword",
                    "string",
                    "number",
                    "decorator",
                    "builtinConstant",
                    "typeParameter",
                ],
            },
            "range": True,
        },
        "signatureHelpProvider": {
            "retriggerCharacters": [")"],
            "triggerCharacters": ["(", ","],
        },
        "typeDefinitionProvider": True,
        "workspaceSymbolProvider": True,
        "textDocumentSync": {"change": {"syncKind": 2}, "didOpen": {}, "didClose": {}},
    },
    "serverInfo": {"name": "ty", "version": "0.0.1-alpha.22"},
}
```

which results in:
> ty: ty failed
> ty:   Cause: Failed to start server
> ty:   Cause: Workspace URL is not a file or directory:

```
Url {
    scheme: "file",
    cannot_be_a_base: false,
    username: "",
    password: None,
    host: Some(Domain("wsl.localhost")),
    port: None,
    path: "/Ubuntu-20.04/home/klo/projects/api",
    query: None,
    fragment: None,
}
```

And right after that it reports:
```
 -> ty initialized: {}
 -> ty workspace/didChangeConfiguration: {}
 -> ty textDocument/didOpen: {'textDocument': {'uri': 'file://wsl.localhost/Ubuntu-20.04/...
```
However LSP stays in "initializing..." phase maybe because of the previous fail and it doesn't work.

Ok, so that is my problem, thanks for reading I know its lengthy and condensed but couldn't explain it shorter.


### Version

ty 0.0.1-alpha.22 (2f3190d09 2025-10-10)

---

_Label `server` added by @AlexWaygood on 2025-10-15 14:49_

---

_Label `windows` added by @AlexWaygood on 2025-10-15 14:49_

---

_Comment by @klonuo on 2025-10-15 15:17_

Same happens with `ruff` server too, while trying to run with `wsl`, but `ruff` server doesn't fail to start on the same project from my Windows `ruff` binary.

For `ty` as mentioned it fails also to start Windows binary, and besides provided logs above, this is what is appended:

```
ty: 2025-10-15 17:07:11.489449800 ERROR Failed to create project for `\\wsl.localhost\Ubuntu-20.04\home\klo\projects\api`: Failed to discover the site-packages directory: Invalid local virtual environment `\\?\UNC\wsl.localhost\Ubuntu-20.04\home\klo\projects\api\.venv`: Could not find a `site-packages` directory for this Python installation/executable. Falling back to default settings
:: [17:07:11.489] <-  ty window/showMessage: {'message': 'Failed to load project rooted at \\\\wsl.localhost\\Ubuntu-20.04\\home\\klo\\projects\\api. Please refer to the logs for more details.', 'type': 1}
ty: 2025-10-15 17:07:11.490223200  INFO Defaulting to python-platform `win32`
ty: 2025-10-15 17:07:11.585543400 ERROR panicked at crates\ty_server\src\session.rs:546:30:
ty: Default configuration to be valid: Failed to discover the site-packages directory
ty: 
ty: Caused by:
ty:     Invalid local virtual environment `\\?\UNC\wsl.localhost\Ubuntu-20.04\home\klo\projects\api\.venv`: Could not find a `site-packages` directory for this Python installation/executable
ty:    0: <unknown>
ty:    1: <unknown>
ty:    2: <unknown>
ty:    3: <unknown>
ty:    4: <unknown>
ty:    5: <unknown>
ty:    6: <unknown>
ty:    7: <unknown>
ty:    8: <unknown>
ty:    9: <unknown>
ty:   10: <unknown>
ty:   11: <unknown>
ty:   12: <unknown>
ty:   13: BaseThreadInitThunk
ty:   14: RtlUserThreadStart
ty: 
ty: panicked at crates\ty_server\src\session.rs:546:30:
ty: Default configuration to be valid: Failed to discover the site-packages directory
ty: 
ty: Caused by:
ty:     Invalid local virtual environment `\\?\UNC\wsl.localhost\Ubuntu-20.04\home\klo\projects\api\.venv`: Could not find a `site-packages` directory for this Python installation/executable
ty:    0: <unknown>
ty:    1: <unknown>
ty:    2: <unknown>
ty:    3: <unknown>
ty:    4: <unknown>
ty:    5: <unknown>
ty:    6: <unknown>
ty:    7: <unknown>
ty:    8: <unknown>
ty:    9: <unknown>
ty:   10: <unknown>
ty:   11: <unknown>
ty:   12: <unknown>
ty:   13: BaseThreadInitThunk
ty:   14: RtlUserThreadStart
```

---

_Comment by @klonuo on 2025-10-15 15:28_

Actually the problem seems to be in Sublime LSP, as for the same WSL project both `ty` and `ruff` servers work correctly in Zed

---

_Closed by @klonuo on 2025-10-15 15:28_

---
