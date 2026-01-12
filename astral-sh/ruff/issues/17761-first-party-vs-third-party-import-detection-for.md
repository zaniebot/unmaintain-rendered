```yaml
number: 17761
title: First-party vs third-party import detection for modules built with C++/Rust
type: issue
state: closed
author: heiner
labels:
  - bug
  - isort
assignees: []
created_at: 2025-05-01T08:15:05Z
updated_at: 2025-05-05T16:40:02Z
url: https://github.com/astral-sh/ruff/issues/17761
synced_at: 2026-01-12T15:54:56Z
```

# First-party vs third-party import detection for modules built with C++/Rust

---

_@heiner_

### Summary

This is a spin out of https://github.com/astral-sh/ruff/issues/16712

Consider this setup:

Consider the following setup

```
$ tree
.
├── crates
│   └── my-package
│       └── pyproject.toml
├── known.py
├── main.py
└── pyproject.toml

3 directories, 4 files
```

With these contents:

```
$ cat crates/my-package/pyproject.toml
[project]
name = "my-package"
version = "0.1.0"

[build-system]
build-backend = "maturin"
requires = ["maturin>=1.0,<2.0"]

[tool.maturin]
bindings = "pyo3"
module-name = "my_package"


$ cat known.py


$ cat main.py
import os
import sys

import my_package
import torch
import unknown

import known


def main():
    print("Hello from myexample!")
    sys / my_package / os / unknown / torch / known


if __name__ == "__main__":
    main()


$ cat pyproject.toml
[project]
name = "myexample"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.13"
dependencies = []

[tool.ruff]
src = [
    ".",
    "crates/*",
]

[tool.ruff.lint]
preview = true
select = [
    "I00",    # unsorted-imports, missing-required-import.
]
```

(imagine there's also `crates/my-package/src/lib.rs` that implements a PyO3 package, say).

The sort order here is what ruff will produce:

```
import os
import sys

import my_package
import torch
import unknown

import known
```

However, `my_package` is first-party. The actual module will be produced as an .so upon building.

Note that having `crates/my-package/my_package.pyi` does not change the behavior.

There seems to be no setting for `src` in `[tool.ruff]` that makes it identify the `my_package` import as first-party.

Initially I assumed it was the underscore that mislead it, but renaming `crates/my-package` to `crates/my_package` does not change its behavior.

### Version

ruff 0.11.7

---

_Comment by @MichaReiser on 2025-05-01 09:18_

Here's a repro: [isort.zip](https://github.com/user-attachments/files/19998480/isort.zip)

It seems Ruff only checks for the existence of directories with the import name and files ending in `.py` but not with `.pyi`. This should be an easy fix or is there a specific reason we skip `pyi` (@dylwil3 ?)

https://github.com/astral-sh/ruff/blob/a192d96880399413432a0316e24ce7982cbc3cb2/crates/ruff_linter/src/rules/isort/categorize.rs#L161-L170

Also testing for `.pyi` gives

```
[2025-05-01][11:18:49][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'my_package' as Known(FirstParty) (SourceMatch("/Users/micha/astral/test/isort/crates/my-package"))
```

For know, adding `my_package` to known first party is a possible work around

---

_Label `bug` added by @MichaReiser on 2025-05-01 09:20_

---

_Label `isort` added by @MichaReiser on 2025-05-01 09:20_

---

_Comment by @dylwil3 on 2025-05-01 16:07_

This will already be fixed in preview by https://github.com/astral-sh/ruff/pull/16565 - which will hopefully land soon. Just waiting on me to add some documentation to thr PR, sorry for the delay!

Since we consider this a bug, I can also make the non-preview version of the check look for `.pyi` files.

---

_Comment by @MichaReiser on 2025-05-01 16:13_

I'm fine with going with preview first.

---

_Closed by @dylwil3 on 2025-05-05 16:40_

---
