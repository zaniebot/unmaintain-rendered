```yaml
number: 1614
title: Niche issue - Neovim on WSL with Windows executables
type: issue
state: closed
author: the-citto
labels:
  - question
  - server
  - windows
assignees: []
created_at: 2025-11-23T21:47:34Z
updated_at: 2025-11-26T15:19:18Z
url: https://github.com/astral-sh/ty/issues/1614
synced_at: 2026-01-12T15:54:25Z
```

# Niche issue - Neovim on WSL with Windows executables

---

_@the-citto_

### Question

Quite niche issue - not many people use Neovim on WSL to develop on Windows

> The `ty` LSP in Neovim doesn't recognise the Windows virtual environment

from the LspLog,  `ty` parses `pyenv.cfg` to 'create project'
`pyenv.cfg` that refers to `C:\` while WSL understands `/mnt/o/` could be the core issue, but the log says that the error was encountered when trying to resolve the `home` value 

tried to point to `.venv/Scripts/python.exe` - this is what I do to make `mypy` work
tried with just `.venv` - this works for `pyrigth`

I use this setup with all, `pytright`, `mypy`, `ruff` (although auto conf reload doesn't work for it), `flake8`. `black` and `isort`
I also just started using `uv` - good results with it too, when working on Windows project I use `uv.exe`, when on Linux projects simple `uv` 
`uv.exe run` is a god-send, before I was `.venv/Scripts/python.exe` for everything: virtual env activation is not an option with this setup 

all of the above, LPSs including ty, linters, formatters, work with .exe

I really hope this will find a solution, still early stages but I'm really looking forward to be able to forget about js' pyright in favour of blazingly fast ty

### Version

_No response_

---

_Label `question` added by @the-citto on 2025-11-23 21:47_

---

_Label `windows` added by @AlexWaygood on 2025-11-23 21:48_

---

_Comment by @MichaReiser on 2025-11-24 07:37_

Thanks for opening the issue. Can you share your project layout, or some steps on how I can reproduce the issue? I don't think I understand exactly where and when you see incorrect paths from reading the issue summary. If you can, can you share ty's logs?

---

_Comment by @the-citto on 2025-11-24 19:36_

thank MichaReiser

I use Neovim on Linux, everything good there, and on WSL too
at work I also have Windows projects, so I found ways to use Neovim on WSL for those, interacting directly with the .exe files
(as I mentioned, my setup is unusual, not sure anybody does something like it)

to make pyright work, my toml
```toml
[tool.pyright]
venvPath = "."
venv = ".venv"
```
for mypy
```toml
[tool.mypy]
python_executable = ".venv/Scripts/python.exe"
```

the issue was that ty didn't find the third party libraries
just found that this works
```toml
[tool.ty.environment]
extra-paths = [".venv/Lib/site-packages"]
```

leaving it on pyproject is a bit weird, and all colleagues use VSCode 
I'll make some experiment with Lua for when Neovim is on WSL - really want to use the same config at work and at home 
thing is that the [cmd](https://github.com/neovim/nvim-lspconfig/blob/30a2b191bccf541ce1797946324c9329e90ec448/lsp/ty.lua#L11C3-L11C27) from Lspconfig is `ty server` and that doesn't accept  ` --extra-search-path` 


anyways, my previous LspLogs
```
INFO No workspace options found for file:///mnt/c/Users/path-to-project, using default options\n"
```
```
ERROR Failed to create project for `/mnt/c/Users/path-to-project`: Failed to parse the `pyvenv.cfg` file at `/mnt/c/Users/path-to-project/.venv/pyvenv.cfg` because the following error was encountered when trying to resolve the `home` value to a directory on disk: No such file or directory (os error 2). Falling back to default settings\n"
```



---

_Label `server` added by @AlexWaygood on 2025-11-24 19:37_

---

_Comment by @MichaReiser on 2025-11-25 07:42_

Can you try using [`python = ".venv"`](https://docs.astral.sh/ty/reference/configuration/#python) over using `extra-paths`?

---

_Comment by @the-citto on 2025-11-25 08:51_

tried that but it didn't have effect
also tried `python = ".venv/Scripts/python.exe"`, if I understood correctly it was a possible setting too

---

_Comment by @MichaReiser on 2025-11-26 07:40_

Sorry for the many questions but I don't have a WSL machine at hand. 

Would you mind running ty with `-vv` so that it prints debug logs and share them with us

>  also tried python = ".venv/Scripts/python.exe", if I understood correctly it was a possible setting too

The path should point to the site-packages directory. But I don't remember if pointing it to an executable works too. @AlexWaygood might have added that at some point.

---

_Comment by @AlexWaygood on 2025-11-26 08:11_

Pointing it to an executable should also work, yes

---

_Comment by @the-citto on 2025-11-26 09:23_

thank you for the support

i went in my `.local/share/nvim/mason/packages/ty` and run both `venv/bin/python -m ty server -vv` and `venv/bin/python -m ty -vv server` but `server` doesn't accept `-v` argument
that is the one run by Neovim, the Lspconfig defaults `cmd = { "ty", "server" }` 

I _think_ the crux of this niche issue is that `ty` internals have some reference depending on the platform, WSL is Linux, and this use-case calls for Windows folder structures and executables 


anyway, for now I had to give up and postpone for WSL-Windows: unfortunately changes to project's files weren't picked up...
I wrote this in my Neovim Lspconfig to disable `ty` _only_ for WSL-Windows projects (WSL mounts Windows' `C:\` on `/mnt/c`)
```lua
    vim.lsp.config("ty", { capabilities = capabilities })
    if vim.fn.getcwd():match( "/([^/]+)") == "mnt" then
        vim.lsp.config("ty", { cmd = {} })
    end
```
I shouldn't have niche problems running `ty` in WSL for Linux projects :)

additional note for those 5 people in the world with a similar use case WSL-Neovim-Windows-python
I also just started using `uv` with this setup, running `uv.exe` from the WSL terminal
yesterday I found that `python-dotenv` wasn't installed correctly, and simply run `uv add` from a PowerShell terminal in the same location

---

_Comment by @MichaReiser on 2025-11-26 09:28_

Oh right sorry, I forgot that this is specific to neovim. I think the ideal outcome would be if we can reproduce this outside neovim, by just using the CLI (e.g. running the `ty.exe check` from within WSL?)

Or you could set the [`logFile`](https://docs.astral.sh/ty/reference/editor-settings/#logfile) and [`logLevel`](https://docs.astral.sh/ty/reference/editor-settings/#loglevel) settings. Hopefully this produces some useful output

---

_Comment by @the-citto on 2025-11-26 11:13_

cheers
I do use `ty.exe check` in the WSL terminal, together with `mypy.exe .` etc., and it works well 
in fact, I could run the checks on PowerShell, only a `CTRL+Tab` away on Windows terminal (which is what I do now for `uv add`, etc.)

all my fuss is because I'm so much looking forward to replace `pyright` and perhaps `mypy` with `ty`, but it's surely premature even for all Linux
once we'll get there, I'll get back to this WSL-Neovim-Windows thing

---

_Closed by @the-citto on 2025-11-26 15:19_

---
