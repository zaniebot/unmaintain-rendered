---
number: 11366
title: Native server indexes the entire project when no local config found
type: issue
state: closed
author: akthe-at
labels:
  - server
assignees: []
created_at: 2024-05-11T02:48:16Z
updated_at: 2024-10-18T05:21:44Z
url: https://github.com/astral-sh/ruff/issues/11366
synced_at: 2026-01-10T01:22:51Z
---

# Native server indexes the entire project when no local config found

---

_Issue opened by @akthe-at on 2024-05-11 02:48_

I initially posted this in #11258 as an issue I noticed but I figured it was going to be fixed with the associated PR(#11266).  However with ruff 0.4.4 I noticed that the problem described below still persists. I am going to attach the LSP error from neovim, it appears to have a lot of repeating messages so I apologize.

```
[ERROR][2024-05-10 21:40:46] .../vim/lsp/rpc.lua:770	"rpc"	"C:\\Users\\ARK010\\AppData\\Local\\nvim-data\\mason\\bin\\ruff.CMD"	"stderr"	"┐ruff_server::server::api::notifications::did_open::run{file=file:///C:/Users/ARK010/test.py}\n┘\n  39.925874s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n"
[ERROR][2024-05-10 21:40:46] .../vim/lsp/rpc.lua:770	"rpc"	"C:\\Users\\ARK010\\AppData\\Local\\nvim-data\\mason\\bin\\ruff.CMD"	"stderr"	"┐ruff_server::server::api::notifications::did_change::run{file=file:///C:/Users/ARK010/test.py}\n┘\n  39.926023s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n┐ruff_server::server::api::notifications::did_change::run{file=file:///C:/Users/ARK010/test.py}\n┘\n  39.926098s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n┐ruff_server::server::api::notifications::did_change::run{file=file:///C:/Users/ARK010/test.py}\n┘\n  39.926179s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n┐ruff_server::server::api::notifications::did_change::run{file=file:///C:/Users/ARK010/test.py}\n┘\n  39.926229s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n┐ruff_server::server::api::notifications::did_change::run{file=file:///C:/Users/ARK010/test.py}\n┘\n┐ruff_server::server::api::notifications::did_change::run{file=file:///C:/Users/ARK010/test.py}\n┘\n  39.926298s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n  39.926323s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n  39.926377s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n┐ruff_server::server::api::notifications::cancel::run{}\n┘\n  39.926542s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n┐ruff_server::server::api::notifications::cancel::run{}\n┘\n┐ruff_formatter::printer::Printer::print{}\n┘\n"
[ERROR][2024-05-10 21:40:46] .../vim/lsp/rpc.lua:770	"rpc"	"C:\\Users\\ARK010\\AppData\\Local\\nvim-data\\mason\\bin\\ruff.CMD"	"stderr"	"  39.927576s INFO ruff_server::server Configuration file watcher successfully registered\n┐ruff_formatter::printer::Printer::print{}\n┘\n"
```

I am also going to copy and paste the lsp log from ~2 minutes later when I tried again in the same file (without closing it, just waiting ~ 2 minutes, and it successfully formatted.

```
[ERROR][2024-05-10 21:43:23] .../vim/lsp/rpc.lua:770	"rpc"	"C:\\Users\\ARK010\\AppData\\Local\\nvim-data\\mason\\bin\\ruff.CMD"	"stderr"	" 196.088724s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n┐ruff_formatter::printer::Printer::print{}\n┘\n"
[ERROR][2024-05-10 21:43:23] .../vim/lsp/rpc.lua:770	"rpc"	"C:\\Users\\ARK010\\AppData\\Local\\nvim-data\\mason\\bin\\ruff.CMD"	"stderr"	"┐ruff_server::server::api::notifications::did_change::run{file=file:///C:/Users/ARK010/test.py}\n┘\n"
[ERROR][2024-05-10 21:43:23] .../vim/lsp/rpc.lua:770	"rpc"	"C:\\Users\\ARK010\\AppData\\Local\\nvim-data\\mason\\bin\\ruff.CMD"	"stderr"	"┐ruff_server::server::api::notifications::did_change::run{file=file:///C:/Users/ARK010/test.py}\n┘\n"
[ERROR][2024-05-10 21:43:23] .../vim/lsp/rpc.lua:770	"rpc"	"C:\\Users\\ARK010\\AppData\\Local\\nvim-data\\mason\\bin\\ruff.CMD"	"stderr"	" 196.157007s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n"
[ERROR][2024-05-10 21:43:23] .../vim/lsp/rpc.lua:770	"rpc"	"C:\\Users\\ARK010\\AppData\\Local\\nvim-data\\mason\\bin\\ruff.CMD"	"stderr"	" 196.160130s INFO ruff_server::session::workspace::ruff_settings No Ruff settings file found for C:\\Users\\ARK010\\test.py; falling back to default configuration\n"
```


> @snowsignal Does this PR solve this scenario:
> Windows 10
> Neovim Nightly Latest
> Ruff 0.4.3
> Python 3.12.3
> 
> - Have global config in ~/.config/ruff/pyproject.toml
> - Create "test.py" in  ~/ (not a project/workspace and there is no present .toml config)
> - Create a test function
> ```python
> def this_function(y: int, x: int) -> int:
> 
> 
>     """ This is an example function """
> 
> 
>     word_fx: int = 0
>     return y + x + word_fx
> ```
> 
> - Notice the excessive space between function definition and doc strings...
> - Attempt to save (my neovim config would use conform.nvim to call ruff to format on save)
> - 2-4 second hang...ruff errors and times out (   Warn  8:53:50 PM notify.warn [LSP][ruff] timeout)
> - Curve ball...wait a minute or two to type this example/report up...go to save and it formats successfully this time.
> - This does not occur in projects with a local .toml file...
> - I am assuming this is because it takes time to look for the global config and it doesn't merge with the editor config without the local config?
> 
> _Originally posted by @akthe-at in https://github.com/astral-sh/ruff/issues/11258#issuecomment-2093954838_
            

---

_Label `server` added by @AlexWaygood on 2024-05-11 09:24_

---

_Assigned to @snowsignal by @snowsignal on 2024-05-13 01:53_

---

_Comment by @akthe-at on 2024-05-29 03:12_

@snowsignal I have more information, I think this revelation might make you want to close this issue?

I had a hunch and moved the simple .py featured above from the location of `"~"` or `C:/Users/FooBar/ `to a much more nested directory like `~/config/test/` and this problem no longer persisted.

It appears the issue is that formatting with ruff times out when the file is in the ~/ directory because it is busy parsing all of the sub-directories?

---

_Comment by @danielhollas on 2024-06-03 13:01_

I've ran into this as well, it's very confusing. The ruff Lsp would be delayed on startup if a modified python file is in a top level folder without a config file (i.e. outside of project directory). I noticed in Lsp logs that ruff is walking all subdirectories looking for a config file.

I don't understand why ruff should be looking for a config file in subdirectories? Shouldn't it be the other way around i.e it should walk the parent directories to look for the config. This seems like a bug to me.

Cc @dhruvmanila 

---

_Comment by @MichaReiser on 2024-06-03 13:12_

I think it has to scan sub folders in case you have a configuration e.g. only in the `src` directory. We then want to apply that configuration to `src` only, but fall back to the default configuration for everything else. 

I wonder if we ignore directories like `.git` etc when walking. If not, then that could add a significant overhead.

---

_Comment by @charliermarsh on 2024-06-03 13:19_

@MichaReiser - We ignore them now. We didn't in the initial beta release.

---

_Comment by @danielhollas on 2024-06-03 14:21_

> I think it has to scan sub folders in case you have a configuration e.g. only in the `src` directory. We then want to apply that configuration to `src` only, but fall back to the default configuration for everything else. 
> 
> I wonder if we ignore directories like `.git` etc when walking. If not, then that could add a significant overhead.

I am confused. Surely if I am editing a file from outside the src/ directory, then even if you find a config there it shouldn't apply to the file I am editing, right?

Right now if I am editing a python file in me $HOME, it seems to walk the entirety of my HOME which seems suboptimal:-)

---

_Added to milestone `Ruff Server: Stable` by @snowsignal on 2024-06-17 16:28_

---

_Comment by @dhruvmanila on 2024-07-02 06:18_

I'm looking into this. The `ruff check t.py` command where `t.py` file is in the home directory is instantaneous. So, I'm guessing we're doing extra work in the server compared to what's being done on the command-line.

Looking at the following logs, it does seem like we're traversing the entire home directory to build up the index which I think was intentional in https://github.com/astral-sh/ruff/pull/10950.
```
[START][2024-07-02 11:17:44] LSP logging initiated
[ERROR][2024-07-02 11:17:44] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/dhruv/.local/bin/ruff"	"stderr"	"warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `projects/algorithms_keeper/pyproject.toml`:\n  - 'ignore' -> 'lint.ignore'\n  - 'select' -> 'lint.select'\n  - 'mccabe' -> 'lint.mccabe'\n  - 'pylint' -> 'lint.pylint'\n  - 'per-file-ignores' -> 'lint.per-file-ignores'\n"
[ERROR][2024-07-02 11:18:10] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/dhruv/.local/bin/ruff"	"stderr"	"warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `Library/Caches/uv/archive-v0/iWen-UJ1Onc60Mw9JA5ey/pandas/pyproject.toml`:\n  - 'ignore' -> 'lint.ignore'\n  - 'select' -> 'lint.select'\n  - 'typing-modules' -> 'lint.typing-modules'\n  - 'unfixable' -> 'lint.unfixable'\n  - 'per-file-ignores' -> 'lint.per-file-ignores'\n"
[ERROR][2024-07-02 11:18:11] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/dhruv/.local/bin/ruff"	"stderr"	"warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `.cargo/registry/src/index.crates.io-6f17d22bba15001f/pep440_rs-0.3.12/pyproject.toml`:\n  - 'per-file-ignores' -> 'lint.per-file-ignores'\n"
[ERROR][2024-07-02 11:18:17] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/dhruv/.local/bin/ruff"	"stderr"	"warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `work/astral/parser-checkouts/openai-python/pyproject.toml`:\n  - 'ignore' -> 'lint.ignore'\n  - 'ignore-init-module-imports' -> 'lint.ignore-init-module-imports'\n  - 'select' -> 'lint.select'\n  - 'unfixable' -> 'lint.unfixable'\n  - 'per-file-ignores' -> 'lint.per-file-ignores'\n"
[ERROR][2024-07-02 11:18:17] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/dhruv/.local/bin/ruff"	"stderr"	"warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `work/astral/parser-checkouts/matplotlib/pyproject.toml`:\n  - 'external' -> 'lint.external'\n  - 'ignore' -> 'lint.ignore'\n  - 'select' -> 'lint.select'\n  - 'pydocstyle' -> 'lint.pydocstyle'\n  - 'per-file-ignores' -> 'lint.per-file-ignores'\n"
...
```

---

_Comment by @dhruvmanila on 2024-07-02 06:28_

> We ignore them now. We didn't in the initial beta release.

I don't think we're ignoring them. I just logged the directories that we're visiting and it is visiting the `.git` directory:
```
  [..]
  47.573640042s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/pyright/.git"
  47.966425417s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/vscode/.git"
  48.093653667s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/black/.git"
  48.093867208s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/black/.git/objects"
  48.093895917s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/black/.git/objects/pack"
  48.093932625s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/black/.git/objects/info"
  [..]
```

---

_Comment by @dhruvmanila on 2024-07-02 06:57_

Ok, we are ignoring the `.git` directory but not all of them:
```
   0.155401750s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git"
   0.155417292s DEBUG main ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/work/astral/parser-checkouts/home-assistant-core/.git
```

And via a Python file in home directory:
```
  51.116335792s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/oxc/.git"
  51.116371750s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/oxc/.git/objects"
  51.116647917s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/oxc/.git/objects/pack"

  [..]

  51.168634667s  INFO main ruff_server::session::index::ruff_settings: Visiting "/Users/dhruv/git/kitty/.git"
  51.168642167s DEBUG main ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/git/kitty/.git
```

I think what is happening is that the exclusion is using the existing index which is currently being built:
https://github.com/astral-sh/ruff/blob/25080acb7ad899f1e0ed933582662d9699ef5424/crates/ruff_server/src/session/index/ruff_settings.rs#L145-L147

But, it doesn't contain the resolved settings until it encounters a configuration file and only then it'll start excluding any relevant directories for the matching paths. I think this is an expected behavior.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-07-10 03:01_

---

_Unassigned @snowsignal by @dhruvmanila on 2024-07-10 03:01_

---

_Referenced in [astral-sh/ruff#12299](../../astral-sh/ruff/pulls/12299.md) on 2024-07-12 12:11_

---

_Comment by @dhruvmanila on 2024-07-15 09:54_

There are two more options that could be implemented here:
1. Not indexing the project if there's no root configuration file present
2. Or, use a fallback settings (default) if there's no root configuration file present

---

_Referenced in [astral-sh/ruff#12362](../../astral-sh/ruff/pulls/12362.md) on 2024-07-17 12:51_

---

_Renamed from "ruff server has delay when no local config." to "Native server indexes the entire project when no local config found" by @dhruvmanila on 2024-07-22 10:05_

---

_Removed from milestone `Ruff Server: Stable` by @dhruvmanila on 2024-07-22 10:05_

---

_Added to milestone `Ruff Server: Post-Stable` by @dhruvmanila on 2024-07-22 10:05_

---

_Referenced in [astral-sh/ruff-vscode#549](../../astral-sh/ruff-vscode/issues/549.md) on 2024-08-07 02:38_

---

_Comment by @akthe-at on 2024-08-08 20:02_

Just wanted to say great job on improving this over the last few releases. This only takes ~10 seconds to successfully format compared to the ~2 minutes before...Same file, same directory, same repeated format "issues" and a vastly improved user experience now.

---

_Referenced in [astral-sh/ruff-vscode#606](../../astral-sh/ruff-vscode/issues/606.md) on 2024-09-07 23:44_

---

_Referenced in [astral-sh/ruff-vscode#627](../../astral-sh/ruff-vscode/issues/627.md) on 2024-10-09 06:40_

---

_Comment by @dhruvmanila on 2024-10-10 15:22_

There seems to be one more case when this occurs - when a user opens a random file on their system directly in VS Code. In this case, the client doesn't send the workspace path and the current working directory for the editor is `/`. This means the server will try to index the entire system which is not ideal.

One solution is mentioned in https://github.com/astral-sh/ruff-vscode/issues/627#issuecomment-2405420427:

> We would avoid traversing into the current working directory if the server is running in single-file mode. A single-file mode is basically when there's no workspace provided by the client. So, the indexer would still climb up as well as look into the current working directory for any config but it won't traverse it.

---

_Referenced in [astral-sh/ruff#13770](../../astral-sh/ruff/pulls/13770.md) on 2024-10-16 09:11_

---

_Closed by @dhruvmanila on 2024-10-18 05:21_

---

_Closed by @dhruvmanila on 2024-10-18 05:21_

---

_Referenced in [astral-sh/ruff-vscode#637](../../astral-sh/ruff-vscode/issues/637.md) on 2024-10-29 10:03_

---
