---
number: 11245
title: "`uv build` always adds `--compatibility off` to build args"
type: issue
state: closed
author: jonded94
labels:
  - question
assignees: []
created_at: 2025-02-05T14:10:28Z
updated_at: 2025-02-06T11:27:38Z
url: https://github.com/astral-sh/uv/issues/11245
synced_at: 2026-01-10T01:25:03Z
---

# `uv build` always adds `--compatibility off` to build args

---

_Issue opened by @jonded94 on 2025-02-05 14:10_

### Question

I have a `PyO3` based project which uses `maturn` as a build backend. I'd like to use `uv` as the build frontend (could also directly use `maturn` as well, but I want to interact solely with `uv´ on the CLI).

I can build a wheel with `uv` without a problem, I'm using `uv build --wheel` for that. I can also see that for example `--no-build-isolation` has an effect on what underlying `maturin` command is actually called:

```
$ uv build --wheel
Building wheel...
Running `maturin pep517 build-wheel -i /home/[...]/.cache/uv/builds-v0/.tmpix92WB/bin/python --compatibility off
```

vs 

```
$ uv build --wheel --no-build-isolation
Building wheel...
Running `maturin pep517 build-wheel -i /home/[path to project]/.venv/bin/python3 --compatibility off
```

But, there always is this `--compatibility off` setting which I can't get rid of for some reason. From the [maturin side](https://github.com/PyO3/maturin/blob/060e2108fde4a7066459a0c7bacfe810d6d46cfa/maturin/__init__.py#L94) it should *only* be set whenever I set no `compatiblity` or `manylinux` settings at all. But even if I set this, this doesn't work:

```
$ uv build --wheel -C "build-args=--zig" -C "build-args=--auditwheel=repair" -C "build-args=--compatibility=manylinux_2_24"
Building wheel...
Running `maturin pep517 build-wheel -i /home/[...]/.cache/uv/builds-v0/.tmpTfvC7x/bin/python --compatibility off --zig --auditwheel=repair --compatibility=manylinux_2_24`
🍹 Building a mixed python/rust project
🔗 Found pyo3 bindings with abi3 support for Python ≥ 3.10
🐍 Not using a specific python interpreter
💥 maturin failed
  Caused by: Expected only one platform tag but found 2
Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/home/[...]/.cache/uv/builds-v0/.tmpTfvC7x/bin/python', '--compatibility', 'off', '--zig', '--auditwheel=repair', '--compatibility=manylinux_2_24'] returned non-zero exit status 1
  × Failed to build `/home/[path to project]`
  ╰─▶ Build backend failed to build wheel through `build_wheel` (exit status: 1)
```

The error here is that *multiple* `--compatibility` flags were handed to the `maturin pep517` build command.

I'm assuming that `uv` adds the initial `--compatibility off` flag *somehow*, but I wasn't able to find through which mechanism. How can I bring `uv` to *not* add this flag?

### Platform

Arch Linux amd64

### Version

0.5.6

---

_Label `question` added by @jonded94 on 2025-02-05 14:10_

---

_Comment by @charliermarsh on 2025-02-05 14:26_

I don't believe uv does anything here. This must be added by maturin in some way? \cc @konstin 

---

_Comment by @konstin on 2025-02-06 11:27_

This is a default setting in maturin since maturin can't determine the right preference for a PEP 517 caller (and uv can't set this option since it has no api for it either). You can change the default with https://www.maturin.rs/config.html#maturin-options. If the maturin options don't work for you, please open an issue at https://github.com/PyO3/maturin.

---

_Closed by @konstin on 2025-02-06 11:27_

---
