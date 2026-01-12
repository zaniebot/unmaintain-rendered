```yaml
number: 1841
title: "`ty` ignores type hints for C extension"
type: issue
state: closed
author: albireox
labels:
  - needs-info
  - imports
assignees: []
created_at: 2025-12-10T16:00:37Z
updated_at: 2025-12-11T15:05:01Z
url: https://github.com/astral-sh/ty/issues/1841
synced_at: 2026-01-12T15:54:25Z
```

# `ty` ignores type hints for C extension

---

_@albireox_

### Summary

I have a small project that using `pyo3` to create Python bindings for some Rust code. The package also includes some Python code to import and expose some of the bindings. The `python` directory structure is

```
python/
└─ rheader/
   ├─ __init__.py
   ├─ _rheader.abi3.so
   ├─ _rheader.pyi
   └─ py.typed
```

where `_rheader.abi3.so` is the compiled library produced using `maturin`, and I added `_rheader.pyi` type hints. 

`_rheader.pyi`:

```python
class Header:
    """Representation of a header."""

    keywords: dict[str, Keyword]

class Keyword:
    """Representation of a header keyword."""

    name: str
    value: str | int | float | bool | None
    comment: str | None

def read_header(file_path: str) -> dict:
    """Reads a header from a file and returns it as a dictionary."""
    ...

def read_header_to_class(file_path: str, cls: type) -> Header:
    """Reads a header from a file and returns it as an instance of :obj:`.Header`."""
    ...
```

and `__init__.py`:

```python
__all__ = [
    "Header",
    "Keyword",
    "read_header",
    "read_header_to_class",
]


from ._rheader import Header, Keyword, read_header, read_header_to_class
```

In `__init__.py` `ty` complains that `Cannot resolve imported module ._rheader`. This happens whether I use `pyo3` with the `abi3-py39` feature or not.

### Version

_No response_

---

_Comment by @carljm on 2025-12-10 16:07_

Do you have a `pyproject.toml` or `ty.toml` anywhere? From what current-working-directory do you run ty?

---

_Label `needs-info` added by @carljm on 2025-12-10 16:07_

---

_Label `imports` added by @carljm on 2025-12-10 16:07_

---

_Comment by @albireox on 2025-12-10 16:09_

Here's the complete file structure

```
rheader/
├─ .vscode/
│  └─ settings.json
├─ python/
│  └─ rheader/
│     ├─ __init__.py
│     ├─ _rheader.abi3.so
│     ├─ _rheader.pyi
│     └─ py.typed
├─ src/
│  ├─ header.rs
│  ├─ lib.rs
│  ├─ python.rs
│  └─ tools.rs
├─ .envrc
├─ .gitignore
├─ Cargo.lock
├─ Cargo.toml
├─ pyproject.toml
├─ README.md
└─ uv.lock
```

The `pyproject.toml` looks like

```toml
[project]
name = "rheader"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.9"
dependencies = []

[build-system]
requires = ["maturin>=1.0,<2.0"]
build-backend = "maturin"

[tool.maturin]
python-source = "python"
module-name = "rheader._rheader"

[dependency-groups]
dev = [
    "ipdb>=0.13.13",
    "ipython>=8.18.1",
]
```

and the `Cargo.toml`

```toml
[package]
name = "rheader"
version = "0.1.0"
edition = "2024"

[lib]
name = "rheader"
crate-type = ["cdylib"]

[dependencies]
anyhow = "1.0.100"
bytes = "1.11.0"
flate2 = { version = "1.1.5", features = ["zlib-rs"], default-features = false }
regex = "1.12.2"

[dependencies.pyo3]
version = "0.27.2"
features = ["abi3-py39"]
```

---

_Comment by @MichaReiser on 2025-12-10 16:23_

You'll need to tell `ty` that the root of your python code is in `python` by setting `environment.root = ["python"]`. See https://docs.astral.sh/ty/modules/#first-party-modules

---

_Comment by @MichaReiser on 2025-12-10 16:24_

CC: @Gankra a case where our desperate resolution mode doesn't work.

---

_Comment by @albireox on 2025-12-10 16:26_

Ah! You're completely right and I had missed that part of the documentation. It works as expected when I set the root. Thanks!

---

_Closed by @MichaReiser on 2025-12-10 16:26_

---

_Comment by @carljm on 2025-12-10 16:27_

I'm actually surprised this didn't work as-is. Even without `environment.root = ["python"]`, I would have thought given the top-level outer dir as root, we'd have just treated it as a package named `python.rheader` and the relative import would still work...

---

_Comment by @AlexWaygood on 2025-12-10 16:28_

> You'll need to tell `ty` that the root of your python code is in `python` by setting `environment.root = ["python"]`. See https://docs.astral.sh/ty/modules/#first-party-modules

@michareiser I thought we already automatically added a top-level `python/` directory as a search path whenever we saw one, since this is such a common directory structure for maturin-based projects 

---

_Comment by @AlexWaygood on 2025-12-10 16:29_

See
- https://github.com/astral-sh/ruff/pull/20263

---

_Comment by @carljm on 2025-12-10 16:33_

I wasn't able to successfully reproduce this issue:

```
ruff-examples/issue1841 on  main [✘?]
➜ tree
.
├── main.py
├── pyproject.toml
├── python
│   └── rheader
│       ├── __init__.py
│       └── _rheader.pyi
└── README.md

3 directories, 5 files

ruff-examples/issue1841 on  main [✘?]
➜ cat python/rheader/__init__.py
from ._rheader import X

reveal_type(X)

ruff-examples/issue1841 on  main [✘?]
➜ cat python/rheader/_rheader.pyi
X = 1

ruff-examples/issue1841 on  main [✘?]
➜ cat pyproject.toml
[project]
name = "issue1841"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []

ruff-examples/issue1841 on  main [✘?]
➜ uvx ty check .
warning[undefined-reveal]: `reveal_type` used without importing it
 --> python/rheader/__init__.py:3:1
  |
1 | from ._rheader import X
2 |
3 | reveal_type(X)
  | ^^^^^^^^^^^
  |
info: This is allowed for debugging convenience but will fail at runtime
info: rule `undefined-reveal` is enabled by default

info[revealed-type]: Revealed type
 --> python/rheader/__init__.py:3:13
  |
1 | from ._rheader import X
2 |
3 | reveal_type(X)
  |             ^ `Literal[1]`
  |

Found 2 diagnostics
```

So there is some factor here I don't understand.

---

_Comment by @MichaReiser on 2025-12-10 16:37_

Maybe there's some `ty.toml` or `pyproject.toml` with a `tool.ty` section in one of the ancestor directories?

---

_Comment by @carljm on 2025-12-10 16:38_

That would explain it -- in that case adding the `environment.root` wasn't the key factor, just adding any kind of ty config at all, in the right place...

---

_Comment by @albireox on 2025-12-10 20:27_

At least in my case I don't think I have any `pyproject.toml` above this project with a `[tool.ty]` section. I'm only now starting to use `ty` and I'm fairly sure I've never added a `[tool.ty]` section. Same with `ty.toml`.

---

_Comment by @carljm on 2025-12-10 20:59_

@albireox what was your current working directory you were running ty from?

---

_Comment by @albireox on 2025-12-10 21:04_

I was running it using the VSCode extension and I had the root of the directory (where the `Cargo.toml` and `pyproject.toml` files are).

---

_Comment by @Gankra on 2025-12-11 15:05_

If we auto-add `python/` as a root then this looks suspiciously like [this known broken case](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/workspaces.md#tests-directory-with-ambiguous-project-directories-via-editables)

But if you're running from within the top-level rheader directory that case shouldn't be relevant...

---
