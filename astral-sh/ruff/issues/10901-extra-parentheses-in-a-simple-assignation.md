```yaml
number: 10901
title: Extra parentheses in a simple assignation
type: issue
state: closed
author: enriquesanchezb
labels:
  - needs-info
assignees: []
created_at: 2024-04-12T07:48:59Z
updated_at: 2024-08-17T14:37:11Z
url: https://github.com/astral-sh/ruff/issues/10901
synced_at: 2026-01-10T11:09:53Z
```

# Extra parentheses in a simple assignation

---

_Issue opened by @enriquesanchezb on 2024-04-12 07:48_

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

Hello folks!

In my company, we are migrating one repo we have to Ruff and I found this weird behaviour in a file. We have something like this:
`payload["identity"]["clientUUID"] = client_uuid`
and it changing it to
`payload["identity"]["clientUUID"] = (client_uuid)`

(Note: the name of the variables are longer)

Anyway, it's a simple variable and I don't think is necesssary

---

_Label `needs-info` added by @MichaReiser on 2024-04-12 07:53_

---

_Comment by @MichaReiser on 2024-04-12 07:55_

Hy. Thanks for reporting this. 

We'll need some more information to reproduce the issue. What command are you running (`ruff check` or `ruff format`)? what's your Ruff configuration (what rules have you enabled)? Can you share a specific code example where this reformatting happens or at least an example that uses identifier names that have the same length?

---

_Comment by @enriquesanchezb on 2024-04-12 08:36_

Hi Micha,

thanks for your fast answer. We are using `ruff format <path>` this is our config:

```
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
    "conftest.py"
]

# Same as Black.
line-length = 88

# Assume Python 3.9
target-version = "py39"

[lint]
# Enable Pyflakes (`F`) and a subset of the pycodestyle (`E`)  codes by default.
select = ["E4", "E7", "E9", "F"]
ignore = ["F401","F403","F405","F811"]

# Allow fix for all enabled rules (when `--fix`) is provided.
fixable = ["ALL"]
unfixable = []

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[format]
# Like Black, use double quotes for strings.
quote-style = "double"

# Like Black, indent with spaces, rather than tabs.
indent-style = "space"

# Like Black, automatically detect the appropriate line ending.
line-ending = "auto"
```

and the variables names are something like this:

```
payload_data_for_verify["persona"]["demandSpecificationUID"] = demand_specification_uid
```

---

_Comment by @MichaReiser on 2024-04-12 09:34_

Thanks for the extra information. What Ruff version are you using? 

I tried to reproduce (I copied the formatter configuration) this in the [Playground](https://play.ruff.rs/3a513818-e0c8-45e2-95f7-80b212e44351) but the value doesn't get parenthesized if the line fits into the configured line length.

However, the value gets parenthesized if the entire assignment exceeds the configured line length because keeping the code in the configured line length is one of the formatter's key objectives

```python
payload_data_for_verify["persona"]["demandSpecificationUID"] = demand_specification_uid

payload_data_for_verify["persona"]["demandSpecificationUIDDDD"] = (
    demand_specification_uid
)
```

---

_Closed by @MichaReiser on 2024-08-17 14:37_

---
