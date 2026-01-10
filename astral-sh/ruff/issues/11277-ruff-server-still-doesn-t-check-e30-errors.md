```yaml
number: 11277
title: "ruff server still doesn't check E30* errors"
type: issue
state: closed
author: a1401358759
labels:
  - bug
  - server
assignees: []
created_at: 2024-05-04T04:07:52Z
updated_at: 2024-05-04T17:51:28Z
url: https://github.com/astral-sh/ruff/issues/11277
synced_at: 2026-01-10T11:09:53Z
```

# ruff server still doesn't check E30* errors

---

_Issue opened by @a1401358759 on 2024-05-04 04:07_

ruff version: 0.4.3

my lsp config

```lua
return {
  filetypes = { "python" },
  single_file_support = true,
  cmd = { "ruff", "server", "--preview" },
  settings = {
    configuration = "~/.config/nvim/pyproject.toml",
  },
}
```

my pyprohect.toml
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
select = ["E4", "E7", "E9", "F"]
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

but ruff server still doesn't check E30* errors

<img width="611" alt="image" src="https://github.com/astral-sh/ruff/assets/15830146/653cd1af-6218-434d-97c6-88ef74e0f674">

Is there still a problem with my configuration?

_Originally posted by @a1401358759 in https://github.com/astral-sh/ruff/issues/10814#issuecomment-2093997784_
            

---

_Renamed from "ruff server still doesn't check E30* erro" to "ruff server still doesn't check E30* errors" by @a1401358759 on 2024-05-04 04:09_

---

_Comment by @a1401358759 on 2024-05-04 04:12_

@snowsignal 

---

_Comment by @charliermarsh on 2024-05-04 13:39_

Can you use the full path to the file rather than a `~`?

---

_Label `bug` added by @charliermarsh on 2024-05-04 13:40_

---

_Label `server` added by @charliermarsh on 2024-05-04 13:40_

---

_Closed by @charliermarsh on 2024-05-04 17:51_

---
