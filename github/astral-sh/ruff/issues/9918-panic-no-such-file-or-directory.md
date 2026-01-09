---
number: 9918
title: "[Panic] No such file or directory"
type: issue
state: closed
author: CoderJoshDK
labels:
  - bug
assignees: []
created_at: 2024-02-09T23:08:14Z
updated_at: 2025-02-25T14:17:43Z
url: https://github.com/astral-sh/ruff/issues/9918
synced_at: 2026-01-07T13:12:15-06:00
---

# [Panic] No such file or directory

---

_Issue opened by @CoderJoshDK on 2024-02-09 23:08_

Rare bug, hard to reproduce.

Error's log:
```log
[ERROR][2024-02-09 14:12:56] ...lsp/handlers.lua:535"Ruff: Lint failed (
error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at /Users/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/path-dedot-3.1.1/src/lib.rs:330:70:
called `Result::unwrap()` on an `Err` value: Os { code: 2, kind: NotFound, message: "No such file or directory" }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
)
```

## Context

I wrote a NeoVim plugin that creates files for temp use. In the process of that, a new named buffer is created. That buffer is pointing to a file that does not exist. It will not be writen, until the user saves the buffer. When nvim opens that buffer, the error message comes up. The issue with this bug, is that it is rare. It has happened to me twice in the span of 3 days. I have now patched my plugin, so it creates the buffers in a differnt way. But [this commit version](https://github.com/CoderJoshDK/playground.nvim/tree/502e1fbdafe28b6ee986aa5e4fd355aa3925cf54) is the latest one that can cuase the error.

The basics of the code that cuases this issue, is:

```lua
local buf_n = vim.api.nvim_create_buf(false, false)
vim.api.nvim_buf_set_name(buf_n, "/absolute/path/to/nothing.py") -- absolute path to a new python file that doesn't exist yet.
vim.api.nvim_set_current_buf(buf_n)
vim.cmd.filetype("detect") -- this is what triggers ruff to start and then causes the error. But this is needed and most of the time works
```

Now, the weird thing, is that you would think this error would happen either all the time, or never. But it happens randomly. The issue would seem to be related to some process that tries to read the file, but there is only a buffer that just so happens to have a name.

The error would keep coming up (crashing the server and then I guess restarting it again and crashing) until I closed the whole instance of nvim and opened it up again. (`:qa`)

When I ran into this error, I would run this code in a lua `cwd`. Meaning, there is no local `pyproject.toml`. There is a global config I have, as seen bellow. 

## Computer/Software Info

MacOs : 14.2.1
NVim : 0.9.5
python3 --version : Python 3.11.7 (brew, so there are multiple versions of python on machine, as you will see later)

### Mason info dump
Mason is the lsp manager I use for nvim.

<details>
  <summary>Ruff</summary>

`mason-receipt.json`  
```json
{"name":"ruff","secondary_sources":[],"schema_version":"1.1","links":{"share":{},"bin":{"ruff":"venv\/bin\/ruff"},"opt":{}},"primary_source":{"type":"registry+v1","id":"pkg:pypi\/ruff@0.2.1"},"metrics":{"start_time":1707262291048,"completion_time":1707262295039}}
```
`venv/pyvenv.cfg`
```cfg
home = /opt/homebrew/opt/python@3.12/bin
include-system-site-packages = false
version = 3.12.1
executable = /opt/homebrew/Cellar/python@3.12/3.12.1_1/Frameworks/Python.framework/Versions/3.12/bin/python3.12
command = /opt/homebrew/opt/python@3.12/bin/python3.12 -m venv /Users/<user>/.local/share/nvim/mason/packages/ruff/venv
```
</details>

<details>
  <summary>ruff_lsp</summary>

`mason-receipt.json`  
```json
{"name":"ruff-lsp","secondary_sources":[],"schema_version":"1.1","links":{"share":{},"bin":{"ruff-lsp":"venv\/bin\/ruff-lsp"},"opt":{}},"primary_source":{"type":"registry+v1","id":"pkg:pypi\/ruff-lsp@0.0.52"},"metrics":{"start_time":1707262291169,"completion_time":1707262295374}}
```
`venv/pyvenv.cfg`
```cfg
home = /opt/homebrew/opt/python@3.12/bin
include-system-site-packages = false
version = 3.12.1
executable = /opt/homebrew/Cellar/python@3.12/3.12.1_1/Frameworks/Python.framework/Versions/3.12/bin/python3.12
command = /opt/homebrew/opt/python@3.12/bin/python3.12 -m venv /Users/<user>/.local/share/nvim/mason/packages/ruff-lsp/venv
```
</details>

### Global configs

<details>
  <summary>`${config_dir}/ruff/pyproject.toml` (mac)</summary>

```toml
line-length = 90
fix = true
target-version = "py311"

unfixable = ["F841"]
ignore = ["PLR0913", "ISC001"]

[lint]
select = ["E", "F", "B", "W", "Q", "A",
    "UP", "ASYNC", "FA", "ISC", "RET", "SIM", "ARG", "PL", "RUF"]
# Maybe don't allow some SIM options
extend-select = ["I001"]
```
</details>

<details>
  <summary>Unlikely to matter; extra info</summary>

I also have ruff global pip installed. That version is `0.1.15`. I do have a different plugin that uses this global one. (actually this doesn't feel right. I feel like I remember updating it to `0.2.x` but my pip freeze says otherwise.) But this shouldn't be the issue, becasuse this is only used for formating. And I know this isn't the issue, because it **only** runs on formatting, and the error would come from just opening the buffer. The server would restart and then keep crashing every time I sent an update to the server (like moving my cursor or typing). 
</details>

Please let me know if there is anything else you need or want to know. This would seem to be a low priority issue, since it is inconsitant and also me being a strange use case. But good to keep in mind if something larger pops up that is related. 
I will let you know if the error comes back. And I will try to keep better notes next time. 

---

_Label `bug` added by @charliermarsh on 2024-02-11 00:09_

---

_Comment by @charliermarsh on 2024-02-11 00:09_

\cc @dhruvmanila who commented on this in Discord.

---

_Comment by @dhruvmanila on 2024-02-12 10:39_

Hey, thanks for providing all the details regarding the issue! Unfortunately, I'm unable to reproduce this locally.

My first instinct is that not having a file on disk shouldn't result in any error because the way `ruff-lsp` and `ruff` interacts is via stdin. The buffer content is passed in via stdin and the path is set via the [`--stdin-filename` CLI argument](https://github.com/astral-sh/ruff-lsp/blob/4afe87915f89574340f4a7c36d398478cc5574a5/ruff_lsp/server.py#L1884-L1885). This means that both the server and Ruff doesn't interact with the file on disk.

The second thing is that the config content you've provided is for a `ruff.toml`/`.ruff.toml` file while the path uses the `pyproject.toml` config. The content is invalid for the `pyproject.toml` file where the top level settings would go under `[tool.ruff]` and linter settings would go under `[tool.ruff.lint]`.

Now, the error message suggests that the failure point is in the [`unwrap` call](https://github.com/magiclen/path-dedot/blob/36c197f922d4c8779594ce25b8ffc6cb0315c6a8/src/lib.rs#L329-L330) to get the current working directory here in Ruff:

https://github.com/astral-sh/ruff/blob/6dc1b219176a1a94e4f1436a431c02222926b44c/crates/ruff_linter/src/fs.rs#L67-L68

Now, the containing function is invoked throughout the codebase to convert the absolute path to the relative one as per the current working directory. This is used to display the path, store the diagnostics as per the path, etc.

It would be really useful to get the backtrace using `RUST_BACKTRACE=1` to understand where the error is originating from but I assume that's going to be challenging given the frequency of the issue.

I also played around with the linked plugin through `:Playground py random` command but it works as expected i.e., creates a temporary buffer and as I update the buffer content, the diagnostics change, tried code actions and formatting as well without saving the file.

I feel like I need to give it another look with a fresh mind at a later point but this is currently what I've played around with so far.

---

That said,

1. Are you able to reproduce this in any way using the mentioned code above? The one which creates an unnamed buffer.
2. If you can provide a minimal Python code in which case it panics, it could be useful in figuring out where in Ruff this problem is originating from.
3. Using the mentioned plugin, can you provide the steps you took to create the playground and how you used it which lead to the panic? It might be that I'm not using the plugin in a way that you were using it.

---

_Comment by @CoderJoshDK on 2024-02-12 12:38_

I was originally hesitant to even share because I knew this would be a difficult bug to reproduce. But I will try to help.

> The second thing is that the config content you've provided is for a ruff.toml/.ruff.toml file while the path uses the pyproject.toml config. 

Yes, sorry, you are right. It is a `ruff.toml` file. The location is otherwise correct. 

> It would be really useful to get the backtrace using `RUST_BACKTRACE=1`

I agree. I only stumbled into the error. I have set it globally now. But yea, because it was so infrequent, I couldn't catch it while I had this on.

> [ ... ] as I update the buffer content, the diagnostics change, tried code actions and formatting as well without saving the file.

When the error would show up, it would appear upon opening (or maybe switching) to the buffer. So as soon as it is available to type, you would either see the error or not. And then continuing past the panic, every lsp notification event would trigger the panic again.
Meaning, that:

> If you can provide a minimal Python code in which case it panics

The minimum python code required, is no python code, and just the file. 

> [ ... ] can you provide the steps you took to create the playground and how you used it which lead to the panic?

The way you used the plugin, was the exact way I did for causing the error. `:Playground py random` would do it.

I spent about an >hour this morning trying to forcibly recreate the error. And ... no dice. My intuition is telling me that this is most likely not a ruff error. It might be due to discrepancies between how nvim handles file paths. It sometimes will automatically change things around from absolute and relative. But I could not replicate that, and this is pure speculation. It might not be due to that at all. The only other thing that might be messing things up, is that the named buffer, is put into a folder that is newly created. Potentially causing this discrepancy, where the file location does exist but for some reason it isn't being recognized into `CWD`. How that would even be possible, I have no idea. 

But regardless of the exact mechanism, the part of the code that panics seems to agree that this is not a ruff error. I will keep an eye out for the error, but will unlikely find it again. If you think this is as far as it goes, you can close the issue (or give it another try). Either way, good luck (and also thanks for all the work)

---

_Comment by @dhruvmanila on 2024-02-13 11:29_

It could be that this issue isn't related to Neovim.

What kind of setup do you have in Neovim? Can you share the list of plugins you have installed and if possible, can you try to do the same task by having a minimal set of plugins instead?

> It might be due to discrepancies between how nvim handles file paths. It sometimes will automatically change things around from absolute and relative.

Can you expand on this? Like, have you faced this problem before? Do you have any plugin which changes directory automatically for you (something like https://github.com/oberblastmeister/rooter.nvim)?

As per the [documentation of `current_dir`](https://doc.rust-lang.org/stable/std/env/fn.current_dir.html), the error could be due to either directory not existing or insufficient permission. Which OS are you on?

Regardless, we can keep this issue around to see if anyone else faces this problem. In case you're able to provide more information in the future, please do so.

---

_Comment by @CoderJoshDK on 2024-02-13 14:28_

> What kind of setup do you have in Neovim? 

[My own configs](https://github.com/CoderJoshDK/JoshieVim).

<details>
<summary>Plugin List</summary>

```
  Total: 36 plugins

  Loaded (36)
    ● catppuccin 20.94ms  start
    ● cmp-buffer 0.15ms  nvim-cmp
    ● cmp-cmdline 0.13ms  nvim-cmp
    ● cmp-nvim-lsp 0.14ms  nvim-cmp
    ● cmp-path 0.13ms  nvim-cmp
    ● cmp_luasnip 0.05ms  nvim-cmp
    ● Comment.nvim 1.94ms  start
    ● conform.nvim 1.23ms  BufWritePre
    ● fidget.nvim 7.96ms  nvim-lspconfig
    ● friendly-snippets 0.13ms  nvim-cmp
    ● gitsigns.nvim 1.47ms  start
    ● indent-blankline.nvim 4.01ms  start
    ● lazy.nvim 6.97ms  init.lua
    ● lualine.nvim 6.9ms  start
    ● LuaSnip 4.78ms  nvim-cmp
    ● mason-lspconfig.nvim 0.04ms  nvim-lspconfig
    ● mason.nvim 3.58ms  nvim-lspconfig
    ● neodev.nvim 0.04ms  nvim-lspconfig
    ● nvim-cmp 25.92ms  start
    ● nvim-lspconfig 29.9ms  start
    ● nvim-tree.lua 21.5ms  start
    ● nvim-treesitter 5.85ms  start
    ● nvim-treesitter-textobjects 5.28ms  nvim-treesitter
    ● nvim-web-devicons 0.35ms  nvim-tree.lua
    ● peek.nvim 1.11ms  VeryLazy
    ● playground.nvim 1.57ms  start
    ● plenary.nvim 0.45ms  telescope.nvim
    ● sniprun 1.46ms  start
    ● telescope-fzf-native.nvim 0.2ms  telescope.nvim
    ● telescope.nvim 6.32ms  start
    ● vim-fugitive 1.15ms  start
    ● vim-rhubarb 0.47ms  start
    ● vim-sleuth 0.63ms  start
    ● vim-tmux-navigator 0.58ms  start
    ● which-key.nvim 1.75ms  start
    ● zen-mode.nvim 0.3ms  start
```

</details>

> can you try to do the same task by having a minimal set of plugins instead?

Considering that I tried to manually cause the error and failed ... I am not sure that this will help

> As per the [documentation of `current_dir`](https://doc.rust-lang.org/stable/std/env/fn.current_dir.html), the error could be due to either directory not existing or insufficient permission. Which OS are you on?

MacOS : 14.2.1. And the error being reported does seem to be `Current directory does not exist.` (at least based on the wording of the error). However, it says that it is based on `CWD`. Nothing I have messes with the `CWD`. `nvim-tree` has the potential to change the CWD, but even at that, it doesn't seem to have an effect (tried testing where I change my directory).

As for what I am talking about with filename modifiers:
https://vi.stackexchange.com/questions/39786/expand-sometimes-gives-me-a-full-path

---

_Comment by @dhruvinsh on 2024-03-12 17:50_

I am facing similar issue recently, 

```
>  RUST_BACKTRACE=1 ruff format -v --stdin-filename /home/ds/Documents/automation/fire/__init__.py

[2024-03-12][13:49:49][ruff_cli::commands::run][DEBUG] Identified files to lint in: 143.737µs
error: Failed to lint format: No such file or directory (os error 2)
[2024-03-12][13:49:49][ruff_cli::commands::run][DEBUG] ImportMap {
    module_to_imports: {},
}
[2024-03-12][13:49:49][ruff_cli::commands::run][DEBUG] Checked 1 files in: 14.163982ms
format:1:1: E902 No such file or directory (os error 2)
Found 1 error.
```

**Update**: 
After posting I realized, on the virtualenv I was running old version of ruff,
```
ruff --version
ruff 0.0.261
```

After bumping to latest version that is v3.0.2 it get stuck
```
> ruff format -v --stdin-filename /home/ds/Documents/automation/fire/__init__.py

[2024-03-12][13:53:17][ruff::resolve][DEBUG] Using Ruff default settings
```

**Update-2**:
after ruff cleanup and re-running the ruff format seems like working fine. I am running in to some isort issue, need to investigate that with neovim conform.nvim

---

_Comment by @dhruvmanila on 2024-04-01 09:14_

@dhruvinsh If the issue is unrelated to what's being discussed here, I would suggest to open a new ticket for it.

---

_Comment by @mesllo-bc on 2025-02-24 18:30_

Is there a fix for this? I'm suddenly getting this same issue for ruff `v0.9.6` and ruff `0.9.7`. Clearing `.ruff_cache` did not solve the issue. Reinstalling from scratch also did not help.

---

_Comment by @ntBre on 2025-02-24 21:14_

@dhruvmanila would know better than me, but it sounds like this issue could possibly be related to #15392, which came up recently too.

---

_Comment by @dhruvmanila on 2025-02-25 10:26_

I'm not sure if @CoderJoshDK is still facing this issue especially with the native server as `ruff-lsp` is deprecated.

@mesllo-bc I'd suggest opening a new issue with the problem that you're facing along with a way for us to reproduce it.

---

_Closed by @CoderJoshDK on 2025-02-25 13:30_

---

_Comment by @CoderJoshDK on 2025-02-25 14:17_

Thank you for reminder;

I have not experienced this issue in a long time. Sorry for not closing it sooner.

---
