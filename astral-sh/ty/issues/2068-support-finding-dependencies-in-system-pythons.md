```yaml
number: 2068
title: Support finding dependencies in system Pythons that ty is installed into
type: issue
state: open
author: leet0rz
labels:
  - imports
assignees: []
created_at: 2025-12-18T13:02:32Z
updated_at: 2026-01-07T22:35:43Z
url: https://github.com/astral-sh/ty/issues/2068
synced_at: 2026-01-12T15:54:26Z
```

# Support finding dependencies in system Pythons that ty is installed into

---

_@leet0rz_

### Summary

This is just neovim with ty LSP attached to the buffer and this error shows up:

<img width="725" height="197" alt="Image" src="https://github.com/user-attachments/assets/e6c345d9-4f86-4132-96ab-33e3f971395d" />

This does not happen with any of the other major LSPs, let me know if there is a fix for this.

### Version

0.0.3

---

_Label `question` added by @sharkdp on 2025-12-18 13:03_

---

_Comment by @sharkdp on 2025-12-18 13:04_

Thank you for reporting this.

Have you seen [this FAQ entry](https://docs.astral.sh/ty/reference/typing-faq/#why-cant-ty-resolve-my-imports)? (let us know if that is not enough to resolve your issue).

---

_Comment by @leet0rz on 2025-12-18 18:17_

> Thank you for reporting this.
> 
> Have you seen [this FAQ entry](https://docs.astral.sh/ty/reference/typing-faq/#why-cant-ty-resolve-my-imports)? (let us know if that is not enough to resolve your issue).

The libs are globally installed on the computer and sits in python313/lib/site-packages, these are not installed in any virtual env, these are global.

---

_Comment by @carljm on 2025-12-18 18:26_

Is ty installed in the same system Python that these packages are installed in?

---

_Comment by @sharkdp on 2025-12-18 18:27_

Can you tell us how you run ty, and maybe run it with `-v`, to see the search paths?

---

_Comment by @AlexWaygood on 2025-12-18 18:29_

We don't support automatically resolving imports from a non-virtual (global/system) environment. If you point us to a non-virtual environment using `--python`, we will resolve imports from that environment, but we'd encourage you to use a virtual environment instead. See https://github.com/astral-sh/ty/issues/684 for some previous discussion.

---

_Comment by @carljm on 2025-12-18 18:31_

> If you point us to a non-virtual environment using `--python`

Or if ty itself is installed in a system Python, we should use that system Python environment

---

_Comment by @leet0rz on 2025-12-18 19:38_

> Is ty installed in the same system Python that these packages are installed in?

Yes, ty sits in `python313/scripts/ty`

---

_Comment by @leet0rz on 2025-12-18 19:40_

> Can you tell us how you run ty, and maybe run it with `-v`, to see the search paths?

This is how I've configured it in neovim:
```lua
vim.lsp.config("ty", {
    cmd = { "ty", "server" },
    filetypes = { "python" },
    root_markers = { "pyproject.toml", "ruff.toml", ".ruff.toml", ".git" },
})
```
should I add "-v" in the cmd there you mean? when I do `ty -v` I just get this:
```
ty -v
error: unexpected argument '-v' found
```

---

_Comment by @leet0rz on 2025-12-18 19:42_

> We don't support automatically resolving imports from a non-virtual (global/system) environment. If you point us to a non-virtual environment using `--python`, we will resolve imports from that environment, but we'd encourage you to use a virtual environment instead. See [#684](https://github.com/astral-sh/ty/issues/684) for some previous discussion.

But that is not always going to be the case for every use case, not everyone is running virtual envs every time in every project, I highly recommend adding support for this as every other LSP has and does. Thanks.

---

_Comment by @carljm on 2025-12-23 00:37_

I don't think we've actually identified yet what the problem is here, because if ty is installed in a system Python, we should be able to see packages installed in that Python.

Are you able to reproduce the failed-imports problem running `ty check` on the command line, on the same code, from the same directory that your `root_markers` would cause nvim to pick as the project root? If so, adding `-v` to that `ty check` will generate some useful logs that might help us figure out what's causing the issue.

---

_Label `imports` added by @carljm on 2025-12-23 00:40_

---

_Label `needs-mre` added by @dhruvmanila on 2025-12-23 05:00_

---

_Comment by @dhruvmanila on 2025-12-23 05:08_

> This is how I've configured it in neovim:
> ```lua
> vim.lsp.config("ty", {
>     cmd = { "ty", "server" },
>     filetypes = { "python" },
>     root_markers = { "pyproject.toml", "ruff.toml", ".ruff.toml", ".git" },
> })
> ```

This suggests that `ty` executable should be available globally on `PATH`, can you confirm whether the `ty` that's being run in Neovim is the same as `python313/scripts/ty`? Can you provide debug server logs? You can use the following config to enable debug logs and provide the content of `path/to/ty_server.log`:

```lua
vim.lsp.config("ty", {
    cmd = { "ty", "server" },
    filetypes = { "python" },
    root_markers = { "pyproject.toml", "ruff.toml", ".ruff.toml", ".git" },
	init_options = {
	  logLevel = 'debug',
	  logFile = 'path/to/ty_server.log',
	},
})
```

---

_Comment by @leet0rz on 2025-12-23 10:11_

> I don't think we've actually identified yet what the problem is here, because if ty is installed in a system Python, we should be able to see packages installed in that Python.
> 
> Are you able to reproduce the failed-imports problem running `ty check` on the command line, on the same code, from the same directory that your `root_markers` would cause nvim to pick as the project root? If so, adding `-v` to that `ty check` will generate some useful logs that might help us figure out what's causing the issue.

```
✗  ty check -v
INFO Defaulting to python-platform `win32`
INFO Python version: Python 3.14, platform: win32
INFO Indexed 1 file(s) in 0.008s
error[unresolved-import]: Cannot resolve imported module `requests`
 --> prisjakt.py:1:8
  |
1 | import requests
  |        ^^^^^^^^
2 | import time
  |
info: Searched in the following paths during module resolution:
info:   1. D:\Aleksander2\Programming\Python\Projects\PriceTracker (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

---

_Comment by @leet0rz on 2025-12-23 10:23_

> > This is how I've configured it in neovim:
> > vim.lsp.config("ty", {
> >     cmd = { "ty", "server" },
> >     filetypes = { "python" },
> >     root_markers = { "pyproject.toml", "ruff.toml", ".ruff.toml", ".git" },
> > })
> 
> This suggests that `ty` executable should be available globally on `PATH`, can you confirm whether the `ty` that's being run in Neovim is the same as `python313/scripts/ty`? Can you provide debug server logs? You can use the following config to enable debug logs and provide the content of `path/to/ty_server.log`:
> 
> vim.lsp.config("ty", {
>     cmd = { "ty", "server" },
>     filetypes = { "python" },
>     root_markers = { "pyproject.toml", "ruff.toml", ".ruff.toml", ".git" },
> 	init_options = {
> 	  logLevel = 'debug',
> 	  logFile = 'path/to/ty_server.log',
> 	},
> })

Yes, it is in fact global and it's the same path. Here is the log:
```
2025-12-23 11:21:59.568647500 DEBUG Initialization options: InitializationOptions {
    log_level: Some(
        Debug,
    ),
    log_file: Some(
        "~/ty_server.log",
    ),
    options: ClientOptions {
        global: GlobalOptions {
            diagnostic_mode: None,
            experimental: None,
        },
        workspace: WorkspaceOptions {
            disable_language_services: None,
            inlay_hints: None,
            completions: None,
            python_extension: None,
        },
        unknown: {},
    },
}
2025-12-23 11:21:59.569368000 DEBUG Resolved client capabilities: ["WORKSPACE_DIAGNOSTIC_REFRESH", "INLAY_HINT_REFRESH", "PULL_DIAGNOSTICS", "TYPE_DEFINITION_LINK_SUPPORT", "DEFINITION_LINK_SUPPORT", "DECLARATION_LINK_SUPPORT", "PREFER_MARKDOWN_IN_HOVER", "MULTILINE_SEMANTIC_TOKENS", "SIGNATURE_LABEL_OFFSET_SUPPORT", "SIGNATURE_ACTIVE_PARAMETER_SUPPORT", "HIERARCHICAL_DOCUMENT_SYMBOL_SUPPORT", "WORK_DONE_PROGRESS", "FILE_WATCHER_SUPPORT", "RELATIVE_FILE_WATCHER_SUPPORT", "DIAGNOSTIC_DYNAMIC_REGISTRATION", "WORKSPACE_CONFIGURATION", "DIAGNOSTIC_RELATED_INFORMATION"]
2025-12-23 11:21:59.569483400  INFO Version: 0.0.3 (fadfe0966 2025-12-17)
2025-12-23 11:21:59.603224500  WARN No workspace(s) were provided during initialization. Using the current working directory from the fallback system as a default workspace: D:\Aleksander2\Programming\Python\Projects\PriceTracker
2025-12-23 11:21:59.603447700 DEBUG Requesting workspace configuration for workspaces
2025-12-23 11:21:59.603735400 DEBUG Deferring `textDocument/didOpen` notification until all workspaces are initialized
2025-12-23 11:21:59.605983500 DEBUG Deferring `textDocument/semanticTokens/range` request until all workspaces are initialized
2025-12-23 11:21:59.606362100 DEBUG client_response{id=0 method="workspace/configuration"}: Received workspace configurations, initializing workspaces
2025-12-23 11:21:59.606392100 DEBUG client_response{id=0 method="workspace/configuration"}: No workspace options provided for file:///D:/Aleksander2/Programming/Python/Projects/PriceTracker, using default options
2025-12-23 11:21:59.606429600 DEBUG Initializing workspace `file:///D:/Aleksander2/Programming/Python/Projects/PriceTracker`
2025-12-23 11:21:59.606457800 DEBUG Searching for a project in 'D:\Aleksander2\Programming\Python\Projects\PriceTracker'
2025-12-23 11:21:59.606754000 DEBUG The ancestor directories contain no `pyproject.toml`. Falling back to a virtual project.
2025-12-23 11:21:59.606773900 DEBUG Searching for a user-level configuration at `C:\Users\waffle\AppData\Roaming\ty\ty.toml`
2025-12-23 11:21:59.607867200  INFO Defaulting to python-platform `win32`
2025-12-23 11:21:59.607886600 DEBUG Discovering virtual environment in `D:\Aleksander2\Programming\Python\Projects\PriceTracker`
2025-12-23 11:21:59.608112300 DEBUG Attempting to parse virtual environment metadata at 'C:\Users\waffle\AppData\Local\Programs\Python\Python313\pyvenv.cfg'
2025-12-23 11:21:59.608154900 DEBUG Failed to discover ty's environment: Invalid ty environment `C:\Users\waffle\AppData\Local\Programs\Python\Python313`: points to a broken venv with no pyvenv.cfg file
2025-12-23 11:21:59.608321300 DEBUG No virtual environment found
2025-12-23 11:21:59.608380000 DEBUG Including `.` in `environment.root`
2025-12-23 11:21:59.608403100 DEBUG Adding first-party search path `D:\Aleksander2\Programming\Python\Projects\PriceTracker`
2025-12-23 11:21:59.608436900 DEBUG Using vendored stdlib
2025-12-23 11:21:59.608766100  INFO Python version: Python 3.14, platform: win32
2025-12-23 11:21:59.609126800 DEBUG Adding new file root 'D:\Aleksander2\Programming\Python\Projects\PriceTracker' of kind Project
2025-12-23 11:21:59.609183000 DEBUG Registering diagnostic capability with OpenFilesOnly diagnostic mode
2025-12-23 11:21:59.609227300 DEBUG Resolving dynamic module resolution paths
2025-12-23 11:21:59.609262800 DEBUG Processing deferred notification `textDocument/didOpen`
2025-12-23 11:21:59.611432800 DEBUG Skipping directory 'D:\Aleksander2\Programming\Python\Projects\PriceTracker\.ruff_cache' because it is excluded by a default or `src.exclude` pattern
2025-12-23 11:21:59.618281200 DEBUG notification{method="textDocument/didOpen"}:apply_changes: Adding file `D:\Aleksander2\Programming\Python\Projects\PriceTracker\prisjakt.py` to project `PriceTracker`
2025-12-23 11:21:59.618312900 DEBUG notification{method="textDocument/didOpen"}: Opening file `D:\Aleksander2\Programming\Python\Projects\PriceTracker\prisjakt.py`
2025-12-23 11:21:59.618320300 DEBUG notification{method="textDocument/didOpen"}: Take open project files
2025-12-23 11:21:59.618341500 DEBUG notification{method="textDocument/didOpen"}:set_open_files{open_files={file(Id(1000))}}: Set open project files (count: 1)
2025-12-23 11:21:59.618351300 DEBUG Processing deferred request `textDocument/semanticTokens/range`
2025-12-23 11:21:59.618421400 DEBUG client_response{id=1 method="client/registerCapability"}: Registered dynamic capabilities
2025-12-23 11:21:59.619527700 DEBUG request{id=2 method="textDocument/semanticTokens/range"}: Module `requests` not found in search paths
2025-12-23 11:21:59.619838000 DEBUG request{id=2 method="textDocument/semanticTokens/range"}: Module `requests` not found while looking in parent dirs (neither stub nor real module file)
2025-12-23 11:21:59.620444400 DEBUG Skipping directory 'D:\Aleksander2\Programming\Python\Projects\PriceTracker\.ruff_cache' because it is excluded by a default or `src.exclude` pattern
2025-12-23 11:21:59.628932300  INFO request{id=3 method="textDocument/diagnostic"}:check_file{file=File(System("D:\\Aleksander2\\Programming\\Python\\Projects\\PriceTracker\\prisjakt.py"))}:Project::index_files{project=PriceTracker}: Indexed 1 file(s) in 0.010s
2025-12-23 11:21:59.629076600 DEBUG request{id=3 method="textDocument/diagnostic"}:check_file{file=File(System("D:\\Aleksander2\\Programming\\Python\\Projects\\PriceTracker\\prisjakt.py"))}: Checking file 'D:\Aleksander2\Programming\Python\Projects\PriceTracker\prisjakt.py'
2025-12-23 11:21:59.666197200 DEBUG request{id=2 method="textDocument/semanticTokens/range"}: Module `requests` not found while looking in parent dirs (neither stub nor real module file)
2025-12-23 11:21:59.666423800 DEBUG request{id=2 method="textDocument/semanticTokens/range"}: Module `requests` not found while looking in parent dirs (neither stub nor real module file)
2025-12-23 11:22:02.389699000 DEBUG request{id=4 method="textDocument/semanticTokens/range"}: Module `requests` not found while looking in parent dirs (neither stub nor real module file)
2025-12-23 11:22:02.389900900 DEBUG request{id=4 method="textDocument/semanticTokens/range"}: Module `requests` not found while looking in parent dirs (neither stub nor real module file)
```

---

_Comment by @carljm on 2025-12-23 20:40_

Thanks for the extra details! In both CLI and LSP it looks indeed like we are not discovering any third-party search paths at all. We look at `C:\Users\waffle\AppData\Local\Programs\Python\Python313` but reject it because it doesn't have a `pyvenv.cfg`. 

Looking at the code, it seems I was wrong that we support system Pythons if ty is installed into them. We return `true` from `SysPrefixPathOrigin::must_be_virtual_env` for `Self::SelfEnvironment`, and there's a comment there explaining that we didn't include support for this initially.

I think we definitely should support it, though -- using this issue to track that.

Thanks again @leet0rz !

---

_Renamed from "Error on importing installed libs like requests/pyside6 etc." to "Support finding dependencies in system Pythons that ty is installed into" by @carljm on 2025-12-23 20:41_

---

_Label `question` removed by @carljm on 2025-12-23 20:41_

---

_Label `needs-mre` removed by @carljm on 2025-12-23 20:41_

---

_Added to milestone `Stable` by @carljm on 2025-12-23 20:41_

---

_Comment by @lludriga on 2025-12-26 19:43_

Looking forward to this for neovim integration in quarto documents, where I use the system python.

For now, I'm using the following configuration for the lsp in neovim:
```lua
-- You need to:
-- uv init --script script.py
-- uv add --script script.py 'dependency'
-- uv run script.py
-- For the environment to be created for the script
local function uv_script_interpreter(script_path)
    local result = vim.system(
        { 'uv', 'python', 'find', '--script', script_path },
        { text = true }
    ):wait()
    if result.code == 0 then
        return vim.fn.trim(result.stdout)
    end
end

local function uv_interpreter(script_path)
    local result = vim.system(
        { 'uv', 'python', 'find' },
        { text = true }
    ):wait()
    if result.code == 0 then
        return vim.fn.trim(result.stdout)
    end
end

return {
    cmd = { "ty", "server" },
    filetypes = { "python" },
    root_markers = { "ty.toml", "pyproject.toml", "setup.py", "setup.cfg", "requirements.txt", ".git" },
    settings = { -- settings needs to be initialized for before_init to change it
        ty = {
            configuration = {
                environment = {
                    python = nil,
                },
            },
        },
    },
    before_init = function(_, config)
        local script = vim.api.nvim_buf_get_name(0)
        local python = uv_script_interpreter(script)
        if not python then
            python = uv_interpreter(script)
        end
        config.settings.ty.configuration.environment.python = python
    end,
}
```
It detects the system python if not in a venv, or the scripts venv if it was already created, using [#22053](https://github.com/astral-sh/ruff/pull/22053).

It works for me but haven't tested it much, use at your own risk :)

---

_Comment by @justinjhendrick on 2026-01-05 23:56_

If I'm understanding this issue correctly, then the quote from the docs (below) is inaccurate.

> Passing a path to a Python executable is supported, but passing a path to a dynamic executable (such as a shim) is not currently supported.
> This option can be used to point to virtual or system Python environments.

from https://docs.astral.sh/ty/reference/configuration/#python

Should those docs be updated to point here? Until this feature is implemented, or support for non-virtual environments is dropped?

---

_Comment by @carljm on 2026-01-05 23:57_

I think that portion of the docs is accurate. Explicitly pointing to a system Python installation using the `python` configuration option (in config or via CLI) is supported. What isn't supported currently is automatically using a system Python installation that ty itself is installed into.

---

_Comment by @justinjhendrick on 2026-01-06 00:07_

Ah, I was incorrect. I see the distinction now, thanks for explaining.

---

_Comment by @amotl on 2026-01-07 22:03_

Hi. I don't know if our observation fits here. Please hide/collapse this message when not. We'd like to invoke `ty` in a traditional CI/GHA environment where we are not using `uv` yet. 

- GH-2384

---

_Comment by @carljm on 2026-01-07 22:35_

Unfortunately I think if you don't know where your Python environment is, then really the only answers are either to put it in a place where you know where it is (i.e. use uv and/or a venv), or fix this issue in ty. We'll try to get to it as soon as we can; pull requests welcome! I don't think it should be too hard, the current code just explicitly excludes system environments. It might be as simple as removing that exclusion? @zanieb might know if there are other likely dragons.

---
