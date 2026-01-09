---
number: 14617
title: "Weird behavior with `uv pip install -r requirements.txt` when using relative directories"
type: issue
state: closed
author: Panacea729
labels:
  - question
assignees: []
created_at: 2025-07-14T22:38:34Z
updated_at: 2025-07-15T21:47:47Z
url: https://github.com/astral-sh/uv/issues/14617
synced_at: 2026-01-07T13:12:18-06:00
---

# Weird behavior with `uv pip install -r requirements.txt` when using relative directories

---

_Issue opened by @Panacea729 on 2025-07-14 22:38_

### Question

I have the following directory structure
```
.
├── packages
│   ├── my-pkg
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── my_pkg
│   │           └── __init__.py
│   └── my-pkg-lib
│       ├── pyproject.toml
│       └── src
│           └── my_pkg_lib
│               └── __init__.py
├── pyproject.toml
├── requirements.txt
├── src
│   └── root_pkg
│       └── __init__.py
└── uv.lock
```
and the following pyproject.tomls
```
[project]
name = "root-pkg"
version = "1.0"
requires-python = ">=3.10"
dependencies = [
        "my-pkg"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.workspace]
members = [
    "packages/my-pkg",
    "packages/my-pkg-lib",
]

[tool.uv.sources]
my-pkg = { workspace = true }
my-pkg-lib = { workspace = true }
```
```
[project]
name = "my-pkg"
version = "1.0"
requires-python = ">=3.10"
dependencies = [
        "my-pkg-lib"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
```
[project]
name = "my-pkg-lib"
version = "1.0"
requires-python = ">=3.10"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

I have a requirements.txt as follows:
```
.
./packages/my-pkg
    # via root-pkg
./packages/my-pkg-lib
    # via my-pkg
```
This is what gets generated from `uv export --no-editable`

So, now my question/issue. If I run `uv pip install -r requirements.txt --target my-deps`, my-pkg and my-pkg-lib are installed as editable, even though the requirements.txt does not indicate to install them as such.

```
└─$ ls my-deps/
my_pkg-1.0.dist-info  my_pkg_lib-1.0.dist-info  _my_pkg_lib.pth  _my_pkg.pth  root_pkg  root_pkg-1.0.dist-info
```
Switching the order of the requirements doesn't matter:
```
./packages/my-pkg-lib
./packages/my-pkg
.
```

Another thing to note, is if I don't install the root-pkg, my-pkg gets installed as non-editable and my-pkg-lib gets installed as editable, so clearly it seems as though there's something up with how the dependencies are installed.

```
./packages/my-pkg-lib
./packages/my-pkg
```
```
└─$ ls my-deps/
my_pkg  my_pkg-1.0.dist-info  my_pkg_lib-1.0.dist-info  _my_pkg_lib.pth
```

Obviously if the requirements were listed with `-e` you would expect this to be the case, but without `-e` this is unintuitive to me. Is there some way to get around this? Someway to indicate that I don't want to install the lib package (or whatever other dependencies) as editable?

With `uv sync` there's `--no-editable` which guarantees that these workspace members won't be installed as editable, but there's no such thing for `uv pip install`, I imagine because it's expected to come from the requirements file.

Any help would be appreciated.

### Platform

Linux

### Version

uv 0.7.21


---

_Label `question` added by @Panacea729 on 2025-07-14 22:38_

---

_Renamed from "Weird behavior with `pip install -r requirements.txt` when using relative directories" to "Weird behavior with `uv pip install -r requirements.txt` when using relative directories" by @Panacea729 on 2025-07-14 22:52_

---

_Comment by @Panacea729 on 2025-07-14 23:02_

For reference when using `pip` and not `uv pip` the files are installed as uneditable:
```
 pip install -r requirements.txt --target my-deps-pip
```
```
ls my-deps-pip/
my_pkg  my_pkg-1.0.dist-info  my_pkg_lib  my_pkg_lib-1.0.dist-info  root_pkg  root_pkg-1.0.dist-info
```

---

_Comment by @zanieb on 2025-07-14 23:05_

I think this is the same as https://github.com/astral-sh/uv/issues/13087#issuecomment-2971492504

Does it reproduce with the `--no-deps` flag, e.g., `pip install -r requirements.txt --target my-deps-pip --no-deps`?


---

_Comment by @Panacea729 on 2025-07-14 23:07_

```
 1737  rm -rf my-deps-pip
 1738  pip install -r requirements.txt --target my-deps-pip --no-deps
 1739  ls my-deps-pip
 1740  history
```
```
└─$ ls my-deps-pip
my_pkg  my_pkg-1.0.dist-info  my_pkg_lib  my_pkg_lib-1.0.dist-info  root_pkg  root_pkg-1.0.dist-info
```

---

_Comment by @Panacea729 on 2025-07-14 23:09_

Ah, if you meant `uv pip install -r requirements.txt --target my-deps-pip --no-deps` that seemed to work. Thanks. Should've realized that.

---

_Closed by @Panacea729 on 2025-07-14 23:09_

---

_Comment by @Panacea729 on 2025-07-15 15:32_

Ah so the issue I run into with `--no-deps` in my actual project is that I actually DO want the deps. I want the deps from the `lib` packages. My lib packages have dependencies on external python packages from pypi.

The "workaround" for this is either use two requirements.txt files (one with --no-deps and one without, which seems as though it might run into dependency conflicts), or as I'm currently doing, use `uv venv` and `uv sync --no-editable --group MY_GROUP --no-dev` and manually parse that venv to grab the packages we want. The second way seems to be working well enough for now, but really, I want to avoid creating a bunch of different dependency groups within the pyproject.toml when I could be using `requirements.txt` files.

For reference the project I'm making essentially builds a group of plugins, and there are different combinations of plugins that are supported by the main program. I organize those different sets of plugins that work together into dependency-groups. I want all the plugins in the same workspace since developers will always be working on them all at the same time, and they all should share dependencies for simplicity since they can be combined in so many different ways.

The reason I tended toward using `uv pip install` was to use different sources than present in the pyproject.toml. In some of our CI builds, we want to point the project to use a package from a private repository, where other times we want to point it to use the project from the workspace (very weird scenario), but conditional sources like this isn't supported by `uv`. Seems like it's only able to differentiate sources based on the os/python version using dependency specifiers.

---

_Reopened by @Panacea729 on 2025-07-15 15:32_

---

_Comment by @Panacea729 on 2025-07-15 15:33_

I'd be nice if the `uv pip install` interface matched the same behavior as `pip install` here. (which is a separate issue from ading `no-editable` to `uv pip install` although they solve similar issues)

---

_Comment by @zanieb on 2025-07-15 16:54_

`uv export` should include all those transitive dependencies, you should always install with `--no-deps` from `requirements.txt` files to avoid pulling in unlocked dependencies? I'm a bit confused.

Does using `--no-sources` on the `uv pip install` invocation work?

---

_Comment by @Panacea729 on 2025-07-15 19:00_

You can ignore my specific use case, it's very unique. I would rather not include the transitive dependencies in the `requirements.txt` files if possible.

Essentially, I want to, without a uv project (due to lack of support for conditional sources), have a bunch of different `requirements.txt` files,
i.e. requirements-group1.txt, requirements-group2.txt, etc...

Some of these requirements.txt files can have relative paths to resolve their dependencies, i.e.
```
./packages/common-lib
./packages/plugin1
./packages/plugin2
plugin3 == 1.0.0
plugin4 == 1.0.0
./packages/plugin5
```
The packages can have transitive dependencies, i.e. common-lib can depend on "requests" or "cryptography", which are not listed in the requirements.txt

If, for example, plugin1 depends on `common-lib`, `common-lib` SHOULD be installed as non-editable since it is included in the requirements.txt file as non-editable.

However, with `uv`s current implementation `common-lib` is installed as editable when doing `uv pip install -r requirements-group1.txt`. Additionally, `uv pip install --no-deps` cannot be used, since transitive dependencies (outside of those included in the requirements file) should still be resolved.

This seems to go against the standards that `pip` sets up since `pip` would install `common-lib` as non-editable, as shown here https://github.com/astral-sh/uv/issues/14617#issuecomment-3071281240. It would help me if `uv pip install` followed `pip`s same logic.

Thank you for the help by the way, I really appreciate the dev team's responsiveness.

---

_Comment by @Panacea729 on 2025-07-15 21:40_

I'm closing this because `--no-sources` should work and I just messed up. I initially tried it and got an error and figured it didn't fix it, but realized that I was getting an error because we link to a hatch plugin in the workspace as part of the build-dependencies for the "lib" pkg. It was working without `--no-sources` since the root-workspace had `hatch-plugin` as part of the workspace (so the lib pkg would traverse up and grab it), but with `--no-sources` it now doesn't know how to find it. I'm not really sure of a good solution for this, but might just manually install it into the `venv` and then later uninstall it.



---

_Closed by @Panacea729 on 2025-07-15 21:40_

---

_Comment by @Panacea729 on 2025-07-15 21:40_

Once again, thanks for the help.

---

_Comment by @Panacea729 on 2025-07-15 21:47_

It would be helpful to have a `--no-sources-package` and a `--no-sources-build` or something like that to differentiate between allowing resolving for build dependencies vs package dependencies.

---

_Referenced in [astral-sh/uv#5631](../../astral-sh/uv/issues/5631.md) on 2025-07-22 21:37_

---

_Referenced in [astral-sh/uv#14963](../../astral-sh/uv/issues/14963.md) on 2025-07-29 23:24_

---

_Referenced in [astral-sh/uv#15297](../../astral-sh/uv/issues/15297.md) on 2025-08-15 15:45_

---

_Referenced in [aws/aws-cdk#35500](../../aws/aws-cdk/pulls/35500.md) on 2025-09-15 18:16_

---
