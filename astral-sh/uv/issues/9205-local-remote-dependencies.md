```yaml
number: 9205
title: Local + remote dependencies
type: issue
state: open
author: nunokaeru
labels: []
assignees: []
created_at: 2024-11-18T16:37:03Z
updated_at: 2024-11-22T02:25:35Z
url: https://github.com/astral-sh/uv/issues/9205
synced_at: 2026-01-12T15:59:44Z
```

# Local + remote dependencies

---

_@nunokaeru_

Let's say I have a repository that has two packages, one of them depends on the other but I want to both test that dependency with it's local version and with a remote "downgraded" version.

```
> project
     > src
          > package-1
          > package-2
```


```
package-1/pypoject.toml

[project]
version = "1.0.0"

dependencies = ["package-2==1.0.0"]


package-2/pyproject.toml
[project]
version = "1.1.0"

dependencies = []
```

When I run `uv sync` in package-1, the environment will be installed with the version 1.0.0 of package-2 even though that package has been updated. Is there a way to define both local and remote dependencies and choosing at runtime which one to use. This way it's possible to ensure ABI compability between package versions.

---

_Comment by @ReinforcedKnowledge on 2024-11-22 01:32_

Hi!

I think you can do this with the `--with` option of `uv run`, or `--with-requirements` if you have many dependencies.

Toy example for illustrative purposes:
```
.
├── package1
│   ├── README.md
│   ├── pyproject.toml
│   ├── src
│   │   └── package1
│   │       ├── __init__.py
│   │       └── hello.py
├── package2
│   ├── README.md
│   ├── pyproject.toml
│   └── src
│       └── package2
│           ├── __init__.py
│           └── pk2_hello.py
└── remote
    └── package2
        ├── README.md
        ├── pyproject.toml
        └── src
            └── package2
                ├── __init__.py
                └── pk2_hello.py
```

The "remote" version of `package2` runs with version `0.1.0`. Its `pk2_hello.py` contains the following code:
```python
def pk2_hello():
    print("Hello from 0.1.0")
```

The other `package2` runs with version `0.1.1` and contains the following code in its `pk2_hello.py`:
```python
def pk2_hello():
    print("Hello from 0.1.0")
```

In my `package1` I do `uv add ../remote/package2`, which makes the `pyproject.toml` like:
```toml
[project]
name = "package1"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "ReinforcedKnowledge", email = myemail@gmail.com }
]
requires-python = ">=3.13"
dependencies = [
    "package2",
]

[project.scripts]
package1 = "package1:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
package2 = { path = "../remote/package2" }
```

We can verify in `uv.lock` the correct version:
```
version = 1
requires-python = ">=3.13"

[[package]]
name = "package1"
version = "0.1.0"
source = { editable = "." }
dependencies = [
    { name = "package2" },
]

[package.metadata]
requires-dist = [{ name = "package2", directory = "../remote/package2" }]

[[package]]
name = "package2"
version = "0.1.0"
source = { directory = "../remote/package2" }
```

I add this code in `hello.py`:
```python
from package2.pk2_hello import pk2_hello


def pk1_hello():
    pk2_hello()


pk1_hello()
```

Doing `uv run src/package1/hello.py` echos "Hello from 0.1.0".

So `package1` runs wells with the downgraded version of `package2`.

Now do this `uv run --with ../package2 src/package1/hello.py`, you should see "Hello from 0.1.1"

This creates a new ephemeral virtual environment in which it layers the new dependencies on top of the existing ones.

From the manual (the bold part is not in the manual)

>       --with <WITH>
          Run with the given packages installed.
          
          When used in a project, these dependencies will be layered on top of
          the project environment in a separate, ephemeral environment. These
          dependencies are allowed to conflict with those specified by the
          project.

---
