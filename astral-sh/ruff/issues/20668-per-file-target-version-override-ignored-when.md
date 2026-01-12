```yaml
number: 20668
title: "`per-file-target-version` override ignored when combined with a broader pattern"
type: issue
state: open
author: mgaitan
labels:
  - configuration
assignees: []
created_at: 2025-10-01T12:38:13Z
updated_at: 2025-10-01T13:53:10Z
url: https://github.com/astral-sh/ruff/issues/20668
synced_at: 2026-01-12T15:54:57Z
```

# `per-file-target-version` override ignored when combined with a broader pattern

---

_@mgaitan_

### Summary

When using `per-file-target-version` with both a broad pattern (e.g., `*.py`) and a specific file override, Ruff appears to always apply the broad pattern, ignoring the more specific one.

### Minimal reproducible example

Repo: https://github.com/mgaitan/demo_ruff_bug_per_file_ignore

**src/demo/example.py**
```python
from typing import List

def example_function() -> List[str]:
    return ["Hello", "World"]
```


with   `pyproject.toml`
```toml
[tool.ruff.lint]
select = [
    "UP",     # https://docs.astral.sh/ruff/rules/#pyupgrade-up
]

[tool.ruff.per-file-target-version]
"src/demo/*.py" = "py38"
"src/demo/example.py" = "py312"
```

```bash
$ uvx ruff@latest check --verbose src/ --no-cache
Installed 1 package in 6ms
[2025-10-01][09:25:29][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/tin/lab/demo/pyproject.toml
[2025-10-01][09:25:29][ruff_workspace::pyproject][DEBUG] Detected minimum supported `requires-python` version: 3.8
[2025-10-01][09:25:29][ignore::gitignore][DEBUG] opened gitignore file: /home/tin/lab/demo/.gitignore
[2025-10-01][09:25:29][ignore::gitignore][DEBUG] opened gitignore file: /home/tin/lab/demo/.git/info/exclude
[2025-10-01][09:25:29][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/tin/lab/demo/src/demo/example.py"
[2025-10-01][09:25:29][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/tin/lab/demo/src/demo/__init__.py"
[2025-10-01][09:25:29][ruff::commands::check][DEBUG] Identified files to lint in: 3.841828ms
[2025-10-01][09:25:29][ruff::diagnostics][DEBUG] Checking: /home/tin/lab/demo/src/demo/example.py
[2025-10-01][09:25:29][ruff::diagnostics][DEBUG] Checking: /home/tin/lab/demo/src/demo/__init__.py
[2025-10-01][09:25:29][ruff_linter::settings::types][DEBUG] Setting Python version for "/home/tin/lab/demo/src/demo/example.py" due to absolute match on "(?-u)^/home/tin/lab/demo/src/demo/.*\\.py$": PythonVersion { major: 3, minor: 8 }
[2025-10-01][09:25:29][ruff_linter::settings::types][DEBUG] Setting Python version for "/home/tin/lab/demo/src/demo/__init__.py" due to absolute match on "(?-u)^/home/tin/lab/demo/src/demo/.*\\.py$": PythonVersion { major: 3, minor: 8 }
[2025-10-01][09:25:29][ruff::commands::check][DEBUG] Checked 2 files in: 4.023072ms
All checks passed!
```

No `UP` warnings are raised, even though `example.py` should be treated as `py312`.

Or course, if I change the config to:

```toml
[tool.ruff.per-file-target-version]
"src/demo/*.py" = "py312"
```

then Ruff behaves as expected:

```bash
$ uvx ruff@latest check --verbose src/ --no-cache
Installed 1 package in 3ms
[2025-10-01][09:26:32][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/tin/lab/demo/pyproject.toml
[2025-10-01][09:26:32][ruff_workspace::pyproject][DEBUG] Detected minimum supported `requires-python` version: 3.8
[2025-10-01][09:26:32][ignore::gitignore][DEBUG] opened gitignore file: /home/tin/lab/demo/.gitignore
[2025-10-01][09:26:32][ignore::gitignore][DEBUG] opened gitignore file: /home/tin/lab/demo/.git/info/exclude
[2025-10-01][09:26:32][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/tin/lab/demo/src/demo/example.py"
[2025-10-01][09:26:32][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/tin/lab/demo/src/demo/__init__.py"
[2025-10-01][09:26:32][ruff::commands::check][DEBUG] Identified files to lint in: 2.363499ms
[2025-10-01][09:26:32][ruff::diagnostics][DEBUG] Checking: /home/tin/lab/demo/src/demo/example.py
[2025-10-01][09:26:32][ruff::diagnostics][DEBUG] Checking: /home/tin/lab/demo/src/demo/__init__.py
[2025-10-01][09:26:32][ruff_linter::settings::types][DEBUG] Setting Python version for "/home/tin/lab/demo/src/demo/example.py" due to absolute match on "(?-u)^/home/tin/lab/demo/src/demo/.*\\.py$": PythonVersion { major: 3, minor: 12 }
[2025-10-01][09:26:32][ruff_linter::settings::types][DEBUG] Setting Python version for "/home/tin/lab/demo/src/demo/__init__.py" due to absolute match on "(?-u)^/home/tin/lab/demo/src/demo/.*\\.py$": PythonVersion { major: 3, minor: 12 }
[2025-10-01][09:26:32][ruff::commands::check][DEBUG] Checked 2 files in: 900.833µs
UP035 `typing.List` is deprecated, use `list` instead
 --> src/demo/example.py:1:1
  |
1 | from typing import List
  | ^^^^^^^^^^^^^^^^^^^^^^^
2 |
3 | def example_function() -> List[str]:
  |

UP006 [*] Use `list` instead of `List` for type annotation
 --> src/demo/example.py:3:27
  |
1 | from typing import List
2 |
3 | def example_function() -> List[str]:
  |                           ^^^^
4 |     return ["Hello", "World"]
  |
help: Replace with `list`

Found 2 errors.
[*] 1 fixable with the `--fix` option.
```

---

### Conclusion
It seems that when multiple overlapping `per-file-target-version` rules exist, Ruff applies the broader one instead of the more specific one. The expected behavior would be for the more specific match (`src/demo/example.py = py312`) to take precedence.  


### Version

ruff 0.13.2

---

_Comment by @ntBre on 2025-10-01 12:41_

Have you tried swapping the order of the two patterns? I need to double check, but I think we maintain the order of the list and return when the first matching pattern is found. This might be worth documenting too, if it works.

---

_Label `question` added by @ntBre on 2025-10-01 12:41_

---

_Comment by @ntBre on 2025-10-01 12:43_

Ah sorry, I should have just tried the example repo (thanks for that!). It looks like swapping the order doesn't help.

---

_Label `question` removed by @ntBre on 2025-10-01 12:51_

---

_Label `configuration` added by @ntBre on 2025-10-01 12:51_

---

_Comment by @ntBre on 2025-10-01 12:57_

We do end up saving the resolved patterns in an ordered [collection](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/settings/types.rs#L835), which is what I was thinking of, but we deserialize them from the config file into an unordered [hash map](https://github.com/astral-sh/ruff/blob/main/crates/ruff_workspace/src/options.rs#L360). One potential fix here could be sorting the patterns by descending length before collecting them into the settings.

---

_Comment by @MichaReiser on 2025-10-01 13:53_

Could we use an `IndexMap` instead?

---
