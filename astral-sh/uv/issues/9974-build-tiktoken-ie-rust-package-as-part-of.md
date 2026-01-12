```yaml
number: 9974
title: Build tiktoken (ie. Rust package) as part of workspace
type: issue
state: closed
author: foges
labels:
  - question
assignees: []
created_at: 2024-12-17T16:26:44Z
updated_at: 2025-01-28T15:47:12Z
url: https://github.com/astral-sh/uv/issues/9974
synced_at: 2026-01-12T16:00:03Z
```

# Build tiktoken (ie. Rust package) as part of workspace

---

_@foges_

I'm trying to add the [tiktoken](https://github.com/openai/tiktoken) source code as a package in my `uv` workspace. It's available through pip, but I need to make some local modifications. The core of `tiktoken` is written in Rust and has a `setup.py` compile step. Things I've tried:

- Adding it to my `uv` workspace and importing it directly
  - This doesn't build the package so fails to find `from tiktoken import _tiktoken`
- Building it using either `uv pip install .` (in the tiktoken directory), `uv run python setup.py install`
  - I then just get the error that there's a circular import due to the `from tiktoken import _tiktoken` in the tiktoken module.
- `uv build && uv pip install .`
  - Building works and it adds it to `.venv/lib`, but as soon as I run anything in other workspace directories, it overrides the `.venv/lib` package with a workspace reference.
- Leaving it out of the workspace and just building with `uv run python setup.py install`
  - It now doesn't get built to `.venv/lib`.

My directory structure is
```
pyproject.toml        <-- `members = ["packages/*"]` and `package = false` in here. 
packages/
  mylib/
    mylib/            <-- Trying to import tiktoken from here
    pyproject.toml
  tiktoken/           <-- Copied from the tiktoken github
    tiktoken/
    pyproject.toml
```

One thing to note is that tiktoken uses a different build backend `"setuptools.build_meta"`, instead of `"hatchling.build"`, which seems to cause some issues in terms of package discoverability. I'm not sure it's easy to get around that due to `"setuptools-rust>=1.5.2"`.

At this point I'm out of ideas. Any suggestions are greatly appreciated :)

---

_Comment by @foges on 2024-12-17 19:27_

Update: I found a somewhat useable solution using `uv run python setup.py build_ext --inplace && uv pip install .`, which does allow me to keep tiktoken as part of the workspace. It doesn't seem ideal, so still curious if there are better solutions.

Update 2: Actually this doesn't work after all. As soon as I run `uv sync` or any other command like `uv run pytest` it again reverts to using the workspace dependency, rather than the installed one.

---

_Comment by @foges on 2024-12-19 19:09_

My latest is running this command `cd packages/tiktoken && uv venv --python 3.11 && source .venv/bin/activate && uv run python setup.py build_ext --inplace && deactivate && cd ../../`. I have `tiktoken = { path = "../tiktoken" }` in `mylib/pyproject.toml` although it uses one that gets installed in the top level `.venv/lib`

This all feels very hacky. Am I doing something wrong, or is `uv` not really intended for this kind of use-case?

---

_Comment by @zanieb on 2025-01-06 20:46_

What's in your `pyproject.toml`?

---

_Label `question` added by @zanieb on 2025-01-06 20:46_

---

_Comment by @foges on 2025-01-28 14:21_

Hey, sorry I only just saw this. 

My top level `pyproject.toml` is

```
[project]
name = "base"
version = "0.1.0"
requires-python = "~=3.10"
dependencies = []

[tool.uv]
dev-dependencies = ["mypy>=1.13.0", "pyright>=1.1.389", "pytest>=8.3.3"]
package = false

[tool.uv.workspace]
members = ["packages/*"]
exclude = ["packages/tiktoken"]  # Explicitly excluded it here since I couldn't get it to work
```

I guess the key parts of the `pyproject.toml` in tiktoken is:

```
[build-system]
build-backend = "setuptools.build_meta"
requires = ["setuptools>=62.4", "wheel", "setuptools-rust>=1.5.2"]

[tool.cibuildwheel]
build-frontend = "build"
build-verbosity = 1

linux.before-all = "curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y"
linux.environment = { PATH = "$PATH:$HOME/.cargo/bin" }
macos.before-all = "rustup target add aarch64-apple-darwin x86_64-apple-darwin"

macos.archs = ["x86_64", "arm64"]

[[tool.cibuildwheel.overrides]]
select = "*linux_aarch64"
```

---

_Comment by @foges on 2025-01-28 15:47_

Managed to get it to work by switching to maturin from setuptools_rust. I guess stepping away from it for a couple of weeks helps. Here are the changes to the tiktoken pyproject.toml:

```
[build-system]
requires = ["maturin"]
build-backend = "maturin"

[tool.maturin]
features = ["pyo3/extension-module"]
module-name = "tiktoken._tiktoken"
python-packages = ["tiktoken", "tiktoken_ext"]
```

---

_Closed by @foges on 2025-01-28 15:47_

---
