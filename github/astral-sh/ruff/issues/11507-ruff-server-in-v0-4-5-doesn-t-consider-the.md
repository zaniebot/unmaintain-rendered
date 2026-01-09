---
number: 11507
title: "`ruff server` in `v0.4.5` doesn't consider the configuration setting in Neovim"
type: issue
state: closed
author: a1401358759
labels:
  - bug
  - server
assignees: []
created_at: 2024-05-23T07:54:30Z
updated_at: 2024-05-27T15:36:36Z
url: https://github.com/astral-sh/ruff/issues/11507
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff server` in `v0.4.5` doesn't consider the configuration setting in Neovim

---

_Issue opened by @a1401358759 on 2024-05-23 07:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

When I updated to version 0.4.5, the E30* checks all failed and nothing changed in my configuration

my pyproject.toml
```toml
[tool.ruff]
preview = true
# Exclude a variety of commonly ignored directories.
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".git-rewrite",
    ".hg",
    ".ipynb_checkpoints",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pyenv",
    ".pytest_cache",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    ".vscode",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "site-packages",
    "venv",
]

# Same as Black.
line-length = 120
indent-width = 4

[tool.ruff.lint]
# Enable Pyflakes (`F`) and a subset of the pycodestyle (`E`)  codes by default.
# Unlike Flake8, Ruff doesn't enable pycodestyle warnings (`W`) or
# McCabe complexity (`C901`) by default.
select = ["E4", "E7", "E9", "F", "I001"]
ignore = ["E402","E501","N802","N803","N806","N801","N813","N815","N816","RUF001","RUF002","RUF003","RUF012","ARG001"]
extend-select = ["E", "N", "W", "ARG", "RUF"]

# Allow fix for all enabled rules (when `--fix`) is provided.
fixable = ["ALL"]
unfixable = []

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[tool.ruff.format]
# Like Black, use double quotes for strings.
quote-style = "double"

# Like Black, indent with spaces, rather than tabs.
indent-style = "space"

# Like Black, respect magic trailing commas.
skip-magic-trailing-comma = false

# Like Black, automatically detect the appropriate line ending.
line-ending = "auto"

# Enable auto-formatting of code examples in docstrings. Markdown,
# reStructuredText code/literal blocks and doctests are all supported.
#
# This is currently disabled by default, but it is planned for this
# to be opt-out in the future.
docstring-code-format = false

# Set the line length limit used when formatting code snippets in
# docstrings.
#
# This only has an effect when the `docstring-code-format` setting is
# enabled.
docstring-code-line-length = "dynamic"
```

my ruff config
```lua
  init_options = {
    filetypes = { "python" },
    single_file_support = true,
    cmd = { "ruff", "server", "--preview" },
    settings = {
      configuration = "~/.config/nvim/pyproject.toml",
    },
  },
```


---

_Label `server` added by @snowsignal on 2024-05-24 06:57_

---

_Assigned to @snowsignal by @snowsignal on 2024-05-24 06:57_

---

_Comment by @dhruvmanila on 2024-05-24 07:22_

Can you provide a minimal code snippet on which it's failing? And, just to confirm, by "failing" do you mean that the v0.4.4 wasn't raising any violations but v0.4.5 does?

---

_Comment by @a1401358759 on 2024-05-24 08:31_

@dhruvmanila 
![image](https://github.com/astral-sh/ruff/assets/15830146/7f0af8ac-d4c5-4e84-9ea3-cd0c8ecc4cc0)
I'm sorry, I wasn't accurate in my description. It's not a failure, it's a non-effect.

---

_Comment by @snowsignal on 2024-05-24 23:44_

I haven't been able to reproduce this issue. The `pyproject.toml` you put in the issue is the one in `~/.config/nvim`, right? Do you have any local `.toml` configuration files?

---

_Comment by @a1401358759 on 2024-05-25 03:39_

@snowsignal 

<img width="363" alt="image" src="https://github.com/astral-sh/ruff/assets/15830146/2f49f236-7178-4adc-a7aa-688b61bfeb01">

<img width="304" alt="image" src="https://github.com/astral-sh/ruff/assets/15830146/bf9e89e8-27ce-40ca-9aae-64746f498094">

I don't have any local `.toml` configuration files

---

_Comment by @dhruvmanila on 2024-05-27 11:12_

I think I found the bug.

---

_Label `bug` added by @dhruvmanila on 2024-05-27 11:12_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-05-27 11:14_

---

_Unassigned @snowsignal by @dhruvmanila on 2024-05-27 11:14_

---

_Renamed from "When I updated to version 0.4.5, the E30* checks all failed and nothing changed in my configuration" to "`ruff server` in `v0.4.5` doesn't consider the configuration setting in Neovim" by @dhruvmanila on 2024-05-27 11:15_

---

_Referenced in [astral-sh/ruff#11497](../../astral-sh/ruff/pulls/11497.md) on 2024-05-27 11:20_

---

_Referenced in [astral-sh/ruff#11566](../../astral-sh/ruff/pulls/11566.md) on 2024-05-27 11:23_

---

_Closed by @dhruvmanila on 2024-05-27 15:36_

---
