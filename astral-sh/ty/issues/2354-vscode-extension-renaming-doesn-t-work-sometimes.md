```yaml
number: 2354
title: "VSCode extension: Renaming doesn't work sometimes (including ignoring used line ending style)"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - server
assignees: []
created_at: 2026-01-06T07:32:06Z
updated_at: 2026-01-10T02:19:30Z
url: https://github.com/astral-sh/ty/issues/2354
synced_at: 2026-01-10T03:05:43Z
```

# VSCode extension: Renaming doesn't work sometimes (including ignoring used line ending style)

---

_Issue opened by @MeGaGiGaGon on 2026-01-06 07:32_

### Summary

Hopefully this is even reproducible, since beyond the video and the logs I'm having a very hard time even describing what's happening.

To reproduce:
1. Make a python file and open it in VSCode.
For example in powershell,
   ```
   PS D:\python_projects\ty_issues> echo @"
   x = 1
   x = 2
   # test
   "@ > main.py
   PS D:\python_projects\ty_issues> code main.py
   ```
2. Restart the server by going to the extensions tab, pressing "Disable" then "Restart Extensions", then "Enable" quickly. Or sometimes using the `ty: Restart Server` command. Or sometimes on restarting VSCode. Or sometimes it doesn't happen.

ty is now in an extremely confused state on that open `main.py` file. For example you can now rename numbers even though that shouldn't work:

<img width="256" height="97" alt="Image" src="https://github.com/user-attachments/assets/4d824c45-ecab-4855-a273-87d3954b30bf" />

Additionally, if the End of Line sequence is set to CRLF, renaming the `x`s in the file will cause all newlines around them to start doubling, so it looks like in this state ty fails to figure out what the line ending mode is.

As for why I'm encountering this so much, currently I'm constantly switching between basedpyright and ty in VSC, so that I can compare how they both handle my code. This means I restart both language servers fairly often, and so have run into this bug a few times while trying to rename things.

Video of me triggering the cursed renaming (sorry for very scuffed quality, my computer is dying so I can't record in higher resolutions)
https://youtu.be/3DVj_0Z4ND4

<details>
<summary>ty Logs</summary>

```
2026-01-05 23:16:39.412 [info] Name: ty
2026-01-05 23:16:39.412 [info] Module: ty
2026-01-05 23:16:39.412 [info] Python extension loading
2026-01-05 23:16:39.412 [info] Waiting for interpreter from python extension.
2026-01-05 23:16:39.412 [info] Python extension loaded
2026-01-05 23:16:39.412 [info] Using interpreter: c:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none\python.exe
2026-01-05 23:16:39.413 [info] Initialization options: {
    "logLevel": "trace"
}
2026-01-05 23:16:39.593 [info] Falling back to bundled executable: c:\Users\GiGaGon\.vscode-oss\extensions\astral-sh.ty-2026.0.0-win32-x64\bundled\libs\bin\ty.exe
2026-01-05 23:16:39.599 [info] Found executable at c:\Users\GiGaGon\.vscode-oss\extensions\astral-sh.ty-2026.0.0-win32-x64\bundled\libs\bin\ty.exe
2026-01-05 23:16:39.600 [info] Server run command: c:\Users\GiGaGon\.vscode-oss\extensions\astral-sh.ty-2026.0.0-win32-x64\bundled\libs\bin\ty.exe server
2026-01-05 23:16:39.601 [info] Server: Start requested.
2026-01-05 23:16:39.639 [info] ty server version: 0.0.9 (f1652f05d 2026-01-05)
2026-01-05 23:17:25.941 [info] Server: Stop requested
2026-01-05 23:17:25.945 [info] Using interpreter: c:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none\python.exe
2026-01-05 23:17:25.945 [info] Initialization options: {
    "logLevel": "trace"
}
2026-01-05 23:17:26.129 [info] Falling back to bundled executable: c:\Users\GiGaGon\.vscode-oss\extensions\astral-sh.ty-2026.0.0-win32-x64\bundled\libs\bin\ty.exe
2026-01-05 23:17:26.129 [info] Found executable at c:\Users\GiGaGon\.vscode-oss\extensions\astral-sh.ty-2026.0.0-win32-x64\bundled\libs\bin\ty.exe
2026-01-05 23:17:26.129 [info] Server run command: c:\Users\GiGaGon\.vscode-oss\extensions\astral-sh.ty-2026.0.0-win32-x64\bundled\libs\bin\ty.exe server
2026-01-05 23:17:26.130 [info] Server: Start requested.
2026-01-05 23:17:26.158 [info] ty server version: 0.0.9 (f1652f05d 2026-01-05)
2026-01-05 23:17:38.959 [info] Name: ty
2026-01-05 23:17:38.959 [info] Module: ty
2026-01-05 23:17:38.959 [info] Python extension loading
2026-01-05 23:17:38.959 [info] Waiting for interpreter from python extension.
2026-01-05 23:17:38.959 [info] Python extension loaded
2026-01-05 23:17:38.959 [info] Using interpreter: c:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none\python.exe
2026-01-05 23:17:38.959 [info] Initialization options: {
    "logLevel": "trace"
}
2026-01-05 23:17:39.226 [info] Falling back to bundled executable: c:\Users\GiGaGon\.vscode-oss\extensions\astral-sh.ty-2026.0.0-win32-x64\bundled\libs\bin\ty.exe
2026-01-05 23:17:39.232 [info] Found executable at c:\Users\GiGaGon\.vscode-oss\extensions\astral-sh.ty-2026.0.0-win32-x64\bundled\libs\bin\ty.exe
2026-01-05 23:17:39.232 [info] Server run command: c:\Users\GiGaGon\.vscode-oss\extensions\astral-sh.ty-2026.0.0-win32-x64\bundled\libs\bin\ty.exe server
2026-01-05 23:17:39.233 [info] Server: Start requested.
2026-01-05 23:17:39.270 [info] ty server version: 0.0.9 (f1652f05d 2026-01-05)

```

</details>

<details>
<summary>ty Language Server Logs</summary>

```
2026-01-05 23:17:39.267230400 DEBUG main ty_server::server: Initialization options: InitializationOptions {
    log_level: Some(
        Trace,
    ),
    log_file: None,
    options: ClientOptions {
        global: GlobalOptions {
            diagnostic_mode: None,
            experimental: None,
            show_syntax_errors: None,
        },
        workspace: WorkspaceOptions {
            configuration: None,
            configuration_file: None,
            disable_language_services: None,
            inlay_hints: None,
            completions: None,
            python_extension: None,
        },
        unknown: {},
    },
}
2026-01-05 23:17:39.268020700 DEBUG main ty_server::server: Resolved client capabilities: ["WORKSPACE_DIAGNOSTIC_REFRESH", "INLAY_HINT_REFRESH", "PULL_DIAGNOSTICS", "TYPE_DEFINITION_LINK_SUPPORT", "DEFINITION_LINK_SUPPORT", "DECLARATION_LINK_SUPPORT", "PREFER_MARKDOWN_IN_HOVER", "SIGNATURE_LABEL_OFFSET_SUPPORT", "SIGNATURE_ACTIVE_PARAMETER_SUPPORT", "HIERARCHICAL_DOCUMENT_SYMBOL_SUPPORT", "WORK_DONE_PROGRESS", "FILE_WATCHER_SUPPORT", "RELATIVE_FILE_WATCHER_SUPPORT", "DIAGNOSTIC_DYNAMIC_REGISTRATION", "WORKSPACE_CONFIGURATION", "COMPLETION_ITEM_LABEL_DETAILS_SUPPORT", "DIAGNOSTIC_RELATED_INFORMATION", "PREFER_MARKDOWN_IN_COMPLETION"]
2026-01-05 23:17:39.268042600  INFO main ty_server::server: Version: 0.0.9 (f1652f05d 2026-01-05)
2026-01-05 23:17:39.271374500  WARN main ty_server::server: No workspace(s) were provided during initialization. Using the current working directory from the fallback system as a default workspace: c:\Users\GiGaGon\AppData\Local\Programs\VSCodium
2026-01-05 23:17:39.271577300 DEBUG ty:main ty_server::server::main_loop: Requesting workspace configuration for workspaces
2026-01-05 23:17:39.277698300 DEBUG ty:main ty_server::session: Deferring `textDocument/didOpen` notification until all workspaces are initialized
2026-01-05 23:17:39.279799400 DEBUG ty:main client_response{id=0 method="workspace/configuration"}: ty_server::server::main_loop: Received workspace configurations, initializing workspaces
2026-01-05 23:17:39.279866100 DEBUG ty:main ty_server::session: Initializing workspace `file:///C:/Users/GiGaGon/AppData/Local/Programs/VSCodium`: WorkspaceOptions {
    configuration: None,
    configuration_file: None,
    disable_language_services: Some(
        false,
    ),
    inlay_hints: Some(
        InlayHintOptions {
            variable_types: Some(
                true,
            ),
            call_argument_names: Some(
                true,
            ),
        },
    ),
    completions: Some(
        CompletionOptions {
            auto_import: Some(
                true,
            ),
        },
    ),
    python_extension: Some(
        PythonExtension {
            active_environment: Some(
                ActiveEnvironment {
                    executable: PythonExecutable {
                        uri: Url {
                            scheme: "file",
                            cannot_be_a_base: false,
                            username: "",
                            password: None,
                            host: None,
                            port: None,
                            path: "/c%3A/Users/GiGaGon/AppData/Roaming/uv/python/cpython-3.14.0-windows-x86_64-none/python.exe",
                            query: None,
                            fragment: None,
                        },
                        sys_prefix: "C:\\Users\\GiGaGon\\AppData\\Roaming\\uv\\python\\cpython-3.14.0-windows-x86_64-none",
                    },
                    environment: None,
                    version: Some(
                        EnvironmentVersion {
                            major: 3,
                            minor: 14,
                            patch: 0,
                            sys_version: "3.14.0 (main, Oct 28 2025, 12:05:23) [MSC v.1944 64 bit (AMD64)]",
                        },
                    ),
                },
            ),
        },
    ),
}
2026-01-05 23:17:39.279925600 DEBUG ty:main ty_server::session::options: Using the Python environment selected in your editor in case the configuration doesn't specify a Python environment: C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none
2026-01-05 23:17:39.279934300 DEBUG ty:main ty_server::session::options: Using the Python version selected in your editor: 3.14 in case the configuration doesn't specify a Python version
2026-01-05 23:17:39.280010800 DEBUG ty:main ty_project::metadata: Searching for a project in 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium'
2026-01-05 23:17:39.280496300 DEBUG ty:main ty_project::metadata: The ancestor directories contain no `pyproject.toml`. Falling back to a virtual project.
2026-01-05 23:17:39.280515700 DEBUG ty:main ty_project::metadata::configuration_file: Searching for a user-level configuration at `C:\Users\GiGaGon\AppData\Roaming\ty\ty.toml`
2026-01-05 23:17:39.282059400  INFO ty:main ty_project::metadata::options: Defaulting to python-platform `win32`
2026-01-05 23:17:39.282389300 DEBUG ty:main ty_python_semantic::site_packages: Attempting to parse virtual environment metadata at 'C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none\pyvenv.cfg'
2026-01-05 23:17:39.282458000 DEBUG ty:main ty_python_semantic::site_packages: Searching for site-packages directory in sys.prefix C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none
2026-01-05 23:17:39.282528500 DEBUG ty:main ty_python_semantic::site_packages: Resolved site-packages directories for this environment are: ["C:\\Users\\GiGaGon\\AppData\\Roaming\\uv\\python\\cpython-3.14.0-windows-x86_64-none\\Lib\\site-packages"]
2026-01-05 23:17:39.282545700 DEBUG ty:main ty_python_semantic::site_packages: Searching for real stdlib directory in sys.prefix C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none
2026-01-05 23:17:39.282617400 DEBUG ty:main ty_python_semantic::site_packages: Resolved real stdlib directory for this environment is: "C:\\Users\\GiGaGon\\AppData\\Roaming\\uv\\python\\cpython-3.14.0-windows-x86_64-none\\Lib"
2026-01-05 23:17:39.282738500 DEBUG ty:main ty_project::metadata::options: Including `.` in `environment.root`
2026-01-05 23:17:39.282805500 DEBUG ty:main ty_module_resolver::resolve: Adding first-party search path `C:\Users\GiGaGon\AppData\Local\Programs\VSCodium`
2026-01-05 23:17:39.282880400 DEBUG ty:main ty_module_resolver::resolve: Using vendored stdlib
2026-01-05 23:17:39.283307100 DEBUG ty:main ty_module_resolver::resolve: Adding site-packages search path `C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none\Lib\site-packages`
2026-01-05 23:17:39.283393700  INFO ty:main ty_project::metadata::options: Python version: Python 3.14, platform: win32
2026-01-05 23:17:39.283426700 DEBUG ty:main ruff_db::files::file_root: Adding new file root 'C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none\Lib\site-packages' of kind LibrarySearchPath
2026-01-05 23:17:39.283679600 DEBUG ty:main ruff_db::files::file_root: Adding new file root 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium' of kind Project
2026-01-05 23:17:39.283717900 DEBUG ty:main ty_server::session: Registering diagnostic capability with OpenFilesOnly diagnostic mode
2026-01-05 23:17:39.283743100 DEBUG ty:main ty_module_resolver::resolve: Resolving dynamic module resolution paths
2026-01-05 23:17:39.283899700 DEBUG ty:main ty_server::server::main_loop: Processing deferred notification `textDocument/didOpen`
2026-01-05 23:17:39.283965900 DEBUG ty:main notification{method="textDocument/didOpen"}: ty_project: Opening file `d:\python_projects\ty_issues\main.py`
2026-01-05 23:17:39.283975000 DEBUG ty:main notification{method="textDocument/didOpen"}: ty_project: Take open project files
2026-01-05 23:17:39.284009300 DEBUG ty:main notification{method="textDocument/didOpen"}:set_open_files{open_files={file(Id(1000))}}: ty_project: Set open project files (count: 1)
2026-01-05 23:17:39.287123100 DEBUG ty:main client_response{id=1 method="client/registerCapability"}: ty_server::session: Registered dynamic capabilities
2026-01-05 23:17:39.291365600 DEBUG ThreadId(28) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\node_modules' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.294695400 DEBUG ThreadId(36) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\references-view\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.294918500 DEBUG ThreadId(30) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\git\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.295245100 DEBUG ThreadId(38) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\json-language-features\server\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.295839700 DEBUG ThreadId(35) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\php-language-features\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.296037500 DEBUG ThreadId(37) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\gulp\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.296376900 DEBUG ThreadId(38) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\json-language-features\client\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.296495200 DEBUG ThreadId(29) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\css-language-features\server\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.297011100 DEBUG ThreadId(37) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\grunt\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.297138200 DEBUG ThreadId(28) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\typescript-language-features\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.297768400 DEBUG ThreadId(27) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\terminal-suggest\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.298085800 DEBUG ThreadId(33) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\microsoft-authentication\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.298122300 DEBUG ThreadId(29) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\css-language-features\client\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.298128900 DEBUG ThreadId(30) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\extension-editing\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.298807200 DEBUG ThreadId(33) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\mermaid-chat-features\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.299528000 DEBUG ThreadId(34) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\node_modules' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.299777400 DEBUG ThreadId(37) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\github-authentication\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.300142900 DEBUG ThreadId(28) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\tunnel-forwarding\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.300180300 DEBUG ThreadId(36) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\github\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.300439100 DEBUG ThreadId(27) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\simple-browser\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.300584800 DEBUG ThreadId(33) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\merge-conflict\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.300642700 DEBUG ThreadId(37) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\jake\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.300720700 DEBUG ThreadId(35) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\npm\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.301123800 DEBUG ThreadId(30) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\emmet\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.301188300 DEBUG ThreadId(38) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\search-result\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.301542100 DEBUG ThreadId(34) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\git-base\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.302063700 DEBUG ThreadId(36) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\html-language-features\server\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.302640700 DEBUG ThreadId(34) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\html-language-features\client\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.302853900 DEBUG ThreadId(37) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\ipynb\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.303067200 DEBUG ThreadId(33) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\media-preview\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.303951300 DEBUG ThreadId(37) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\configuration-editing\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.304123800 DEBUG ThreadId(27) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\debug-server-ready\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.304706900 DEBUG ThreadId(32) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\markdown-math\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.304800400 DEBUG ThreadId(38) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\debug-auto-launch\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.305104500 DEBUG ThreadId(31) ty_project::walk: Skipping directory 'C:\Users\GiGaGon\AppData\Local\Programs\VSCodium\resources\app\extensions\markdown-language-features\dist' because it is excluded by a default or `src.exclude` pattern
2026-01-05 23:17:39.309983700  INFO ty:worker:1 request{id=1 method="textDocument/diagnostic"}:check_file{file=File(System("d:\\python_projects\\ty_issues\\main.py"))}:Project::index_files{project=VSCodium}: ty_project: Indexed 0 file(s) in 0.022s
2026-01-05 23:17:39.310043400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=1 duration=22.56ms
2026-01-05 23:17:39.515928100 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=2 duration=93.00µs
2026-01-05 23:17:39.591973400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/codeAction" response.id=3 duration=106.10µs
2026-01-05 23:17:39.592193500 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=4 duration=152.00µs
2026-01-05 23:17:39.626765800 DEBUG ty:worker:1 request{id=5 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 21.8µs
2026-01-05 23:17:39.626866600 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=5 duration=172.60µs
2026-01-05 23:17:39.654108400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/semanticTokens/full" response.id=6 duration=12.41ms
2026-01-05 23:17:39.693326000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentHighlight" response.id=7 duration=6.14ms
2026-01-05 23:17:39.843376500 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/codeAction" response.id=8 duration=61.60µs
2026-01-05 23:17:39.886118100 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=9 duration=107.50µs
2026-01-05 23:17:40.296745200 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=10 duration=87.50µs
2026-01-05 23:17:40.297068800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/codeAction" response.id=11 duration=36.80µs
2026-01-05 23:17:40.600412000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=12 duration=93.30µs
2026-01-05 23:17:48.833777300 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `d:\python_projects\ty_issues\main.py`
2026-01-05 23:17:48.834079800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=13 duration=61.30µs
2026-01-05 23:17:48.860370200 DEBUG ty:worker:0 request{id=14 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 43.4µs
2026-01-05 23:17:48.860411000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=14 duration=126.30µs
2026-01-05 23:17:49.140538400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/semanticTokens/full" response.id=15 duration=200.00µs
2026-01-05 23:17:49.200734000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=16 duration=102.60µs
2026-01-05 23:17:49.425629200 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=17 duration=72.60µs
2026-01-05 23:17:49.637815900 DEBUG ty:worker:0 request{id=18 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 11.7µs
2026-01-05 23:17:49.637853100 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=18 duration=93.30µs
2026-01-05 23:20:03.271658500 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=19 duration=101.10µs
```

</details>

<details>
<summary>ty Language Server Trace Logs</summary>

```
[Trace - 11:17:39 PM] Sending request 'initialize - (0)'.
Params: {
    "processId": 130868,
    "clientInfo": {
        "name": "VSCodium",
        "version": "1.107.1"
    },
    "locale": "en",
    "rootPath": null,
    "rootUri": null,
    "capabilities": {
        "workspace": {
            "applyEdit": true,
            "workspaceEdit": {
                "documentChanges": true,
                "resourceOperations": [
                    "create",
                    "rename",
                    "delete"
                ],
                "failureHandling": "textOnlyTransactional",
                "normalizesLineEndings": true,
                "changeAnnotationSupport": {
                    "groupsOnLabel": true
                }
            },
            "configuration": true,
            "didChangeWatchedFiles": {
                "dynamicRegistration": true,
                "relativePatternSupport": true
            },
            "symbol": {
                "dynamicRegistration": true,
                "symbolKind": {
                    "valueSet": [
                        1,
                        2,
                        3,
                        4,
                        5,
                        6,
                        7,
                        8,
                        9,
                        10,
                        11,
                        12,
                        13,
                        14,
                        15,
                        16,
                        17,
                        18,
                        19,
                        20,
                        21,
                        22,
                        23,
                        24,
                        25,
                        26
                    ]
                },
                "tagSupport": {
                    "valueSet": [
                        1
                    ]
                },
                "resolveSupport": {
                    "properties": [
                        "location.range"
                    ]
                }
            },
            "codeLens": {
                "refreshSupport": true
            },
            "executeCommand": {
                "dynamicRegistration": true
            },
            "didChangeConfiguration": {
                "dynamicRegistration": true
            },
            "workspaceFolders": true,
            "foldingRange": {
                "refreshSupport": true
            },
            "semanticTokens": {
                "refreshSupport": true
            },
            "fileOperations": {
                "dynamicRegistration": true,
                "didCreate": true,
                "didRename": true,
                "didDelete": true,
                "willCreate": true,
                "willRename": true,
                "willDelete": true
            },
            "inlineValue": {
                "refreshSupport": true
            },
            "inlayHint": {
                "refreshSupport": true
            },
            "diagnostics": {
                "refreshSupport": true
            }
        },
        "textDocument": {
            "publishDiagnostics": {
                "relatedInformation": true,
                "versionSupport": false,
                "tagSupport": {
                    "valueSet": [
                        1,
                        2
                    ]
                },
                "codeDescriptionSupport": true,
                "dataSupport": true
            },
            "synchronization": {
                "dynamicRegistration": true,
                "willSave": true,
                "willSaveWaitUntil": true,
                "didSave": true
            },
            "completion": {
                "dynamicRegistration": true,
                "contextSupport": true,
                "completionItem": {
                    "snippetSupport": true,
                    "commitCharactersSupport": true,
                    "documentationFormat": [
                        "markdown",
                        "plaintext"
                    ],
                    "deprecatedSupport": true,
                    "preselectSupport": true,
                    "tagSupport": {
                        "valueSet": [
                            1
                        ]
                    },
                    "insertReplaceSupport": true,
                    "resolveSupport": {
                        "properties": [
                            "documentation",
                            "detail",
                            "additionalTextEdits"
                        ]
                    },
                    "insertTextModeSupport": {
                        "valueSet": [
                            1,
                            2
                        ]
                    },
                    "labelDetailsSupport": true
                },
                "insertTextMode": 2,
                "completionItemKind": {
                    "valueSet": [
                        1,
                        2,
                        3,
                        4,
                        5,
                        6,
                        7,
                        8,
                        9,
                        10,
                        11,
                        12,
                        13,
                        14,
                        15,
                        16,
                        17,
                        18,
                        19,
                        20,
                        21,
                        22,
                        23,
                        24,
                        25
                    ]
                },
                "completionList": {
                    "itemDefaults": [
                        "commitCharacters",
                        "editRange",
                        "insertTextFormat",
                        "insertTextMode",
                        "data"
                    ]
                }
            },
            "hover": {
                "dynamicRegistration": true,
                "contentFormat": [
                    "markdown",
                    "plaintext"
                ]
            },
            "signatureHelp": {
                "dynamicRegistration": true,
                "signatureInformation": {
                    "documentationFormat": [
                        "markdown",
                        "plaintext"
                    ],
                    "parameterInformation": {
                        "labelOffsetSupport": true
                    },
                    "activeParameterSupport": true
                },
                "contextSupport": true
            },
            "definition": {
                "dynamicRegistration": true,
                "linkSupport": true
            },
            "references": {
                "dynamicRegistration": true
            },
            "documentHighlight": {
                "dynamicRegistration": true
            },
            "documentSymbol": {
                "dynamicRegistration": true,
                "symbolKind": {
                    "valueSet": [
                        1,
                        2,
                        3,
                        4,
                        5,
                        6,
                        7,
                        8,
                        9,
                        10,
                        11,
                        12,
                        13,
                        14,
                        15,
                        16,
                        17,
                        18,
                        19,
                        20,
                        21,
                        22,
                        23,
                        24,
                        25,
                        26
                    ]
                },
                "hierarchicalDocumentSymbolSupport": true,
                "tagSupport": {
                    "valueSet": [
                        1
                    ]
                },
                "labelSupport": true
            },
            "codeAction": {
                "dynamicRegistration": true,
                "isPreferredSupport": true,
                "disabledSupport": true,
                "dataSupport": true,
                "resolveSupport": {
                    "properties": [
                        "edit"
                    ]
                },
                "codeActionLiteralSupport": {
                    "codeActionKind": {
                        "valueSet": [
                            "",
                            "quickfix",
                            "refactor",
                            "refactor.extract",
                            "refactor.inline",
                            "refactor.rewrite",
                            "source",
                            "source.organizeImports"
                        ]
                    }
                },
                "honorsChangeAnnotations": true
            },
            "codeLens": {
                "dynamicRegistration": true
            },
            "formatting": {
                "dynamicRegistration": true
            },
            "rangeFormatting": {
                "dynamicRegistration": true,
                "rangesSupport": true
            },
            "onTypeFormatting": {
                "dynamicRegistration": true
            },
            "rename": {
                "dynamicRegistration": true,
                "prepareSupport": true,
                "prepareSupportDefaultBehavior": 1,
                "honorsChangeAnnotations": true
            },
            "documentLink": {
                "dynamicRegistration": true,
                "tooltipSupport": true
            },
            "typeDefinition": {
                "dynamicRegistration": true,
                "linkSupport": true
            },
            "implementation": {
                "dynamicRegistration": true,
                "linkSupport": true
            },
            "colorProvider": {
                "dynamicRegistration": true
            },
            "foldingRange": {
                "dynamicRegistration": true,
                "rangeLimit": 5000,
                "lineFoldingOnly": true,
                "foldingRangeKind": {
                    "valueSet": [
                        "comment",
                        "imports",
                        "region"
                    ]
                },
                "foldingRange": {
                    "collapsedText": false
                }
            },
            "declaration": {
                "dynamicRegistration": true,
                "linkSupport": true
            },
            "selectionRange": {
                "dynamicRegistration": true
            },
            "callHierarchy": {
                "dynamicRegistration": true
            },
            "semanticTokens": {
                "dynamicRegistration": true,
                "tokenTypes": [
                    "namespace",
                    "type",
                    "class",
                    "enum",
                    "interface",
                    "struct",
                    "typeParameter",
                    "parameter",
                    "variable",
                    "property",
                    "enumMember",
                    "event",
                    "function",
                    "method",
                    "macro",
                    "keyword",
                    "modifier",
                    "comment",
                    "string",
                    "number",
                    "regexp",
                    "operator",
                    "decorator"
                ],
                "tokenModifiers": [
                    "declaration",
                    "definition",
                    "readonly",
                    "static",
                    "deprecated",
                    "abstract",
                    "async",
                    "modification",
                    "documentation",
                    "defaultLibrary"
                ],
                "formats": [
                    "relative"
                ],
                "requests": {
                    "range": true,
                    "full": {
                        "delta": true
                    }
                },
                "multilineTokenSupport": false,
                "overlappingTokenSupport": false,
                "serverCancelSupport": true,
                "augmentsSyntaxTokens": true
            },
            "linkedEditingRange": {
                "dynamicRegistration": true
            },
            "typeHierarchy": {
                "dynamicRegistration": true
            },
            "inlineValue": {
                "dynamicRegistration": true
            },
            "inlayHint": {
                "dynamicRegistration": true,
                "resolveSupport": {
                    "properties": [
                        "tooltip",
                        "textEdits",
                        "label.tooltip",
                        "label.location",
                        "label.command"
                    ]
                }
            },
            "diagnostic": {
                "dynamicRegistration": true,
                "relatedDocumentSupport": false
            }
        },
        "window": {
            "showMessage": {
                "messageActionItem": {
                    "additionalPropertiesSupport": true
                }
            },
            "showDocument": {
                "support": true
            },
            "workDoneProgress": true
        },
        "general": {
            "staleRequestSupport": {
                "cancel": true,
                "retryOnContentModified": [
                    "textDocument/semanticTokens/full",
                    "textDocument/semanticTokens/range",
                    "textDocument/semanticTokens/full/delta"
                ]
            },
            "regularExpressions": {
                "engine": "ECMAScript",
                "version": "ES2020"
            },
            "markdown": {
                "parser": "marked",
                "version": "1.1.0"
            },
            "positionEncodings": [
                "utf-16"
            ]
        },
        "notebookDocument": {
            "synchronization": {
                "dynamicRegistration": true,
                "executionSummarySupport": true
            }
        }
    },
    "initializationOptions": {
        "logLevel": "trace"
    },
    "trace": "verbose",
    "workspaceFolders": null
}


[Trace - 11:17:39 PM] Received response 'initialize - (0)' in 9ms.
Result: {
    "capabilities": {
        "codeActionProvider": {
            "codeActionKinds": [
                "quickfix"
            ]
        },
        "completionProvider": {
            "triggerCharacters": [
                "."
            ]
        },
        "declarationProvider": true,
        "definitionProvider": true,
        "documentHighlightProvider": true,
        "documentSymbolProvider": true,
        "executeCommandProvider": {
            "commands": [
                "ty.printDebugInformation"
            ],
            "workDoneProgress": false
        },
        "hoverProvider": true,
        "inlayHintProvider": {},
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
        "referencesProvider": true,
        "renameProvider": {
            "prepareProvider": true
        },
        "selectionRangeProvider": true,
        "semanticTokensProvider": {
            "full": true,
            "legend": {
                "tokenModifiers": [
                    "definition",
                    "readonly",
                    "async",
                    "documentation"
                ],
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
                    "typeParameter"
                ]
            },
            "range": true
        },
        "signatureHelpProvider": {
            "retriggerCharacters": [
                ")"
            ],
            "triggerCharacters": [
                "(",
                ","
            ]
        },
        "textDocumentSync": {
            "change": 2,
            "openClose": true
        },
        "typeDefinitionProvider": true,
        "workspaceSymbolProvider": true
    },
    "serverInfo": {
        "name": "ty",
        "version": "0.0.9 (f1652f05d 2026-01-05)"
    }
}


[Trace - 11:17:39 PM] Sending notification 'initialized'.
Params: {}


[Trace - 11:17:39 PM] Sending notification 'textDocument/didOpen'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py",
        "languageId": "python",
        "version": 5,
        "text": "a = 1\r\n\r\na = 1"
    }
}


[Trace - 11:17:39 PM] Received request 'workspace/configuration - (0)'.
Params: {
    "items": [
        {
            "scopeUri": "file:///C:/Users/GiGaGon/AppData/Local/Programs/VSCodium",
            "section": "ty"
        }
    ]
}


[Trace - 11:17:39 PM] Sending response 'workspace/configuration - (0)'. Processing request took 1ms
Result: [
    {
        "configuration": null,
        "configurationFile": null,
        "disableLanguageServices": false,
        "diagnosticMode": "openFilesOnly",
        "showSyntaxErrors": true,
        "inlayHints": {
            "variableTypes": true,
            "callArgumentNames": true
        },
        "completions": {
            "autoImport": true
        },
        "pythonExtension": {
            "activeEnvironment": {
                "version": {
                    "major": 3,
                    "minor": 14,
                    "patch": 0,
                    "sysVersion": "3.14.0 (main, Oct 28 2025, 12:05:23) [MSC v.1944 64 bit (AMD64)]"
                },
                "environment": null,
                "executable": {
                    "uri": "file:///c%3A/Users/GiGaGon/AppData/Roaming/uv/python/cpython-3.14.0-windows-x86_64-none/python.exe",
                    "sysPrefix": "C:\\Users\\GiGaGon\\AppData\\Roaming\\uv\\python\\cpython-3.14.0-windows-x86_64-none"
                }
            }
        }
    }
]


[Trace - 11:17:39 PM] Received request 'client/registerCapability - (1)'.
Params: {
    "registrations": [
        {
            "id": "ty/textDocument/diagnostic",
            "method": "textDocument/diagnostic",
            "registerOptions": {
                "documentSelector": null,
                "identifier": "ty",
                "interFileDependencies": true,
                "workDoneProgress": false,
                "workspaceDiagnostics": false
            }
        },
        {
            "id": "ty/workspace/didChangeWatchedFiles",
            "method": "workspace/didChangeWatchedFiles",
            "registerOptions": {
                "watchers": [
                    {
                        "globPattern": {
                            "baseUri": "file:///C:/Users/GiGaGon/AppData/Local/Programs/VSCodium",
                            "pattern": "**"
                        },
                        "kind": 7
                    },
                    {
                        "globPattern": {
                            "baseUri": "file:///C:/Users/GiGaGon/AppData/Roaming/uv/python/cpython-3.14.0-windows-x86_64-none/Lib/site-packages",
                            "pattern": "**"
                        },
                        "kind": 7
                    }
                ]
            }
        }
    ]
}


[Trace - 11:17:39 PM] Sending response 'client/registerCapability - (1)'. Processing request took 2ms
No result returned.


[Trace - 11:17:39 PM] Sending request 'textDocument/diagnostic - (1)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:39 PM] Received response 'textDocument/diagnostic - (1)' in 24ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 11:17:39 PM] Sending request 'textDocument/diagnostic - (2)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:39 PM] Received response 'textDocument/diagnostic - (2)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 11:17:39 PM] Sending request 'textDocument/codeAction - (3)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 0
        }
    },
    "context": {
        "diagnostics": [],
        "triggerKind": 2
    }
}


[Trace - 11:17:39 PM] Sending request 'textDocument/documentSymbol - (4)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:39 PM] Received response 'textDocument/codeAction - (3)' in 10ms.
No result returned.


[Trace - 11:17:39 PM] Received response 'textDocument/documentSymbol - (4)' in 7ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "a",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "a",
        "range": {
            "end": {
                "character": 5,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        }
    }
]


[Trace - 11:17:39 PM] Sending request 'textDocument/inlayHint - (5)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 2,
            "character": 5
        }
    }
}


[Trace - 11:17:39 PM] Received response 'textDocument/inlayHint - (5)' in 2ms.
Result: []


[Trace - 11:17:39 PM] Sending request 'textDocument/semanticTokens/full - (6)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:39 PM] Received response 'textDocument/semanticTokens/full - (6)' in 15ms.
Result: {
    "data": [
        0,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0,
        2,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0
    ]
}


[Trace - 11:17:39 PM] Sending request 'textDocument/documentHighlight - (7)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    },
    "position": {
        "line": 0,
        "character": 1
    }
}


[Trace - 11:17:39 PM] Received response 'textDocument/documentHighlight - (7)' in 9ms.
Result: [
    {
        "kind": 3,
        "range": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "kind": 3,
        "range": {
            "end": {
                "character": 1,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        }
    }
]


[Trace - 11:17:39 PM] Sending request 'textDocument/codeAction - (8)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 1
        },
        "end": {
            "line": 0,
            "character": 1
        }
    },
    "context": {
        "diagnostics": [],
        "triggerKind": 2
    }
}


[Trace - 11:17:39 PM] Received response 'textDocument/codeAction - (8)' in 0ms.
No result returned.


[Trace - 11:17:39 PM] Sending request 'textDocument/documentSymbol - (9)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:39 PM] Received response 'textDocument/documentSymbol - (9)' in 1ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "a",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "a",
        "range": {
            "end": {
                "character": 5,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        }
    }
]


[Trace - 11:17:40 PM] Sending request 'textDocument/documentSymbol - (10)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:40 PM] Sending request 'textDocument/codeAction - (11)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 1
        },
        "end": {
            "line": 0,
            "character": 1
        }
    },
    "context": {
        "diagnostics": [],
        "triggerKind": 2
    }
}


[Trace - 11:17:40 PM] Received response 'textDocument/documentSymbol - (10)' in 2ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "a",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "a",
        "range": {
            "end": {
                "character": 5,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        }
    }
]


[Trace - 11:17:40 PM] Received response 'textDocument/codeAction - (11)' in 1ms.
No result returned.


[Trace - 11:17:40 PM] Sending request 'textDocument/documentSymbol - (12)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:40 PM] Received response 'textDocument/documentSymbol - (12)' in 0ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "a",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "a",
        "range": {
            "end": {
                "character": 5,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        }
    }
]


[Trace - 11:17:48 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py",
        "version": 6
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 2,
                    "character": 0
                },
                "end": {
                    "line": 2,
                    "character": 1
                }
            },
            "rangeLength": 1,
            "text": "b"
        },
        {
            "range": {
                "start": {
                    "line": 1,
                    "character": 0
                },
                "end": {
                    "line": 1,
                    "character": 0
                }
            },
            "rangeLength": 0,
            "text": "\r\n"
        },
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 5
                },
                "end": {
                    "line": 0,
                    "character": 5
                }
            },
            "rangeLength": 0,
            "text": "\r\n"
        },
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 0
                },
                "end": {
                    "line": 0,
                    "character": 1
                }
            },
            "rangeLength": 1,
            "text": "b"
        }
    ]
}


[Trace - 11:17:48 PM] Sending request 'textDocument/diagnostic - (13)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:48 PM] Received response 'textDocument/diagnostic - (13)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 11:17:48 PM] Sending request 'textDocument/inlayHint - (14)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 4,
            "character": 5
        }
    }
}


[Trace - 11:17:48 PM] Received response 'textDocument/inlayHint - (14)' in 0ms.
Result: []


[Trace - 11:17:49 PM] Sending request 'textDocument/semanticTokens/full - (15)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:49 PM] Received response 'textDocument/semanticTokens/full - (15)' in 0ms.
Result: {
    "data": [
        0,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0,
        4,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0
    ]
}


[Trace - 11:17:49 PM] Sending request 'textDocument/documentSymbol - (16)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:49 PM] Received response 'textDocument/documentSymbol - (16)' in 1ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "b",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "b",
        "range": {
            "end": {
                "character": 5,
                "line": 4
            },
            "start": {
                "character": 0,
                "line": 4
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 4
            },
            "start": {
                "character": 0,
                "line": 4
            }
        }
    }
]


[Trace - 11:17:49 PM] Sending request 'textDocument/documentSymbol - (17)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:17:49 PM] Received response 'textDocument/documentSymbol - (17)' in 0ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "b",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "b",
        "range": {
            "end": {
                "character": 5,
                "line": 4
            },
            "start": {
                "character": 0,
                "line": 4
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 4
            },
            "start": {
                "character": 0,
                "line": 4
            }
        }
    }
]


[Trace - 11:17:49 PM] Sending request 'textDocument/inlayHint - (18)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 4,
            "character": 5
        }
    }
}


[Trace - 11:17:49 PM] Received response 'textDocument/inlayHint - (18)' in 1ms.
Result: []


[Trace - 11:20:03 PM] Sending request 'textDocument/diagnostic - (19)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    }
}


[Trace - 11:20:03 PM] Received response 'textDocument/diagnostic - (19)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}
```

</details>

### Version

ty 0.0.9 VSCode Version 2026.0.0

---

_Renamed from "Replace can work with stale data after server restart" to "Renaming doesn't work sometimes (including ignoring used line ending style)" by @MeGaGiGaGon on 2026-01-06 07:32_

---

_Label `server` added by @AlexWaygood on 2026-01-06 10:02_

---

_Comment by @MichaReiser on 2026-01-06 10:43_

Hmm, this sounds weird indeed. 

Not sure what's going on. It can't be stale state because we don't store any state across restarts. But it clearly suggests that the client/server state go out of sync for some reason.

What I notice in the video is that there are unsaved changes in `main.py` when opening codium. But even that shouldn't cause any issues downstream.

Looking at the server trace. I don't see any `rename` request in the server trace logs. 

---

_Comment by @MeGaGiGaGon on 2026-01-06 16:54_

Messing around some more, it looks like this gets more complicated. The Python extension has it's own sort-of rename built in; if I disable ty and only leave Python, I still get the newline-ignorant renaming, so that issue shouldn't be ty's fault. The strange thing is with just python, renaming numbers still fails, so I have no clue why that is sometimes happens. 

Looking at the server trace I do see the renames showing up, but ty never returns anything:
```
[Trace - 8:46:58 AM] Sending request 'textDocument/rename - (14)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    },
    "position": {
        "line": 1,
        "character": 5
    },
    "newName": "1"
}


[Trace - 8:46:58 AM] Received response 'textDocument/rename - (14)' in 1ms.
No result returned.
```
So what may be happening is that in this case where ty rename doesn't return anything, VSCode instead falls back to the Python extension renamer.

Something else very odd, if I then restart ty using the `ty: Restart Server` command, on rename I get a different rename trace, but still no ty response:
```
[Trace - 8:52:47 AM] Sending request 'textDocument/prepareRename - (23)'.
Params: {
    "textDocument": {
        "uri": "file:///d%3A/python_projects/ty_issues/main.py"
    },
    "position": {
        "line": 1,
        "character": 1
    }
}


[Trace - 8:52:47 AM] Received response 'textDocument/prepareRename - (23)' in 1ms.
No result returned.
```
However for some reason in this state it looks like VSCode doesn't fall back to the Python extension's renamer, since all renames give the "This element can't be renamed" message

---

_Comment by @MichaReiser on 2026-01-06 17:59_

Huh, that's interesting indeed. Did you set the `LSP` setting to `None` for the Python extension? 

---

_Comment by @MeGaGiGaGon on 2026-01-06 18:13_

Looks like I didn't, so that was what was causing the rename to pop up even with ty not responding for the renames, so the newline issue should be with the fallback LSP in the python extension. With that changed it looks like the request is consistently `prepareRename` with no response from ty, but wasn't renaming enabled by default some time ago? So there should be some result, but everything is instead showing the not renamable message.

---

_Comment by @MeGaGiGaGon on 2026-01-06 20:26_

Ok, I have literally no clue what is going on. Switching to my other computer, it looks like there are two separate bugs, since that computer already had the LSP set to none, and now ty is giving `rename` responses with no `prepareRename ` to be seen, even though it should be the exact same setup. I am so confused.
<details>
<summary> `prepareRename` reponse that confuses newlines</summary>

```
[Trace - 12:20:50 PM] Sending request 'textDocument/rename - (83)'.
Params: {
    "textDocument": {
        "uri": "untitled:Untitled-1"
    },
    "position": {
        "line": 1,
        "character": 1
    },
    "newName": "y"
}


[Trace - 12:20:50 PM] Received response 'textDocument/rename - (83)' in 1ms.
Result: {
    "changes": {
        "untitled:Untitled-1": [
            {
                "newText": "y",
                "range": {
                    "end": {
                        "character": 1,
                        "line": 0
                    },
                    "start": {
                        "character": 0,
                        "line": 0
                    }
                }
            },
            {
                "newText": "y",
                "range": {
                    "end": {
                        "character": 1,
                        "line": 1
                    },
                    "start": {
                        "character": 0,
                        "line": 1
                    }
                }
            }
        ]
    }
}
```

</details>

Extra confusingly, that file name in the logs is wrong - I saved it as an actual file. So maybe that's what's going wrong?

Edit: No, looks like that's not it, since the issue still happens after a restart, but the file name is correct now. But only after I did a rename, undid it, switched file, and went back did the issue reappear.

---

_Comment by @MichaReiser on 2026-01-08 13:12_

> Edit: No, looks like that's not it, since the issue still happens after a restart, but the file name is correct now. But only after I did a rename, undid it, switched file, and went back did the issue reappear.

I'm not sure I follow. Can you specify the exact steps to reproduce?

---

_Comment by @MeGaGiGaGon on 2026-01-08 23:35_

I was just able to reproduce it from a clean install.
1. Download and extract the portable version of VSCodium https://vscodium.com/#install
2. Inside the extracted folder, make a `data` directory to put VSCodium in portable mode (should make it so any existing system configurations/installs don't affect this new version)
3. Open using the exe in the folder
4. Install the ty extension from the marketplace (If ty is already installed, you forgot to put it in portable mode (step 2))
5. Optional but to get logs, open VSCodium options and set `Ty: Log Level` to `trace` and `Ty > Trace: Server` to `verbose`
6. Make a new folder somewhere in the computer
7. Open that folder in VSCodium
8. Make a `main.py` in that folder using VSCodium
9. Write
```py
x = 1
x = 2
```
10. Save the file
11. Use F2 to rename `x` to `y` and observe how a newline gets added between the two statements (the bug)
12. Optional but to see the logs, I do ctrl+shift+p then `Ty: Show Client Logs` and copy that as well as Server and Server Trace logs.

Logs from that process:

<details>
</summary>Ty Client Logs</summary>

```
2026-01-08 15:20:33.910 [info] Name: ty
2026-01-08 15:20:33.910 [info] Module: ty
2026-01-08 15:20:33.910 [info] Python extension loading
2026-01-08 15:20:33.910 [info] Waiting for interpreter from python extension.
2026-01-08 15:20:34.581 [info] Python extension loaded
2026-01-08 15:20:34.582 [info] Using interpreter: c:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.2-windows-x86_64-none\python.exe
2026-01-08 15:20:34.582 [info] Initialization options: {
    "logLevel": "trace"
}
2026-01-08 15:20:34.719 [info] Falling back to bundled executable: c:\Users\GiGaGon\Downloads\VSCodium-win32-x64-1.107.18627\data\extensions\astral-sh.ty-2026.2.0-win32-x64\bundled\libs\bin\ty.exe
2026-01-08 15:20:34.725 [info] Found executable at c:\Users\GiGaGon\Downloads\VSCodium-win32-x64-1.107.18627\data\extensions\astral-sh.ty-2026.2.0-win32-x64\bundled\libs\bin\ty.exe
2026-01-08 15:20:34.725 [info] Server run command: c:\Users\GiGaGon\Downloads\VSCodium-win32-x64-1.107.18627\data\extensions\astral-sh.ty-2026.2.0-win32-x64\bundled\libs\bin\ty.exe server
2026-01-08 15:20:34.726 [info] Server: Start requested.
2026-01-08 15:20:35.466 [info] ty server version: 0.0.10 (d18902cdc 2026-01-07)
```

</details>


<details>
<summary>Ty Language Server Logs</summary>

```
2026-01-08 15:20:35.463552500 DEBUG main ty_server::server: Initialization options: InitializationOptions {
    log_level: Some(
        Trace,
    ),
    log_file: None,
    options: ClientOptions {
        global: GlobalOptions {
            diagnostic_mode: None,
            experimental: None,
            show_syntax_errors: None,
        },
        workspace: WorkspaceOptions {
            configuration: None,
            configuration_file: None,
            disable_language_services: None,
            inlay_hints: None,
            completions: None,
            python_extension: None,
        },
        unknown: {},
    },
}
2026-01-08 15:20:35.463849000 DEBUG main ty_server::server: Resolved client capabilities: ["WORKSPACE_DIAGNOSTIC_REFRESH", "INLAY_HINT_REFRESH", "PULL_DIAGNOSTICS", "TYPE_DEFINITION_LINK_SUPPORT", "DEFINITION_LINK_SUPPORT", "DECLARATION_LINK_SUPPORT", "PREFER_MARKDOWN_IN_HOVER", "SIGNATURE_LABEL_OFFSET_SUPPORT", "SIGNATURE_ACTIVE_PARAMETER_SUPPORT", "HIERARCHICAL_DOCUMENT_SYMBOL_SUPPORT", "WORK_DONE_PROGRESS", "FILE_WATCHER_SUPPORT", "RELATIVE_FILE_WATCHER_SUPPORT", "DIAGNOSTIC_DYNAMIC_REGISTRATION", "WORKSPACE_CONFIGURATION", "COMPLETION_ITEM_LABEL_DETAILS_SUPPORT", "DIAGNOSTIC_RELATED_INFORMATION", "PREFER_MARKDOWN_IN_COMPLETION"]
2026-01-08 15:20:35.463870600  INFO main ty_server::server: Version: 0.0.10 (d18902cdc 2026-01-07)
2026-01-08 15:20:35.466813800 DEBUG ty:main ty_server::server::main_loop: Requesting workspace configuration for workspaces
2026-01-08 15:20:35.475658800 DEBUG ty:main ty_server::session: Deferring `textDocument/didOpen` notification until all workspaces are initialized
2026-01-08 15:20:35.478151800 DEBUG ty:main ty_server::session: Deferring `textDocument/documentSymbol` request until all workspaces are initialized
2026-01-08 15:20:35.478551000 DEBUG ty:main ty_server::session: Deferring `textDocument/inlayHint` request until all workspaces are initialized
2026-01-08 15:20:35.479397400 DEBUG ty:main client_response{id=0 method="workspace/configuration"}: ty_server::server::main_loop: Received workspace configurations, initializing workspaces
2026-01-08 15:20:35.479600300 DEBUG ty:main ty_server::session: Initializing workspace `file:///c%3A/Users/GiGaGon/ty_issues`: WorkspaceOptions {
    configuration: None,
    configuration_file: None,
    disable_language_services: Some(
        false,
    ),
    inlay_hints: Some(
        InlayHintOptions {
            variable_types: Some(
                true,
            ),
            call_argument_names: Some(
                true,
            ),
        },
    ),
    completions: Some(
        CompletionOptions {
            auto_import: Some(
                true,
            ),
        },
    ),
    python_extension: Some(
        PythonExtension {
            active_environment: Some(
                ActiveEnvironment {
                    executable: PythonExecutable {
                        uri: Url {
                            scheme: "file",
                            cannot_be_a_base: false,
                            username: "",
                            password: None,
                            host: None,
                            port: None,
                            path: "/c%3A/Users/GiGaGon/AppData/Roaming/uv/python/cpython-3.14.2-windows-x86_64-none/python.exe",
                            query: None,
                            fragment: None,
                        },
                        sys_prefix: "C:\\Users\\GiGaGon\\AppData\\Roaming\\uv\\python\\cpython-3.14.2-windows-x86_64-none",
                    },
                    environment: None,
                    version: Some(
                        EnvironmentVersion {
                            major: 3,
                            minor: 14,
                            patch: 2,
                            sys_version: "3.14.2 (main, Dec  9 2025, 19:03:14) [MSC v.1944 64 bit (AMD64)]",
                        },
                    ),
                },
            ),
        },
    ),
}
2026-01-08 15:20:35.479714400 DEBUG ty:main ty_server::session::options: Using the Python environment selected in your editor in case the configuration doesn't specify a Python environment: C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.2-windows-x86_64-none
2026-01-08 15:20:35.479727800 DEBUG ty:main ty_server::session::options: Using the Python version selected in your editor: 3.14 in case the configuration doesn't specify a Python version
2026-01-08 15:20:35.479790100 DEBUG ty:main ty_project::metadata: Searching for a project in 'c:\Users\GiGaGon\ty_issues'
2026-01-08 15:20:35.480401200 DEBUG ty:main ty_project::metadata: The ancestor directories contain no `pyproject.toml`. Falling back to a virtual project.
2026-01-08 15:20:35.480444600 DEBUG ty:main ty_project::metadata::configuration_file: Searching for a user-level configuration at `C:\Users\GiGaGon\AppData\Roaming\ty\ty.toml`
2026-01-08 15:20:35.481942300  INFO ty:main ty_project::metadata::options: Defaulting to python-platform `win32`
2026-01-08 15:20:35.482174900 DEBUG ty:main ty_python_semantic::site_packages: Attempting to parse virtual environment metadata at 'C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.2-windows-x86_64-none\pyvenv.cfg'
2026-01-08 15:20:35.482335700 DEBUG ty:main ty_python_semantic::site_packages: Searching for site-packages directory in sys.prefix C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.2-windows-x86_64-none
2026-01-08 15:20:35.482405200 DEBUG ty:main ty_python_semantic::site_packages: Resolved site-packages directories for this environment are: ["C:\\Users\\GiGaGon\\AppData\\Roaming\\uv\\python\\cpython-3.14.2-windows-x86_64-none\\Lib\\site-packages"]
2026-01-08 15:20:35.482435900 DEBUG ty:main ty_python_semantic::site_packages: Searching for real stdlib directory in sys.prefix C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.2-windows-x86_64-none
2026-01-08 15:20:35.482557400 DEBUG ty:main ty_python_semantic::site_packages: Resolved real stdlib directory for this environment is: "C:\\Users\\GiGaGon\\AppData\\Roaming\\uv\\python\\cpython-3.14.2-windows-x86_64-none\\Lib"
2026-01-08 15:20:35.482741400 DEBUG ty:main ty_project::metadata::options: Including `.` in `environment.root`
2026-01-08 15:20:35.482842300 DEBUG ty:main ty_module_resolver::resolve: Adding first-party search path `c:\Users\GiGaGon\ty_issues`
2026-01-08 15:20:35.482946200 DEBUG ty:main ty_module_resolver::resolve: Using vendored stdlib
2026-01-08 15:20:35.483428000 DEBUG ty:main ty_module_resolver::resolve: Adding site-packages search path `C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.2-windows-x86_64-none\Lib\site-packages`
2026-01-08 15:20:35.483520400  INFO ty:main ty_project::metadata::options: Python version: Python 3.14, platform: win32
2026-01-08 15:20:35.483569200 DEBUG ty:main ruff_db::files::file_root: Adding new file root 'C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.2-windows-x86_64-none\Lib\site-packages' of kind LibrarySearchPath
2026-01-08 15:20:35.484232000 DEBUG ty:main ruff_db::files::file_root: Adding new file root 'c:\Users\GiGaGon\ty_issues' of kind Project
2026-01-08 15:20:35.484321700 DEBUG ty:main ty_server::session: Registering diagnostic capability with OpenFilesOnly diagnostic mode
2026-01-08 15:20:35.484403000 DEBUG ty:main ty_module_resolver::resolve: Resolving dynamic module resolution paths
2026-01-08 15:20:35.484572300 DEBUG ty:main ty_server::server::main_loop: Processing deferred notification `textDocument/didOpen`
2026-01-08 15:20:35.498602300 DEBUG ty:main notification{method="textDocument/didOpen"}:apply_changes: ty_project: Adding file `c:\Users\GiGaGon\ty_issues\main.py` to project `ty_issues`
2026-01-08 15:20:35.498718900 DEBUG ty:main notification{method="textDocument/didOpen"}: ty_project: Opening file `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:35.498734000 DEBUG ty:main notification{method="textDocument/didOpen"}: ty_project: Take open project files
2026-01-08 15:20:35.498795300 DEBUG ty:main notification{method="textDocument/didOpen"}:set_open_files{open_files={file(Id(1000))}}: ty_project: Set open project files (count: 1)
2026-01-08 15:20:35.498826000 DEBUG ty:main ty_server::server::main_loop: Processing deferred request `textDocument/documentSymbol`
2026-01-08 15:20:35.498911900 DEBUG ty:main ty_server::server::main_loop: Processing deferred request `textDocument/inlayHint`
2026-01-08 15:20:35.499014100 DEBUG ty:main client_response{id=1 method="client/registerCapability"}: ty_server::session: Registered dynamic capabilities
2026-01-08 15:20:35.499165300 DEBUG ty:worker:1 request{id=2 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 63.1µs
2026-01-08 15:20:35.499179300 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=1 duration=344.40µs
2026-01-08 15:20:35.499241000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=2 duration=321.60µs
2026-01-08 15:20:35.506530100  INFO ty:worker:0 request{id=3 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}:Project::index_files{project=ty_issues}: ty_project: Indexed 1 file(s) in 0.007s
2026-01-08 15:20:35.506650000 DEBUG ty:worker:0 request{id=3 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:35.506959700 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=3 duration=7.83ms
2026-01-08 15:20:35.675336500 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:35.675906600 DEBUG ty:worker:3 request{id=4 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:35.704896000 DEBUG ty:worker:3 request{id=4 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_module_resolver::resolve: Module `__builtins__` not found in search paths
2026-01-08 15:20:35.705186900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=4 duration=29.51ms
2026-01-08 15:20:35.712380700 DEBUG ty:worker:1 request{id=6 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 56.4µs
2026-01-08 15:20:35.712437400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=6 duration=187.00µs
2026-01-08 15:20:35.745833400 DEBUG ty:worker:2 request{id=5 method="textDocument/completion"}:map_stub_definition: ty_module_resolver::resolve: Module `_types` not found in search paths
2026-01-08 15:20:35.745901400 DEBUG ty:worker:2 request{id=5 method="textDocument/completion"}:map_stub_definition: ty_module_resolver::resolve: Module `_types` not found while looking in parent dirs (neither stub nor real module file)
2026-01-08 15:20:35.747697400 DEBUG ty:worker:2 request{id=5 method="textDocument/completion"}:all_symbols: ty_module_resolver::resolve: Resolving dynamic module resolution paths
2026-01-08 15:20:35.747940600 DEBUG ty:worker:2 request{id=5 method="textDocument/completion"}:all_symbols: ty_module_resolver::resolve: Module `typing_extensions` not found in search paths
2026-01-08 15:20:35.748274900 DEBUG ty:worker:2 request{id=5 method="textDocument/completion"}:all_symbols: ty_module_resolver::resolve: Module `typing_extensions` not found while looking in parent dirs (stubs not allowed but some shadowing allowed)
2026-01-08 15:20:35.748333200 DEBUG ty:worker:2 request{id=5 method="textDocument/completion"}:all_symbols: ty_module_resolver::list: Listing modules in search path 'c:\Users\GiGaGon\ty_issues'
2026-01-08 15:20:35.748418600 DEBUG ty:worker:2 request{id=5 method="textDocument/completion"}:all_symbols: ty_module_resolver::list: Listing modules in search path 'vendored://stdlib'
2026-01-08 15:20:35.749643100 DEBUG ty:worker:2 request{id=5 method="textDocument/completion"}:all_symbols: ty_module_resolver::list: Listing modules in search path 'C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.2-windows-x86_64-none\Lib\site-packages'
2026-01-08 15:20:35.834003900 DEBUG ty:worker:2 request{id=5 method="textDocument/completion"}: ty_server::server::api: request id=5 method=textDocument/completion was cancelled by salsa, re-queueing for retry
2026-01-08 15:20:35.834121300 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:35.836812200 DEBUG ty:worker:0 request{id=7 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:35.836987000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=7 duration=320.20µs
2026-01-08 15:20:35.847615700 DEBUG ty:worker:3 request{id=5 method="textDocument/completion"}:all_symbols: ty_module_resolver::resolve: Module `typing_extensions` not found while looking in parent dirs (stubs not allowed but some shadowing allowed)
2026-01-08 15:20:35.859095000 DEBUG ty:worker:1 request{id=8 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 11µs
2026-01-08 15:20:35.859147900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=8 duration=105.10µs
2026-01-08 15:20:35.883207200 DEBUG ty:worker:3 request{id=5 method="textDocument/completion"}: ruff_db::parsed: File `vendored://stdlib\typing.pyi` was reparsed after being collected in the current Salsa revision
2026-01-08 15:20:35.886219900 DEBUG ty:worker:3 request{id=5 method="textDocument/completion"}: ruff_db::parsed: File `vendored://stdlib\types.pyi` was reparsed after being collected in the current Salsa revision
2026-01-08 15:20:35.889956700 DEBUG ty:worker:3 request{id=5 method="textDocument/completion"}: ty_server::server::api::requests::completion: Completions request returned 972 suggestions in 53.2142ms
2026-01-08 15:20:35.893276200 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/completion" response.id=5 duration=185.45ms
2026-01-08 15:20:35.949600300 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:35.950284900 DEBUG ty:worker:2 request{id=9 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:35.950529900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=9 duration=362.60µs
2026-01-08 15:20:35.976910400 DEBUG ty:worker:0 request{id=10 method="textDocument/inlayHint"}: ruff_db::parsed: File `vendored://stdlib\ty_extensions.pyi` was reparsed after being collected in the current Salsa revision
2026-01-08 15:20:35.977754500 DEBUG ty:worker:0 request{id=10 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 1 hints in 1.377ms
2026-01-08 15:20:35.977805000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=10 duration=1.48ms
2026-01-08 15:20:36.006627000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/semanticTokens/full" response.id=11 duration=180.30µs
2026-01-08 15:20:36.040166600 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:36.041219800 DEBUG ty:worker:3 request{id=12 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:36.041390000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=12 duration=289.90µs
2026-01-08 15:20:36.066968700 DEBUG ty:worker:2 request{id=13 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 1 hints in 129.9µs
2026-01-08 15:20:36.067030900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=13 duration=246.10µs
2026-01-08 15:20:36.197588500 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:36.198470800 DEBUG ty:worker:0 request{id=14 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:36.198622800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=14 duration=254.50µs
2026-01-08 15:20:36.215134000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=15 duration=122.30µs
2026-01-08 15:20:36.215886600 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/codeAction" response.id=16 duration=98.10µs
2026-01-08 15:20:36.226277600 DEBUG ty:worker:2 request{id=17 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 13.1µs
2026-01-08 15:20:36.226321800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=17 duration=107.20µs
2026-01-08 15:20:36.339417600 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/semanticTokens/full" response.id=18 duration=86.30µs
2026-01-08 15:20:36.378555400 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:36.379425100 DEBUG ty:worker:1 request{id=19 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:36.379540400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=19 duration=210.20µs
2026-01-08 15:20:36.402704000 DEBUG ty:worker:3 request{id=20 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 21.7µs
2026-01-08 15:20:36.402751000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=20 duration=121.60µs
2026-01-08 15:20:36.688486800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/semanticTokens/full" response.id=21 duration=89.70µs
2026-01-08 15:20:36.739437500 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=22 duration=128.00µs
2026-01-08 15:20:36.739541400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=23 duration=52.40µs
2026-01-08 15:20:36.837265000 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:36.837822900 DEBUG ty:worker:3 request{id=24 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:36.837950800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=24 duration=214.00µs
2026-01-08 15:20:36.851679300 DEBUG ty:worker:2 request{id=25 method="textDocument/completion"}:all_symbols: ty_module_resolver::resolve: Module `typing_extensions` not found while looking in parent dirs (stubs not allowed but some shadowing allowed)
2026-01-08 15:20:36.854891900 DEBUG ty:worker:2 request{id=25 method="textDocument/completion"}: ty_server::server::api::requests::completion: Completions request returned 973 suggestions in 4.4019ms
2026-01-08 15:20:36.858203900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/completion" response.id=25 duration=7.77ms
2026-01-08 15:20:36.873899600 DEBUG ty:worker:0 request{id=26 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 15.1µs
2026-01-08 15:20:36.873938800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=26 duration=110.20µs
2026-01-08 15:20:36.956990700 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:36.957576200 DEBUG ty:worker:1 request{id=27 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:36.957699300 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=27 duration=190.40µs
2026-01-08 15:20:36.984211200 DEBUG ty:worker:3 request{id=28 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 31.1µs
2026-01-08 15:20:36.984267900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=28 duration=157.80µs
2026-01-08 15:20:37.134075600 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:37.134621700 DEBUG ty:worker:2 request{id=29 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:37.134782500 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=29 duration=262.20µs
2026-01-08 15:20:37.146383200 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/semanticTokens/full" response.id=30 duration=96.00µs
2026-01-08 15:20:37.160667600 DEBUG ty:worker:1 request{id=31 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 1 hints in 249.2µs
2026-01-08 15:20:37.160752400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=31 duration=391.00µs
2026-01-08 15:20:37.249301700 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:37.249809600 DEBUG ty:worker:3 request{id=32 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:37.249954000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=32 duration=240.40µs
2026-01-08 15:20:37.273845300 DEBUG ty:worker:2 request{id=33 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 1 hints in 205.2µs
2026-01-08 15:20:37.273900400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=33 duration=315.80µs
2026-01-08 15:20:37.437824500 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:37.438204800 DEBUG ty:worker:0 request{id=34 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:37.438357300 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=34 duration=256.50µs
2026-01-08 15:20:37.463736400 DEBUG ty:worker:1 request{id=35 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 20.3µs
2026-01-08 15:20:37.463778900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=35 duration=112.80µs
2026-01-08 15:20:37.561457000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/semanticTokens/full" response.id=36 duration=121.60µs
2026-01-08 15:20:37.704868000 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/codeAction" response.id=37 duration=81.90µs
2026-01-08 15:20:37.796458800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=38 duration=96.00µs
2026-01-08 15:20:37.796818800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=39 duration=56.40µs
2026-01-08 15:20:38.238621800 DEBUG ty:worker:3 request{id=40 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 15.3µs
2026-01-08 15:20:38.238676800 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=40 duration=141.10µs
2026-01-08 15:20:38.253993100 DEBUG     ty:main notification{method="workspace/didChangeWatchedFiles"}: ty_server::server::api::notifications::did_change_watched_files: Applying changes to `c:\Users\GiGaGon\ty_issues`
2026-01-08 15:20:38.255544600 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=41 duration=74.30µs
2026-01-08 15:20:38.789483200 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentHighlight" response.id=42 duration=175.10µs
2026-01-08 15:20:39.401120300 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/codeAction" response.id=43 duration=78.40µs
2026-01-08 15:20:40.404749300 DEBUG     ty:main notification{method="textDocument/didChange"}:apply_changes: ruff_db::files: Updating the revision of `c:\Users\GiGaGon\ty_issues\main.py`
2026-01-08 15:20:40.405069700 DEBUG ty:worker:3 request{id=44 method="textDocument/diagnostic"}:check_file{file=File(System("c:\\Users\\GiGaGon\\ty_issues\\main.py"))}: ty_python_semantic::types: Checking file 'c:\Users\GiGaGon\ty_issues\main.py'
2026-01-08 15:20:40.405179300 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=44 duration=176.30µs
2026-01-08 15:20:40.432342000 DEBUG ty:worker:2 request{id=45 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 23.2µs
2026-01-08 15:20:40.432395900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=45 duration=157.90µs
2026-01-08 15:20:40.669094100 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/codeAction" response.id=46 duration=100.50µs
2026-01-08 15:20:40.716559300 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/semanticTokens/full" response.id=47 duration=127.50µs
2026-01-08 15:20:40.760079900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=48 duration=146.30µs
2026-01-08 15:20:40.760252400 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/documentSymbol" response.id=49 duration=89.60µs
2026-01-08 15:20:41.206192400 DEBUG ty:worker:0 request{id=50 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 20µs
2026-01-08 15:20:41.206248500 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=50 duration=151.80µs
2026-01-08 15:20:41.997058900 DEBUG     ty:main notification{method="workspace/didChangeWatchedFiles"}: ty_server::server::api::notifications::did_change_watched_files: Applying changes to `c:\Users\GiGaGon\ty_issues`
2026-01-08 15:20:41.998335100 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=51 duration=71.50µs
2026-01-08 15:21:03.973451700 DEBUG ty:worker:3 request{id=52 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 0 hints in 18.3µs
2026-01-08 15:21:03.973499900 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/inlayHint" response.id=52 duration=123.80µs
2026-01-08 15:21:04.150882100 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=53 duration=87.60µs
2026-01-08 15:23:55.970648300 DEBUG     ty:main ty_server::server::main_loop: method="textDocument/diagnostic" response.id=54 duration=99.30µs
```

</details>


<details>
<summary> Ty Language Server Trace Logs</summary>

```
[Trace - 3:20:35 PM] Sending request 'initialize - (0)'.
Params: {
    "processId": 28460,
    "clientInfo": {
        "name": "VSCodium",
        "version": "1.107.1"
    },
    "locale": "en",
    "rootPath": "c:\\Users\\GiGaGon\\ty_issues",
    "rootUri": "file:///c%3A/Users/GiGaGon/ty_issues",
    "capabilities": {
        "workspace": {
            "applyEdit": true,
            "workspaceEdit": {
                "documentChanges": true,
                "resourceOperations": [
                    "create",
                    "rename",
                    "delete"
                ],
                "failureHandling": "textOnlyTransactional",
                "normalizesLineEndings": true,
                "changeAnnotationSupport": {
                    "groupsOnLabel": true
                }
            },
            "configuration": true,
            "didChangeWatchedFiles": {
                "dynamicRegistration": true,
                "relativePatternSupport": true
            },
            "symbol": {
                "dynamicRegistration": true,
                "symbolKind": {
                    "valueSet": [
                        1,
                        2,
                        3,
                        4,
                        5,
                        6,
                        7,
                        8,
                        9,
                        10,
                        11,
                        12,
                        13,
                        14,
                        15,
                        16,
                        17,
                        18,
                        19,
                        20,
                        21,
                        22,
                        23,
                        24,
                        25,
                        26
                    ]
                },
                "tagSupport": {
                    "valueSet": [
                        1
                    ]
                },
                "resolveSupport": {
                    "properties": [
                        "location.range"
                    ]
                }
            },
            "codeLens": {
                "refreshSupport": true
            },
            "executeCommand": {
                "dynamicRegistration": true
            },
            "didChangeConfiguration": {
                "dynamicRegistration": true
            },
            "workspaceFolders": true,
            "foldingRange": {
                "refreshSupport": true
            },
            "semanticTokens": {
                "refreshSupport": true
            },
            "fileOperations": {
                "dynamicRegistration": true,
                "didCreate": true,
                "didRename": true,
                "didDelete": true,
                "willCreate": true,
                "willRename": true,
                "willDelete": true
            },
            "inlineValue": {
                "refreshSupport": true
            },
            "inlayHint": {
                "refreshSupport": true
            },
            "diagnostics": {
                "refreshSupport": true
            }
        },
        "textDocument": {
            "publishDiagnostics": {
                "relatedInformation": true,
                "versionSupport": false,
                "tagSupport": {
                    "valueSet": [
                        1,
                        2
                    ]
                },
                "codeDescriptionSupport": true,
                "dataSupport": true
            },
            "synchronization": {
                "dynamicRegistration": true,
                "willSave": true,
                "willSaveWaitUntil": true,
                "didSave": true
            },
            "completion": {
                "dynamicRegistration": true,
                "contextSupport": true,
                "completionItem": {
                    "snippetSupport": true,
                    "commitCharactersSupport": true,
                    "documentationFormat": [
                        "markdown",
                        "plaintext"
                    ],
                    "deprecatedSupport": true,
                    "preselectSupport": true,
                    "tagSupport": {
                        "valueSet": [
                            1
                        ]
                    },
                    "insertReplaceSupport": true,
                    "resolveSupport": {
                        "properties": [
                            "documentation",
                            "detail",
                            "additionalTextEdits"
                        ]
                    },
                    "insertTextModeSupport": {
                        "valueSet": [
                            1,
                            2
                        ]
                    },
                    "labelDetailsSupport": true
                },
                "insertTextMode": 2,
                "completionItemKind": {
                    "valueSet": [
                        1,
                        2,
                        3,
                        4,
                        5,
                        6,
                        7,
                        8,
                        9,
                        10,
                        11,
                        12,
                        13,
                        14,
                        15,
                        16,
                        17,
                        18,
                        19,
                        20,
                        21,
                        22,
                        23,
                        24,
                        25
                    ]
                },
                "completionList": {
                    "itemDefaults": [
                        "commitCharacters",
                        "editRange",
                        "insertTextFormat",
                        "insertTextMode",
                        "data"
                    ]
                }
            },
            "hover": {
                "dynamicRegistration": true,
                "contentFormat": [
                    "markdown",
                    "plaintext"
                ]
            },
            "signatureHelp": {
                "dynamicRegistration": true,
                "signatureInformation": {
                    "documentationFormat": [
                        "markdown",
                        "plaintext"
                    ],
                    "parameterInformation": {
                        "labelOffsetSupport": true
                    },
                    "activeParameterSupport": true
                },
                "contextSupport": true
            },
            "definition": {
                "dynamicRegistration": true,
                "linkSupport": true
            },
            "references": {
                "dynamicRegistration": true
            },
            "documentHighlight": {
                "dynamicRegistration": true
            },
            "documentSymbol": {
                "dynamicRegistration": true,
                "symbolKind": {
                    "valueSet": [
                        1,
                        2,
                        3,
                        4,
                        5,
                        6,
                        7,
                        8,
                        9,
                        10,
                        11,
                        12,
                        13,
                        14,
                        15,
                        16,
                        17,
                        18,
                        19,
                        20,
                        21,
                        22,
                        23,
                        24,
                        25,
                        26
                    ]
                },
                "hierarchicalDocumentSymbolSupport": true,
                "tagSupport": {
                    "valueSet": [
                        1
                    ]
                },
                "labelSupport": true
            },
            "codeAction": {
                "dynamicRegistration": true,
                "isPreferredSupport": true,
                "disabledSupport": true,
                "dataSupport": true,
                "resolveSupport": {
                    "properties": [
                        "edit"
                    ]
                },
                "codeActionLiteralSupport": {
                    "codeActionKind": {
                        "valueSet": [
                            "",
                            "quickfix",
                            "refactor",
                            "refactor.extract",
                            "refactor.inline",
                            "refactor.rewrite",
                            "source",
                            "source.organizeImports"
                        ]
                    }
                },
                "honorsChangeAnnotations": true
            },
            "codeLens": {
                "dynamicRegistration": true
            },
            "formatting": {
                "dynamicRegistration": true
            },
            "rangeFormatting": {
                "dynamicRegistration": true,
                "rangesSupport": true
            },
            "onTypeFormatting": {
                "dynamicRegistration": true
            },
            "rename": {
                "dynamicRegistration": true,
                "prepareSupport": true,
                "prepareSupportDefaultBehavior": 1,
                "honorsChangeAnnotations": true
            },
            "documentLink": {
                "dynamicRegistration": true,
                "tooltipSupport": true
            },
            "typeDefinition": {
                "dynamicRegistration": true,
                "linkSupport": true
            },
            "implementation": {
                "dynamicRegistration": true,
                "linkSupport": true
            },
            "colorProvider": {
                "dynamicRegistration": true
            },
            "foldingRange": {
                "dynamicRegistration": true,
                "rangeLimit": 5000,
                "lineFoldingOnly": true,
                "foldingRangeKind": {
                    "valueSet": [
                        "comment",
                        "imports",
                        "region"
                    ]
                },
                "foldingRange": {
                    "collapsedText": false
                }
            },
            "declaration": {
                "dynamicRegistration": true,
                "linkSupport": true
            },
            "selectionRange": {
                "dynamicRegistration": true
            },
            "callHierarchy": {
                "dynamicRegistration": true
            },
            "semanticTokens": {
                "dynamicRegistration": true,
                "tokenTypes": [
                    "namespace",
                    "type",
                    "class",
                    "enum",
                    "interface",
                    "struct",
                    "typeParameter",
                    "parameter",
                    "variable",
                    "property",
                    "enumMember",
                    "event",
                    "function",
                    "method",
                    "macro",
                    "keyword",
                    "modifier",
                    "comment",
                    "string",
                    "number",
                    "regexp",
                    "operator",
                    "decorator"
                ],
                "tokenModifiers": [
                    "declaration",
                    "definition",
                    "readonly",
                    "static",
                    "deprecated",
                    "abstract",
                    "async",
                    "modification",
                    "documentation",
                    "defaultLibrary"
                ],
                "formats": [
                    "relative"
                ],
                "requests": {
                    "range": true,
                    "full": {
                        "delta": true
                    }
                },
                "multilineTokenSupport": false,
                "overlappingTokenSupport": false,
                "serverCancelSupport": true,
                "augmentsSyntaxTokens": true
            },
            "linkedEditingRange": {
                "dynamicRegistration": true
            },
            "typeHierarchy": {
                "dynamicRegistration": true
            },
            "inlineValue": {
                "dynamicRegistration": true
            },
            "inlayHint": {
                "dynamicRegistration": true,
                "resolveSupport": {
                    "properties": [
                        "tooltip",
                        "textEdits",
                        "label.tooltip",
                        "label.location",
                        "label.command"
                    ]
                }
            },
            "diagnostic": {
                "dynamicRegistration": true,
                "relatedDocumentSupport": false
            }
        },
        "window": {
            "showMessage": {
                "messageActionItem": {
                    "additionalPropertiesSupport": true
                }
            },
            "showDocument": {
                "support": true
            },
            "workDoneProgress": true
        },
        "general": {
            "staleRequestSupport": {
                "cancel": true,
                "retryOnContentModified": [
                    "textDocument/semanticTokens/full",
                    "textDocument/semanticTokens/range",
                    "textDocument/semanticTokens/full/delta"
                ]
            },
            "regularExpressions": {
                "engine": "ECMAScript",
                "version": "ES2020"
            },
            "markdown": {
                "parser": "marked",
                "version": "1.1.0"
            },
            "positionEncodings": [
                "utf-16"
            ]
        },
        "notebookDocument": {
            "synchronization": {
                "dynamicRegistration": true,
                "executionSummarySupport": true
            }
        }
    },
    "initializationOptions": {
        "logLevel": "trace"
    },
    "trace": "verbose",
    "workspaceFolders": [
        {
            "uri": "file:///c%3A/Users/GiGaGon/ty_issues",
            "name": "ty_issues"
        }
    ]
}


[Trace - 3:20:35 PM] Received response 'initialize - (0)' in 23ms.
Result: {
    "capabilities": {
        "codeActionProvider": {
            "codeActionKinds": [
                "quickfix"
            ]
        },
        "completionProvider": {
            "triggerCharacters": [
                "."
            ]
        },
        "declarationProvider": true,
        "definitionProvider": true,
        "documentHighlightProvider": true,
        "documentSymbolProvider": true,
        "executeCommandProvider": {
            "commands": [
                "ty.printDebugInformation"
            ],
            "workDoneProgress": false
        },
        "hoverProvider": true,
        "inlayHintProvider": {},
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
        "referencesProvider": true,
        "renameProvider": {
            "prepareProvider": true
        },
        "selectionRangeProvider": true,
        "semanticTokensProvider": {
            "full": true,
            "legend": {
                "tokenModifiers": [
                    "definition",
                    "readonly",
                    "async",
                    "documentation"
                ],
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
                    "typeParameter"
                ]
            },
            "range": true
        },
        "signatureHelpProvider": {
            "retriggerCharacters": [
                ")"
            ],
            "triggerCharacters": [
                "(",
                ","
            ]
        },
        "textDocumentSync": {
            "change": 2,
            "openClose": true
        },
        "typeDefinitionProvider": true,
        "workspaceSymbolProvider": true
    },
    "serverInfo": {
        "name": "ty",
        "version": "0.0.10 (d18902cdc 2026-01-07)"
    }
}


[Trace - 3:20:35 PM] Sending notification 'initialized'.
Params: {}


[Trace - 3:20:35 PM] Sending notification 'textDocument/didOpen'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "languageId": "python",
        "version": 1,
        "text": ""
    }
}


[Trace - 3:20:35 PM] Sending request 'textDocument/documentSymbol - (1)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:35 PM] Sending request 'textDocument/inlayHint - (2)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 0
        }
    }
}


[Trace - 3:20:35 PM] Received request 'workspace/configuration - (0)'.
Params: {
    "items": [
        {
            "scopeUri": "file:///c%3A/Users/GiGaGon/ty_issues",
            "section": "ty"
        }
    ]
}


[Trace - 3:20:35 PM] Sending response 'workspace/configuration - (0)'. Processing request took 1ms
Result: [
    {
        "configuration": null,
        "configurationFile": null,
        "disableLanguageServices": false,
        "diagnosticMode": "openFilesOnly",
        "showSyntaxErrors": true,
        "inlayHints": {
            "variableTypes": true,
            "callArgumentNames": true
        },
        "completions": {
            "autoImport": true
        },
        "pythonExtension": {
            "activeEnvironment": {
                "version": {
                    "major": 3,
                    "minor": 14,
                    "patch": 2,
                    "sysVersion": "3.14.2 (main, Dec  9 2025, 19:03:14) [MSC v.1944 64 bit (AMD64)]"
                },
                "environment": null,
                "executable": {
                    "uri": "file:///c%3A/Users/GiGaGon/AppData/Roaming/uv/python/cpython-3.14.2-windows-x86_64-none/python.exe",
                    "sysPrefix": "C:\\Users\\GiGaGon\\AppData\\Roaming\\uv\\python\\cpython-3.14.2-windows-x86_64-none"
                }
            }
        }
    }
]


[Trace - 3:20:35 PM] Received request 'client/registerCapability - (1)'.
Params: {
    "registrations": [
        {
            "id": "ty/textDocument/diagnostic",
            "method": "textDocument/diagnostic",
            "registerOptions": {
                "documentSelector": null,
                "identifier": "ty",
                "interFileDependencies": true,
                "workDoneProgress": false,
                "workspaceDiagnostics": false
            }
        },
        {
            "id": "ty/workspace/didChangeWatchedFiles",
            "method": "workspace/didChangeWatchedFiles",
            "registerOptions": {
                "watchers": [
                    {
                        "globPattern": {
                            "baseUri": "file:///C:/Users/GiGaGon/AppData/Roaming/uv/python/cpython-3.14.2-windows-x86_64-none/Lib/site-packages",
                            "pattern": "**"
                        },
                        "kind": 7
                    },
                    {
                        "globPattern": {
                            "baseUri": "file:///C:/Users/GiGaGon/ty_issues",
                            "pattern": "**"
                        },
                        "kind": 7
                    }
                ]
            }
        }
    ]
}


[Trace - 3:20:35 PM] Sending response 'client/registerCapability - (1)'. Processing request took 3ms
No result returned.


[Trace - 3:20:35 PM] Sending request 'textDocument/diagnostic - (3)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:35 PM] Received response 'textDocument/documentSymbol - (1)' in 25ms.
No result returned.


[Trace - 3:20:35 PM] Received response 'textDocument/inlayHint - (2)' in 25ms.
Result: []


[Trace - 3:20:35 PM] Received response 'textDocument/diagnostic - (3)' in 19ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:20:35 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 2
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 0
                },
                "end": {
                    "line": 0,
                    "character": 0
                }
            },
            "rangeLength": 0,
            "text": "x"
        }
    ]
}


[Trace - 3:20:35 PM] Sending request 'textDocument/diagnostic - (4)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:35 PM] Received response 'textDocument/diagnostic - (4)' in 30ms.
Result: {
    "items": [
        {
            "code": "unresolved-reference",
            "codeDescription": {
                "href": "https://ty.dev/rules#unresolved-reference"
            },
            "data": null,
            "message": "Name `x` used when not defined",
            "range": {
                "end": {
                    "character": 1,
                    "line": 0
                },
                "start": {
                    "character": 0,
                    "line": 0
                }
            },
            "relatedInformation": [],
            "severity": 1,
            "source": "ty"
        }
    ],
    "kind": "full",
    "resultId": "3418845420a5faab"
}


[Trace - 3:20:35 PM] Sending request 'textDocument/completion - (5)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "position": {
        "line": 0,
        "character": 1
    },
    "context": {
        "triggerKind": 1
    }
}


[Trace - 3:20:35 PM] Sending request 'textDocument/inlayHint - (6)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 1
        }
    }
}


[Trace - 3:20:35 PM] Received response 'textDocument/inlayHint - (6)' in 2ms.
Result: []


[Trace - 3:20:35 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 3
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 1
                },
                "end": {
                    "line": 0,
                    "character": 1
                }
            },
            "rangeLength": 0,
            "text": " "
        }
    ]
}


[Trace - 3:20:35 PM] Sending request 'textDocument/diagnostic - (7)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "previousResultId": "3418845420a5faab"
}


[Trace - 3:20:35 PM] Received response 'textDocument/diagnostic - (7)' in 6ms.
Result: {
    "kind": "unchanged",
    "resultId": "3418845420a5faab"
}


[Trace - 3:20:35 PM] Sending request 'textDocument/inlayHint - (8)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 2
        }
    }
}


[Trace - 3:20:35 PM] Received response 'textDocument/inlayHint - (8)' in 1ms.
Result: []


[Trace - 3:20:35 PM] Received response 'textDocument/completion - (5)' in 193ms.
Result: {
    "isIncomplete": true,
    "items": [
		// 23k lines of items ommited so I can upload this to GitHub
    ]
}


[Trace - 3:20:35 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 4
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 2
                },
                "end": {
                    "line": 0,
                    "character": 2
                }
            },
            "rangeLength": 0,
            "text": "="
        }
    ]
}


[Trace - 3:20:35 PM] Sending request 'textDocument/diagnostic - (9)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "previousResultId": "3418845420a5faab"
}


[Trace - 3:20:35 PM] Received response 'textDocument/diagnostic - (9)' in 2ms.
Result: {
    "items": [
        {
            "code": "invalid-syntax",
            "data": null,
            "message": "Expected an expression",
            "range": {
                "end": {
                    "character": 3,
                    "line": 0
                },
                "start": {
                    "character": 3,
                    "line": 0
                }
            },
            "relatedInformation": [],
            "severity": 1,
            "source": "ty"
        }
    ],
    "kind": "full",
    "resultId": "5ed3f45ae91cd22b"
}


[Trace - 3:20:35 PM] Sending request 'textDocument/inlayHint - (10)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 3
        }
    }
}


[Trace - 3:20:35 PM] Received response 'textDocument/inlayHint - (10)' in 2ms.
Result: [
    {
        "kind": 1,
        "label": [
            {
                "value": ": "
            },
            {
                "location": {
                    "range": {
                        "end": {
                            "character": 7,
                            "line": 13
                        },
                        "start": {
                            "character": 0,
                            "line": 13
                        }
                    },
                    "uri": "file:///C:/Users/GiGaGon/AppData/Local/ty/cache/vendored/typeshed/d1d5fe58664b30a0c2dde3cd5c3dc8091f0f16ae/stdlib/ty_extensions.pyi"
                },
                "value": "Unknown"
            }
        ],
        "position": {
            "character": 1,
            "line": 0
        },
        "textEdits": [
            {
                "newText": ": Unknown",
                "range": {
                    "end": {
                        "character": 1,
                        "line": 0
                    },
                    "start": {
                        "character": 1,
                        "line": 0
                    }
                }
            }
        ]
    }
]


[Trace - 3:20:36 PM] Sending request 'textDocument/semanticTokens/full - (11)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/semanticTokens/full - (11)' in 1ms.
Result: {
    "data": [
        0,
        0,
        1,
        5,
        1
    ]
}


[Trace - 3:20:36 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 5
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 3
                },
                "end": {
                    "line": 0,
                    "character": 3
                }
            },
            "rangeLength": 0,
            "text": " "
        }
    ]
}


[Trace - 3:20:36 PM] Sending request 'textDocument/diagnostic - (12)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "previousResultId": "5ed3f45ae91cd22b"
}


[Trace - 3:20:36 PM] Received response 'textDocument/diagnostic - (12)' in 2ms.
Result: {
    "items": [
        {
            "code": "invalid-syntax",
            "data": null,
            "message": "Expected an expression",
            "range": {
                "end": {
                    "character": 4,
                    "line": 0
                },
                "start": {
                    "character": 4,
                    "line": 0
                }
            },
            "relatedInformation": [],
            "severity": 1,
            "source": "ty"
        }
    ],
    "kind": "full",
    "resultId": "e8f06aa0ee903f9f"
}


[Trace - 3:20:36 PM] Sending request 'textDocument/inlayHint - (13)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 4
        }
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/inlayHint - (13)' in 1ms.
Result: [
    {
        "kind": 1,
        "label": [
            {
                "value": ": "
            },
            {
                "location": {
                    "range": {
                        "end": {
                            "character": 7,
                            "line": 13
                        },
                        "start": {
                            "character": 0,
                            "line": 13
                        }
                    },
                    "uri": "file:///C:/Users/GiGaGon/AppData/Local/ty/cache/vendored/typeshed/d1d5fe58664b30a0c2dde3cd5c3dc8091f0f16ae/stdlib/ty_extensions.pyi"
                },
                "value": "Unknown"
            }
        ],
        "position": {
            "character": 1,
            "line": 0
        },
        "textEdits": [
            {
                "newText": ": Unknown",
                "range": {
                    "end": {
                        "character": 1,
                        "line": 0
                    },
                    "start": {
                        "character": 1,
                        "line": 0
                    }
                }
            }
        ]
    }
]


[Trace - 3:20:36 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 6
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 4
                },
                "end": {
                    "line": 0,
                    "character": 4
                }
            },
            "rangeLength": 0,
            "text": "1"
        }
    ]
}


[Trace - 3:20:36 PM] Sending request 'textDocument/diagnostic - (14)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "previousResultId": "e8f06aa0ee903f9f"
}


[Trace - 3:20:36 PM] Received response 'textDocument/diagnostic - (14)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:20:36 PM] Sending request 'textDocument/documentSymbol - (15)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:36 PM] Sending request 'textDocument/codeAction - (16)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 5
        },
        "end": {
            "line": 0,
            "character": 5
        }
    },
    "context": {
        "diagnostics": [],
        "triggerKind": 2
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/documentSymbol - (15)' in 2ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "x",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    }
]


[Trace - 3:20:36 PM] Received response 'textDocument/codeAction - (16)' in 2ms.
No result returned.


[Trace - 3:20:36 PM] Sending request 'textDocument/inlayHint - (17)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 5
        }
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/inlayHint - (17)' in 0ms.
Result: []


[Trace - 3:20:36 PM] Sending request 'textDocument/semanticTokens/full - (18)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/semanticTokens/full - (18)' in 0ms.
Result: {
    "data": [
        0,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0
    ]
}


[Trace - 3:20:36 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 7
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 5
                },
                "end": {
                    "line": 0,
                    "character": 5
                }
            },
            "rangeLength": 0,
            "text": "\r\n"
        }
    ]
}


[Trace - 3:20:36 PM] Sending request 'textDocument/diagnostic - (19)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/diagnostic - (19)' in 0ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:20:36 PM] Sending request 'textDocument/inlayHint - (20)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 1,
            "character": 0
        }
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/inlayHint - (20)' in 1ms.
Result: []


[Trace - 3:20:36 PM] Sending request 'textDocument/semanticTokens/full - (21)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/semanticTokens/full - (21)' in 0ms.
Result: {
    "data": [
        0,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0
    ]
}


[Trace - 3:20:36 PM] Sending request 'textDocument/documentSymbol - (22)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:36 PM] Sending request 'textDocument/documentSymbol - (23)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/documentSymbol - (22)' in 1ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "x",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    }
]


[Trace - 3:20:36 PM] Received response 'textDocument/documentSymbol - (23)' in 1ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "x",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    }
]


[Trace - 3:20:36 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 8
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 1,
                    "character": 0
                },
                "end": {
                    "line": 1,
                    "character": 0
                }
            },
            "rangeLength": 0,
            "text": "x"
        }
    ]
}


[Trace - 3:20:36 PM] Sending request 'textDocument/diagnostic - (24)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/diagnostic - (24)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:20:36 PM] Sending request 'textDocument/completion - (25)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "position": {
        "line": 1,
        "character": 1
    },
    "context": {
        "triggerKind": 1
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/completion - (25)' in 16ms.
Result: {
    "isIncomplete": true,
    "items": [
		// Another 23k lines of items ommited so I can upload this to GitHub
    ]
}


[Trace - 3:20:36 PM] Sending request 'textDocument/inlayHint - (26)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 1,
            "character": 1
        }
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/inlayHint - (26)' in 1ms.
Result: []


[Trace - 3:20:36 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 9
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 1,
                    "character": 1
                },
                "end": {
                    "line": 1,
                    "character": 1
                }
            },
            "rangeLength": 0,
            "text": " "
        }
    ]
}


[Trace - 3:20:36 PM] Sending request 'textDocument/diagnostic - (27)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/diagnostic - (27)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:20:36 PM] Sending request 'textDocument/inlayHint - (28)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 1,
            "character": 2
        }
    }
}


[Trace - 3:20:36 PM] Received response 'textDocument/inlayHint - (28)' in 1ms.
Result: []


[Trace - 3:20:37 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 10
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 1,
                    "character": 2
                },
                "end": {
                    "line": 1,
                    "character": 2
                }
            },
            "rangeLength": 0,
            "text": "="
        }
    ]
}


[Trace - 3:20:37 PM] Sending request 'textDocument/diagnostic - (29)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:37 PM] Received response 'textDocument/diagnostic - (29)' in 1ms.
Result: {
    "items": [
        {
            "code": "invalid-syntax",
            "data": null,
            "message": "Expected an expression",
            "range": {
                "end": {
                    "character": 3,
                    "line": 1
                },
                "start": {
                    "character": 3,
                    "line": 1
                }
            },
            "relatedInformation": [],
            "severity": 1,
            "source": "ty"
        }
    ],
    "kind": "full",
    "resultId": "fde1a725c5855b14"
}


[Trace - 3:20:37 PM] Sending request 'textDocument/semanticTokens/full - (30)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:37 PM] Received response 'textDocument/semanticTokens/full - (30)' in 1ms.
Result: {
    "data": [
        0,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0,
        1,
        0,
        1,
        5,
        1
    ]
}


[Trace - 3:20:37 PM] Sending request 'textDocument/inlayHint - (31)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 1,
            "character": 3
        }
    }
}


[Trace - 3:20:37 PM] Received response 'textDocument/inlayHint - (31)' in 1ms.
Result: [
    {
        "kind": 1,
        "label": [
            {
                "value": ": "
            },
            {
                "location": {
                    "range": {
                        "end": {
                            "character": 7,
                            "line": 13
                        },
                        "start": {
                            "character": 0,
                            "line": 13
                        }
                    },
                    "uri": "file:///C:/Users/GiGaGon/AppData/Local/ty/cache/vendored/typeshed/d1d5fe58664b30a0c2dde3cd5c3dc8091f0f16ae/stdlib/ty_extensions.pyi"
                },
                "value": "Unknown"
            }
        ],
        "position": {
            "character": 1,
            "line": 1
        },
        "textEdits": [
            {
                "newText": ": Unknown",
                "range": {
                    "end": {
                        "character": 1,
                        "line": 1
                    },
                    "start": {
                        "character": 1,
                        "line": 1
                    }
                }
            }
        ]
    }
]


[Trace - 3:20:37 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 11
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 1,
                    "character": 3
                },
                "end": {
                    "line": 1,
                    "character": 3
                }
            },
            "rangeLength": 0,
            "text": " "
        }
    ]
}


[Trace - 3:20:37 PM] Sending request 'textDocument/diagnostic - (32)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "previousResultId": "fde1a725c5855b14"
}


[Trace - 3:20:37 PM] Received response 'textDocument/diagnostic - (32)' in 1ms.
Result: {
    "items": [
        {
            "code": "invalid-syntax",
            "data": null,
            "message": "Expected an expression",
            "range": {
                "end": {
                    "character": 4,
                    "line": 1
                },
                "start": {
                    "character": 4,
                    "line": 1
                }
            },
            "relatedInformation": [],
            "severity": 1,
            "source": "ty"
        }
    ],
    "kind": "full",
    "resultId": "f658db62612936d7"
}


[Trace - 3:20:37 PM] Sending request 'textDocument/inlayHint - (33)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 1,
            "character": 4
        }
    }
}


[Trace - 3:20:37 PM] Received response 'textDocument/inlayHint - (33)' in 1ms.
Result: [
    {
        "kind": 1,
        "label": [
            {
                "value": ": "
            },
            {
                "location": {
                    "range": {
                        "end": {
                            "character": 7,
                            "line": 13
                        },
                        "start": {
                            "character": 0,
                            "line": 13
                        }
                    },
                    "uri": "file:///C:/Users/GiGaGon/AppData/Local/ty/cache/vendored/typeshed/d1d5fe58664b30a0c2dde3cd5c3dc8091f0f16ae/stdlib/ty_extensions.pyi"
                },
                "value": "Unknown"
            }
        ],
        "position": {
            "character": 1,
            "line": 1
        },
        "textEdits": [
            {
                "newText": ": Unknown",
                "range": {
                    "end": {
                        "character": 1,
                        "line": 1
                    },
                    "start": {
                        "character": 1,
                        "line": 1
                    }
                }
            }
        ]
    }
]


[Trace - 3:20:37 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 12
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 1,
                    "character": 4
                },
                "end": {
                    "line": 1,
                    "character": 4
                }
            },
            "rangeLength": 0,
            "text": "2"
        }
    ]
}


[Trace - 3:20:37 PM] Sending request 'textDocument/diagnostic - (34)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "previousResultId": "f658db62612936d7"
}


[Trace - 3:20:37 PM] Received response 'textDocument/diagnostic - (34)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:20:37 PM] Sending request 'textDocument/inlayHint - (35)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 1,
            "character": 5
        }
    }
}


[Trace - 3:20:37 PM] Received response 'textDocument/inlayHint - (35)' in 1ms.
Result: []


[Trace - 3:20:37 PM] Sending request 'textDocument/semanticTokens/full - (36)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:37 PM] Received response 'textDocument/semanticTokens/full - (36)' in 2ms.
Result: {
    "data": [
        0,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0,
        1,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0
    ]
}


[Trace - 3:20:37 PM] Sending request 'textDocument/codeAction - (37)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 1,
            "character": 5
        },
        "end": {
            "line": 1,
            "character": 5
        }
    },
    "context": {
        "diagnostics": [],
        "triggerKind": 2
    }
}


[Trace - 3:20:37 PM] Received response 'textDocument/codeAction - (37)' in 1ms.
No result returned.


[Trace - 3:20:37 PM] Sending request 'textDocument/documentSymbol - (38)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:37 PM] Sending request 'textDocument/documentSymbol - (39)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:37 PM] Received response 'textDocument/documentSymbol - (38)' in 2ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "x",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "x",
        "range": {
            "end": {
                "character": 5,
                "line": 1
            },
            "start": {
                "character": 0,
                "line": 1
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 1
            },
            "start": {
                "character": 0,
                "line": 1
            }
        }
    }
]


[Trace - 3:20:37 PM] Received response 'textDocument/documentSymbol - (39)' in 1ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "x",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "x",
        "range": {
            "end": {
                "character": 5,
                "line": 1
            },
            "start": {
                "character": 0,
                "line": 1
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 1
            },
            "start": {
                "character": 0,
                "line": 1
            }
        }
    }
]


[Trace - 3:20:38 PM] Sending request 'textDocument/inlayHint - (40)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 1,
            "character": 5
        }
    }
}


[Trace - 3:20:38 PM] Received response 'textDocument/inlayHint - (40)' in 1ms.
Result: []


[Trace - 3:20:38 PM] Sending notification 'workspace/didChangeWatchedFiles'.
Params: {
    "changes": [
        {
            "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
            "type": 2
        }
    ]
}


[Trace - 3:20:38 PM] Received request 'workspace/diagnostic/refresh - (2)'.
[Trace - 3:20:38 PM] Sending response 'workspace/diagnostic/refresh - (2)'. Processing request took 0ms
No result returned.


[Trace - 3:20:38 PM] Sending request 'textDocument/diagnostic - (41)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:38 PM] Received request 'workspace/inlayHint/refresh - (3)'.
[Trace - 3:20:38 PM] Sending response 'workspace/inlayHint/refresh - (3)'. Processing request took 0ms
No result returned.


[Trace - 3:20:38 PM] Received response 'textDocument/diagnostic - (41)' in 2ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:20:38 PM] Sending request 'textDocument/documentHighlight - (42)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "position": {
        "line": 1,
        "character": 4
    }
}


[Trace - 3:20:38 PM] Received response 'textDocument/documentHighlight - (42)' in 0ms.
No result returned.


[Trace - 3:20:39 PM] Sending request 'textDocument/codeAction - (43)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 1,
            "character": 1
        },
        "end": {
            "line": 1,
            "character": 1
        }
    },
    "context": {
        "diagnostics": [],
        "triggerKind": 2
    }
}


[Trace - 3:20:39 PM] Received response 'textDocument/codeAction - (43)' in 1ms.
No result returned.


[Trace - 3:20:40 PM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
        "version": 13
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 1,
                    "character": 0
                },
                "end": {
                    "line": 1,
                    "character": 1
                }
            },
            "rangeLength": 1,
            "text": "y"
        },
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 5
                },
                "end": {
                    "line": 0,
                    "character": 5
                }
            },
            "rangeLength": 0,
            "text": "\r\n"
        },
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 0
                },
                "end": {
                    "line": 0,
                    "character": 1
                }
            },
            "rangeLength": 1,
            "text": "y"
        }
    ]
}


[Trace - 3:20:40 PM] Sending request 'textDocument/diagnostic - (44)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:40 PM] Received response 'textDocument/diagnostic - (44)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:20:40 PM] Sending request 'textDocument/inlayHint - (45)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 2,
            "character": 5
        }
    }
}


[Trace - 3:20:40 PM] Received response 'textDocument/inlayHint - (45)' in 1ms.
Result: []


[Trace - 3:20:40 PM] Sending request 'textDocument/codeAction - (46)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 2,
            "character": 1
        },
        "end": {
            "line": 2,
            "character": 1
        }
    },
    "context": {
        "diagnostics": [],
        "triggerKind": 2
    }
}


[Trace - 3:20:40 PM] Received response 'textDocument/codeAction - (46)' in 1ms.
No result returned.


[Trace - 3:20:40 PM] Sending request 'textDocument/semanticTokens/full - (47)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:40 PM] Received response 'textDocument/semanticTokens/full - (47)' in 0ms.
Result: {
    "data": [
        0,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0,
        2,
        0,
        1,
        5,
        1,
        0,
        4,
        1,
        11,
        0
    ]
}


[Trace - 3:20:40 PM] Sending request 'textDocument/documentSymbol - (48)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:40 PM] Sending request 'textDocument/documentSymbol - (49)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:40 PM] Received response 'textDocument/documentSymbol - (48)' in 1ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "y",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "y",
        "range": {
            "end": {
                "character": 5,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        }
    }
]


[Trace - 3:20:40 PM] Received response 'textDocument/documentSymbol - (49)' in 1ms.
Result: [
    {
        "children": [],
        "kind": 13,
        "name": "y",
        "range": {
            "end": {
                "character": 5,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    },
    {
        "children": [],
        "kind": 13,
        "name": "y",
        "range": {
            "end": {
                "character": 5,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        },
        "selectionRange": {
            "end": {
                "character": 1,
                "line": 2
            },
            "start": {
                "character": 0,
                "line": 2
            }
        }
    }
]


[Trace - 3:20:41 PM] Sending request 'textDocument/inlayHint - (50)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 2,
            "character": 5
        }
    }
}


[Trace - 3:20:41 PM] Received response 'textDocument/inlayHint - (50)' in 1ms.
Result: []


[Trace - 3:20:41 PM] Sending notification 'workspace/didChangeWatchedFiles'.
Params: {
    "changes": [
        {
            "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py",
            "type": 2
        }
    ]
}


[Trace - 3:20:41 PM] Received request 'workspace/diagnostic/refresh - (4)'.
[Trace - 3:20:41 PM] Sending response 'workspace/diagnostic/refresh - (4)'. Processing request took 0ms
No result returned.


[Trace - 3:20:41 PM] Sending request 'textDocument/diagnostic - (51)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:20:41 PM] Received request 'workspace/inlayHint/refresh - (5)'.
[Trace - 3:20:41 PM] Sending response 'workspace/inlayHint/refresh - (5)'. Processing request took 0ms
No result returned.


[Trace - 3:20:41 PM] Received response 'textDocument/diagnostic - (51)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:21:03 PM] Sending request 'textDocument/inlayHint - (52)'.
Params: {
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 2,
            "character": 5
        }
    }
}


[Trace - 3:21:03 PM] Received response 'textDocument/inlayHint - (52)' in 1ms.
Result: []


[Trace - 3:21:04 PM] Sending request 'textDocument/diagnostic - (53)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:21:04 PM] Received response 'textDocument/diagnostic - (53)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:23:55 PM] Sending request 'textDocument/diagnostic - (54)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:23:55 PM] Received response 'textDocument/diagnostic - (54)' in 1ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 3:24:40 PM] Sending request 'textDocument/diagnostic - (55)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "file:///c%3A/Users/GiGaGon/ty_issues/main.py"
    }
}


[Trace - 3:24:40 PM] Received response 'textDocument/diagnostic - (55)' in 2ms.
Result: {
    "items": [],
    "kind": "full"
}
```

</details>

After this, still in that same clean install, if I set `Python: Language Server` to `None` in the Python extension settings the bug stops happening.

---

_Comment by @MichaReiser on 2026-01-09 07:52_

> After this, still in that same clean install, if I set Python: Language Server to None in the Python extension settings the bug stops happening.

That suggests that it's a Python LSP bug and not an issue with ty or some bad interaction between the two.

---

_Comment by @MeGaGiGaGon on 2026-01-09 19:38_

On doing a clean install of basedpyright to test if this was unique to ty, when I got to checking `Python: Language Server` I noticed it was already `None`. On further investigation I think I found how basedpyright does it, so I just opened astral-sh/ty-vscode#283 which should hopefully fix the problem.

---

_Comment by @michaelfortunato on 2026-01-10 02:02_

The rename code action is broken in my neovim as well.
I find the buffer only updates on file save, whwereas id expect the buffer to update immediately after the rename change completed. Also im not oositive if the rename happens reliably on file save. Ill share a video and some steps to reproduce. 

---

_Comment by @MeGaGiGaGon on 2026-01-10 02:09_

@michaelfortunato As I just figured out above, this issue is VSCode specific, and will hopefully be closed soon with the fix I found for it. Please open a new issue with your video/repro steps, since it's almost certainly unrelated to this.

---

_Renamed from "Renaming doesn't work sometimes (including ignoring used line ending style)" to "VSCode extension: Renaming doesn't work sometimes (including ignoring used line ending style)" by @MeGaGiGaGon on 2026-01-10 02:19_

---
