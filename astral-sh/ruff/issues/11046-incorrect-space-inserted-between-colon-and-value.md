```yaml
number: 11046
title: Incorrect space inserted between colon and value during slice operation
type: issue
state: closed
author: puffincircle
labels: []
assignees: []
created_at: 2024-04-19T15:54:33Z
updated_at: 2024-04-20T01:52:26Z
url: https://github.com/astral-sh/ruff/issues/11046
synced_at: 2026-01-12T15:54:50Z
```

# Incorrect space inserted between colon and value during slice operation

---

_@puffincircle_

ruff inserts a space between colon and value when slicing into a list.

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  "colon" "space"
* A minimal code snippet that reproduces the bug.
```python
from collections import namedtuple

Config = namedtuple("config", ["MAXIMUM_TITLE_LENGTH"])
config = Config(MAXIMUM_TITLE_LENGTH=2)


class Thing:
    def __init__(self) -> None:
        _thing = [1, 2, 3, 4]
        self.title = _thing[:config.MAXIMUM_TITLE_LENGTH]

```
```
❯ ruff format --check --diff --isolated thing.py
--- thing.py
+++ thing.py
@@ -7,4 +7,4 @@
 class Thing:
     def __init__(self) -> None:
         _thing = [1, 2, 3, 4]
-        self.title = _thing[:config.MAXIMUM_TITLE_LENGTH]
+        self.title = _thing[: config.MAXIMUM_TITLE_LENGTH]

1 file would be reformatted
```
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
`ruff format --check --diff --isolated thing.py`
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
```
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
    "generated"
]

# Same as Black.
line-length = 88
indent-width = 4

# Assume Python 3.8
target-version = "py38"

[lint]
# Enable Pyflakes (`F`) and a subset of the pycodestyle (`E`)  codes by default.
select = ["E4", "E7", "E9", "F", "I"]
ignore = []

# Allow fix for all enabled rules (when `--fix`) is provided.
fixable = ["ALL"]
unfixable = []

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[format]
# Unlike Black, use single quotes for strings.
quote-style = "single"

# Like Black, indent with spaces, rather than tabs.
indent-style = "space"

# Like Black, respect magic trailing commas.
skip-magic-trailing-comma = false

# Like Black, automatically detect the appropriate line ending.
line-ending = "auto"

```
* The current Ruff version (`ruff --version`).
```
❯ ruff --version
ruff 0.3.7
```

---

_Comment by @charliermarsh on 2024-04-20 01:51_

Black does the same, I believe this is intentional: https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AF8AOldAD2IimZxl1N_WlkPinBFn5AcJFdeQ5rAoKvfC-g2OAiCMbVJPC_c2FMs5ms-yHthMcV-dSexmZnwHrc5yvWXOUsnRQdlt5ou1YmloTwD8lscPLocpEicEl3JPIZrSiaLR_wCuCXTxOYnC5ga1Yu6kp9yIgIrj878KxWuFisptvvG_tAih3K1TSr8e0eRlzB5UoGnDlB36vgNHba5eLVcM9mdUvijkS1hMJSgA3JhRXQ55KK6wFkK2ImwVO-eHRPPWAM-OnFevHObX5xgl65r3GHn_N2oFoFjVmx-RnSR6kkVVTHWSQDZr8kwAAAAAAWe1uGEggU7AAGFAv0CAADCGGCAscRn-wIAAAAABFla

---

_Comment by @charliermarsh on 2024-04-20 01:51_

It only does this if the value is "complex". For example, there'd be no space inserted in `_thing[:MAXIMUM_TITLE_LENGTH]`.

---

_Closed by @charliermarsh on 2024-04-20 01:51_

---

_Comment by @charliermarsh on 2024-04-20 01:52_

(Happy to re-open if I'm misunderstanding.)

---
