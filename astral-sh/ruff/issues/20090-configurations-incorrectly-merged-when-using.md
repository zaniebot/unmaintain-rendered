```yaml
number: 20090
title: "Configurations incorrectly merged when using `extend` key"
type: issue
state: closed
author: A-CGray
labels:
  - question
assignees: []
created_at: 2025-08-25T19:57:51Z
updated_at: 2025-09-02T13:48:06Z
url: https://github.com/astral-sh/ruff/issues/20090
synced_at: 2026-01-10T11:09:59Z
```

# Configurations incorrectly merged when using `extend` key

---

_Issue opened by @A-CGray on 2025-08-25 19:57_

### Summary

I am running into issues trying to use the `extend` key to combine global and local ruff configurations.

My "global" ruff config file is at `~/.config/ruff/ruff.toml`:

```toml
target-version = "py39"

line-length = 120

exclude = [
    ".git",
    "**/__pycache__",
    "**/doc/conf.py",
    "**/__init__.py",
    "setup.py",
]

[lint]
ignore = [
    "E266", 
    "B006", 
    "E501", 
    "W605", 
    "D301", 
    "D10", 
    "D400", 
    "D205", 
    "D200", 
    "D202", 
    "D401", 
    "D404", 
]

select = [
    "B",
    "D",
    "E",
    "F",
    "W",
]

[lint.pydocstyle]
convention = "numpy"

[format]
quote-style = "double"
indent-style = "space"
docstring-code-format = true

[lint.per-file-ignores]
"tests/*" = ["D"] # Ignore docstring checks in tests
```

Then, in the root folder of a project called CMPLXFOIL, I have the following local ruff config that I want to add to the global config:
```toml
extend = "~/.config/ruff/ruff.toml"
extend-exclude =[
    "src_cs/complexify.py"
]
```

However, when I run `ruff check -v` in the root folder of this project I get the following:

```
[2025-08-25][15:31:05][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/ali/repos/CMPLXFOIL/ruff.toml
[2025-08-25][15:31:05][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-08-25][15:31:05][ruff_workspace::pyproject][DEBUG] No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified
[2025-08-25][15:31:05][ignore::gitignore][DEBUG] opened gitignore file: /home/ali/repos/CMPLXFOIL/.gitignore
[2025-08-25][15:31:05][ignore::gitignore][DEBUG] opened gitignore file: /home/ali/repos/CMPLXFOIL/.git/info/exclude
[2025-08-25][15:31:05][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
[2025-08-25][15:31:05][ruff_workspace::pyproject][DEBUG] No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/testflo_report.out: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "*.out", actual: "**/*.out", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/cmplxfoil.egg-info: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "*.egg-info/", actual: "**/*.egg-info", is_whitelist: false, is_only_dir: true })))
[2025-08-25][15:31:05][ignore::gitignore][DEBUG] opened gitignore file: /home/ali/repos/CMPLXFOIL/.ruff_cache/.gitignore
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/.pre-commit-config.yaml: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: ".pre-commit-config.yaml", actual: "**/.pre-commit-config.yaml", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/.ruff_cache/CACHEDIR.TAG: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.ruff_cache/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/.ruff_cache/0.12.10: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.ruff_cache/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/.ruff_cache/0.12.7: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.ruff_cache/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/.ruff_cache/.gitignore: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.ruff_cache/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/src/build: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "build/", actual: "**/build", is_whitelist: false, is_only_dir: true })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/config.mk: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "config.mk", actual: "**/config.mk", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/cmplxfoil/libcmplxfoil.so: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "*.so", actual: "**/*.so", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/config/config.mk: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "config/config*", actual: "config/config*", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/ali/repos/CMPLXFOIL/doc/conf.py"
[2025-08-25][15:31:05][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/ali/repos/CMPLXFOIL/setup.py"
[2025-08-25][15:31:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/ali/repos/CMPLXFOIL/cmplxfoil/MExt.py"
[2025-08-25][15:31:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/ali/repos/CMPLXFOIL/examples/single_point.py"
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/tests/__pycache__: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "__pycache__/", actual: "**/__pycache__", is_whitelist: false, is_only_dir: true })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/cmplxfoil/__pycache__: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "__pycache__/", actual: "**/__pycache__", is_whitelist: false, is_only_dir: true })))
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/cmplxfoil/libcmplxfoil_cs.so: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "*.so", actual: "**/*.so", is_whitelist: false, is_only_dir: false })))
[2025-08-25][15:31:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/ali/repos/CMPLXFOIL/tests/test_solver_class.py"
[2025-08-25][15:31:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/ali/repos/CMPLXFOIL/cmplxfoil/__init__.py"
[2025-08-25][15:31:05][ignore::walk][DEBUG] ignoring /home/ali/repos/CMPLXFOIL/src_cs/build: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ali/repos/CMPLXFOIL/.gitignore"), original: "build/", actual: "**/build", is_whitelist: false, is_only_dir: true })))
[2025-08-25][15:31:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/ali/repos/CMPLXFOIL/cmplxfoil/CMPLXFOIL.py"
[2025-08-25][15:31:05][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/ali/repos/CMPLXFOIL/cmplxfoil/postprocess.py"
[2025-08-25][15:31:05][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/ali/repos/CMPLXFOIL/.git"
[2025-08-25][15:31:05][ruff::commands::check][DEBUG] Identified files to lint in: 5.243998ms
[2025-08-25][15:31:05][ruff::commands::check][DEBUG] Checked 7 files in: 145.042µs
cmplxfoil/__init__.py:3:24: F401 `.CMPLXFOIL.CMPLXFOIL` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
  |
1 | __version__ = "2.1.2"
2 |
3 | from .CMPLXFOIL import CMPLXFOIL
  |                        ^^^^^^^^^ F401
4 |
5 | try:
  |
  = help: Use an explicit re-export: `CMPLXFOIL as CMPLXFOIL`

cmplxfoil/__init__.py:6:30: F401 `.postprocess.AnimateAirfoilOpt` imported but unused; consider using `importlib.util.find_spec` to test for availability
  |
5 | try:
6 |     from .postprocess import AnimateAirfoilOpt
  |                              ^^^^^^^^^^^^^^^^^ F401
7 | except ImportError:
8 |     pass
  |
  = help: Remove unused import: `.postprocess.AnimateAirfoilOpt`

doc/conf.py:1:1: F403 `from sphinx_mdolab_theme.config import *` used; unable to detect undefined names
  |
1 | from sphinx_mdolab_theme.config import *
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ F403
2 |
3 | # -- Path setup --------------------------------------------------------------
  |

doc/conf.py:25:1: F405 `extensions` may be undefined, or defined from star imports
   |
23 | # here we add external extensions, which must also be added to requirements.txt
24 | # so that RTD can import and use them
25 | extensions.extend([])
   | ^^^^^^^^^^ F405
26 |
27 | # mock import for autodoc
   |

Found 4 errors.
```

It seems like ruff is ignoring several of the configuration options specified in my global config file. For example, it complains that "No `target-version` found in `ruff.toml`" despite this being specified in my global config file. Additionally it is running checks on files such as `__init__.py` and `doc/conf.py` that are specifically excluded from checks in the global config.

If I run `ruff check` in a different project that doesn't have its own local ruff config, the global config file is found and used correctly, so I don't think there are any issues with the content of that file, the issue seems to be with how the local and global configs are merged when using `extend`.

### Version

ruff 0.12.10

---

_Comment by @ntBre on 2025-08-25 20:59_

Thanks for the report!

I'm having some trouble reproducing this. Are there any other configuration files around that could be having an impact?

I tried a fresh docker image with this local config:

```toml
extend = "~/.config/ruff/ruff.toml"
extend-exclude =[
    "src_cs/complexify.py"
]
```

A global ruff config at `~/.config/ruff/ruff.toml`:

```toml
target-version = "py313"

exclude = [
    ".git",
    "**/__pycache__",
    "**/doc/conf.py",
    "**/__init__.py",
    "setup.py",
]

[lint]
select = ["UP046"]
```

And a Python file with an UP046 violation for the configured Python version in `src/try.py`:

```py
from typing import TypeVar, Generic

T = TypeVar("T")

class C(Generic[T]): ...
```

This reports a diagnostic as written and no diagnostic if I comment out the  local `extend` setting.

I'm not sure the logs are going to be very helpful here. I didn't see any logging calls around where we do the extending, and my logs don't show any mention of `~/.config/ruff/ruff.toml` either. The `--show-settings` flag, however, does show the correct `target_version`.

There is a known issue with overlapping configuration options in https://github.com/astral-sh/ruff/issues/9872, which you could be running into if your local config is longer than what's included here.

---

_Label `question` added by @ntBre on 2025-08-25 20:59_

---

_Comment by @A-CGray on 2025-08-25 22:29_

Thanks for the quick response @ntBre !

I created a MWE at https://github.com/A-CGray/ruffMWE, it has an even simpler set of config files and still shows the issue on my machine. The `select` key from the global config is respected while the `exclude` key is not. Would you be able to clone it and see if you get the same behaviour described in the readme?

---

_Comment by @ewu63 on 2025-08-25 23:23_

FWIW may be related to [this report](https://github.com/astral-sh/ruff/issues/8795#issuecomment-3215813669) I made

---

_Comment by @A-CGray on 2025-08-25 23:41_

> FWIW may be related to [this report](https://github.com/astral-sh/ruff/issues/8795#issuecomment-3215813669) I made

You're right, if I pass the local config file explicitly (`ruff check -v --config ruff.toml`) this issue goes away. As suggested by a comment on that issue, if I change the exclude from `"doc/conf.py"` to `"/**/doc/conf.py"` then the issue also goes away. so clearly it is a matter of the paths in the global config not being correctly treated as relative to the location of the local config.

---

_Comment by @Teyik0 on 2025-09-02 06:25_

Also found that lint.per-file-ignores don't work when using extend configuration.

```bash
# ruff_base.toml
[lint.per-file-ignores]
"tests/**/*.py" = [
    "S101",    # Allow assert statements
    "PLR2004", # Allow magic values in tests
    "SLF001",  # Allow private member access
]

# pyproject.toml
[tool.ruff]
extend = "src/ultrapyup/resources/ruff_base.toml"
```

The rules actually works when inside pyproject.toml directly.
```bash
# pyproject.toml
[tool.ruff]
extend = "src/ultrapyup/resources/ruff_base.toml"
[tool.ruff.lint.per-file-ignores] # Workaround for now
"tests/**/*.py" = [
    "S101",    # Allow assert statements
    "PLR2004", # Allow magic values in tests
    "SLF001",  # Allow private member access
]
```

Currently trying to build something like [ultracite](https://www.ultracite.ai/) but for the python ecosystem, this is blocking.
You may label this as a bug

---

_Comment by @ntBre on 2025-09-02 13:48_

Thank y'all! I think I'll close this in favor of #8795 since it sounds like the root cause is the same (relative paths in `extend`ed globs). That way we can keep the discussion in one place.

---

_Closed by @ntBre on 2025-09-02 13:48_

---
