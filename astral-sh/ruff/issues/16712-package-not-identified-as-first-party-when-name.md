```yaml
number: 16712
title: Package not identified as first party when name has a dash
type: issue
state: open
author: bepuca
labels:
  - question
  - isort
  - uv
assignees: []
created_at: 2025-03-13T16:48:36Z
updated_at: 2025-09-23T07:25:18Z
url: https://github.com/astral-sh/ruff/issues/16712
synced_at: 2026-01-10T11:09:57Z
```

# Package not identified as first party when name has a dash

---

_Issue opened by @bepuca on 2025-03-13 16:48_

### Summary

First party package is not identified as such when sorting imports if it has dashes in the name and the code is parallel to the actual package folder which has an underscore in the name. For instance, on tests files.

(unsure if by design or a bug)

### Context

- I am using uv workspaces.
- Some of the packages have dashes in their names.
- I am using ruff for sorting imports
- A minimal folder structure

```text
packages/
├── pyproject.toml
└── example-package/
    ├── src/
    │   └── example_package/
    │       ├── __init__.py
    │       └── dummy.py
    ├── tests/
    │   └── test_dummy.py
    └── pyproject.toml
```

**pyproject.toml**
```toml
[project]
name = "tmp-iywsagbejg"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "example_package",
]

[tool.uv.workspace]
members = ["packages/*"]

[tool.uv.sources]
example_package = { workspace = true }

[tool.ruff]
lint.select = ["I"]
```

**example-package/pyproject.toml**
```toml
[project]
name = "example-package"
version = "0.1.0"
description = "Example package"
dependencies = [
    "requests",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

**dummy.py**
```python
import requests

import example_package


def myfun():
    example_package.main()
    print(requests.__version__)
    return 42
```

**test_dummy.py**
```
import example_package
import requests


def test_myfun():
    print(example_package.dummy.myfun(), requests.__version__)
```

### Expected

When I run:

```bash
vx ruff check --fix .
```

I would expect that `example-package` is always identified as first party when sorting imports and so order of imports is consistent.

### Actual behavior

It is only detected as first party if inside a folder with underscores and not dashes in the name. Thus, for tests (if I want them to live outside the `src` folder) the imports are sorted differently. (see example files above)


---

_Comment by @MichaReiser on 2025-03-13 17:20_

Thanks for the excellent write-up. I won't be able to reproduce this myself today because it's already late but I suspect this is due to Ruff defaulting the root for module resolution to the project-root (in which case it picks up `example-package`). Could you try setting the [`src` setting](https://docs.astral.sh/ruff/settings/#src) to `packages/example-package/src`

---

_Label `question` added by @MichaReiser on 2025-03-13 17:20_

---

_Label `isort` added by @MichaReiser on 2025-03-13 17:20_

---

_Comment by @bepuca on 2025-03-13 17:39_

Thank you for the swift response @MichaReiser ! That does indeed lead to the expected behavior. But does this mean I have to add every package explicitly? Is there a way to have a "generic" pointer in the `src setting` so it resolves as I was expecting for all packages?

For clarity, I can add that but it adds more boiler plate and potential confusion for people less familiar with it so I am curious if there is a "cleaner" way.

---

_Comment by @MichaReiser on 2025-03-13 18:30_

Yeah, you have to add each package manually, which is annoying. An alternative would be to add a `[tool.ruff]` section to every package's `pyproject.toml` that contains a single `extend = "../../pyproject.toml"` but that's somewhat silly too. 

Better mono repo support, especially for uv, is something that's on our mind. CC: @zanieb 

---

_Label `incompatibility` added by @MichaReiser on 2025-03-13 18:30_

---

_Label `incompatibility` removed by @MichaReiser on 2025-03-13 18:31_

---

_Label `uv` added by @MichaReiser on 2025-03-13 18:31_

---

_Comment by @bepuca on 2025-03-14 08:35_

Awesome! I can definitely work with that, thank you very much! From my side, the issue could be closed. I take the opportunity: you know that already but amazing work you are doing, I enjoy astral's stuff SO much.

---

_Comment by @bepuca on 2025-03-14 08:48_

For reference, reading again the docs I realized globs are allowed, so in the end what I find quite a neat solution is at the top level `pyproject.toml`:

```toml
[tool.ruff]
src = ["packages/**/src"]
```

That does sort imports as expected for all packages.

---

_Comment by @MichaReiser on 2025-03-14 08:54_

Oh, I wasn't aware that you can use a glob for `src`. Nice find!

---

_Comment by @heiner on 2025-05-01 07:23_

Note that `src` in `[tool.ruff]` doesn't help in cases where the imported module is an .so file that is built via e.g. C++ or Rust. As an example, a `pyproject.toml` could look like

```
[project]
name = "my-package"
version = "0.1.0"

[build-system]
build-backend = "maturin"
requires = ["maturin>=1.0,<2.0"]

[tool.maturin]
bindings = "pyo3"
module-name = "my_package"
```

The `my_package.so` file will be around only when the package is built. Currently this is identified as third party (even `my_package.pyi` exists).

---

_Comment by @MichaReiser on 2025-05-01 07:27_

@heiner can you provide some more details about your project structure. What's the path to `my_package` (and the corresponding `pyi` file)? 

---

_Comment by @heiner on 2025-05-01 07:44_

Thanks for the quick response!

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

---

_Comment by @MichaReiser on 2025-05-01 08:00_

Can you try adding `crates/my-package` to `src`? Ruff can't find your package because it only searches for packages in your root (and `src` folder) but there's no `my-package` in that folder.

Python requires this too to find `my_package`: It will search in your root directory, followed by your venv. I assume you use editable installs or similar to link `my-package` into your venv and that's where python finds it.

Edit: We may want to create a separate issue. While related, it's different enough from what was asked by the original author.

---

_Comment by @heiner on 2025-05-01 08:07_

> Can you try adding crates/my-package to src? Ruff can't find your package because it only searches for packages in your root (and src folder) but there's no my-package in that folder.

Note that I use

```
[tool.ruff]
src = [
    ".",
    "crates/*",
]
```

> We may want to create a separate issue.

Happy to create that. I assumed it was the underscore that mislead it, but renaming `crates/my-package` to `crates/my_package` does not change its behavior.

---

_Comment by @heiner on 2025-05-01 08:15_

New issue here: https://github.com/astral-sh/ruff/issues/17761

---

_Comment by @Jonathan-Landeed on 2025-09-23 07:20_

On the topic of better monorepo support, would classifying all [tool.uv.workspace].members as first-party make sense? The glob solution works, but it's a bit more cumbersome for me because not everything is at a single level and a double-glob was giving me problems (I think it was finding 3rd party imports in virtual envs).

---
