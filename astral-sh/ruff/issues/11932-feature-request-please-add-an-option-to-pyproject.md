```yaml
number: 11932
title: "[feature request] Please add an option to pyproject.toml to optionally use the external 'ruff' executable"
type: issue
state: closed
author: yurivict
labels:
  - question
assignees: []
created_at: 2024-06-19T02:02:36Z
updated_at: 2025-07-10T13:19:49Z
url: https://github.com/astral-sh/ruff/issues/11932
synced_at: 2026-01-12T15:54:51Z
```

# [feature request] Please add an option to pyproject.toml to optionally use the external 'ruff' executable

---

_@yurivict_

Our use case:
We have both devel/ruff and devel/py-ruff ports on FreeBSD.
The first builds and installs the standalone "ruff" executable.
The second one does the same plus it adds some Python files.

It would be beneficial if devel/py-ruff would be able to just depend on devel/ruff and it would only install Pythioon files using the "ruff" executable installed by devel/ruff.

This would solve 2 problems:
1. It would prevent the need to rebuild ruff multiple times.
2. It would eliminate the conflict between devel/ruff and devel/py-ruff because they both install the ruff executable.

Thank you,
Yuri

---

_Label `question` added by @MichaReiser on 2024-06-19 06:27_

---

_Comment by @MichaReiser on 2024-06-19 06:31_

I'm not sure I follow the use case exactly. Can you tell us more about what python files are part of `py-ruff` and why you have two separate packages? Why does `py-ruff` need to ship the binary if it's already part of the `ruff` package? 

Ruff (the binary) doesn't contain any logic to discover the Ruff binary to use. Calling `ruff check` simply uses the `ruff` binary on your path. That means, adding an option in the `pyproject.toml` won't help changing the ruff executable. 

I think this is different if you start ruff with the python module, although I'm not sure how common that is. But I don't think we want to read any configuration files as part of that module because that would add a significant start up delay because:

* It would need to discover the closest configuration, requiring tree traversal
* It needs to parse the configuration, something that's slow in Python


---

_Comment by @yurivict on 2024-06-19 07:02_

py311-ruff installs these files:
```
$ pkg info -l py311-ruff
py311-ruff-0.4.9:
        /usr/local/bin/ruff
        /usr/local/bin/ruff-3.11
        /usr/local/lib/python3.11/site-packages/ruff-0.4.9.dist-info/METADATA
        /usr/local/lib/python3.11/site-packages/ruff-0.4.9.dist-info/RECORD
        /usr/local/lib/python3.11/site-packages/ruff-0.4.9.dist-info/WHEEL
        /usr/local/lib/python3.11/site-packages/ruff-0.4.9.dist-info/license_files/LICENSE
        /usr/local/lib/python3.11/site-packages/ruff/__init__.py
        /usr/local/lib/python3.11/site-packages/ruff/__main__.py
        /usr/local/lib/python3.11/site-packages/ruff/__pycache__/__init__.cpython-311.opt-1.pyc
        /usr/local/lib/python3.11/site-packages/ruff/__pycache__/__init__.cpython-311.pyc
        /usr/local/lib/python3.11/site-packages/ruff/__pycache__/__main__.cpython-311.opt-1.pyc
        /usr/local/lib/python3.11/site-packages/ruff/__pycache__/__main__.cpython-311.pyc
```

ruff installs these files:
```
$ pkg info -l ruff
ruff-0.4.9:
        /usr/local/bin/ruff
        /usr/local/bin/ruff_dev
        /usr/local/bin/ruff_python_formatter
```

We need for py311-ruff to use binaries installed by ruff.


---

_Comment by @MichaReiser on 2024-06-19 07:15_

Yeah, I'm not familiar enough with linux packaging to give good advice here, but I feel like asking users to specify the ruff path in the pyproject toml isn't a good solution to solve a packaging problem. It's just pushing the problem on them.

I think either

* Simply assume in the `py311-ruff` that `ruff` is on the path and use that version?
* Inject the path (or you know the path?) to the ruff executable in `py311-ruff`
* Install the ruff binary locally in `py311-ruff` instead of globally. How do you handle other python packages with executables like black, mypy, pyright etc?

---

_Comment by @yurivict on 2024-06-19 07:33_

> [...] I feel like asking users to specify the ruff path [...]

No. This isn't what I am asking.
I am asking to have a build-time option in pyproject.toml to use the externally installed ruff binary.
Users would never use it.
Only the packagers would.

---

_Comment by @MichaReiser on 2024-06-19 08:00_

Would you mind explaining in more detail? I don't think I understand what you're asking for. Like what would you Ruff want to do? Where should that path be used? 

Regardless. Have you checked how other linux distribution handle this problem or how this is solved for other python packages with executables?

---

_Comment by @yurivict on 2024-06-19 08:37_

> Would you mind explaining in more detail?

I would like to have a build-time option in pyproject.toml that would not build the ruff binary, and instead use the preinstalled binary.

> Regardless. Have you checked how other linux distribution handle this problem or how this is solved for other python packages with executables?

They have conflicts because they don't care. But I would like to make it right.

---

_Comment by @MichaReiser on 2024-06-19 08:45_

> I would like to have a build-time option in pyproject.toml that would not build the ruff binary, and instead use the preinstalled binary.

Is my assumption correct that you're building ruff from source with maturin? How would you imagine the workflow. would you patch the `pyproject.toml` from the source file before calling maturin?

I think my preferred solution is that you write a tailored build script that simply doesn't call maturin (which calls `cargo build`) and copy move the python files you need. Overall, this seems a packaging problem to me



---

_Comment by @MichaReiser on 2024-06-21 06:09_

I'll close this as it's unclear to me how this is a Ruff and not a packaging issue and if it's a Ruff issue, how this should work in detail. 

---

_Closed by @MichaReiser on 2024-06-21 06:09_

---

_Comment by @saper on 2025-07-10 13:19_

I stumbled into this today and I think we could somehow make it work. Fortunately, the tiny Python binding is [small script](https://github.com/astral-sh/ruff/blob/main/python/ruff/__main__.py) whose main purpose is to find the `ruff` binary.

Unlike other Python libraries written in Rust, the binding is very light here - the small script could have been released or packaged as a separate thing, which just requires the `ruff` binary to work.

The advantage for what the OP wanted is the following:

> `ruff` binary does not need to build every time a Python interpreter version is added/updated

(I was just setting up dev environment for a downstream project and ruff rust binary had to be needlessly rebuilt for every virtual environment being deployed, with {n} Python versions to be installed).

I think we can experiment outside of this repo first, and, if there is something  that this project here could do (like additional ways to find the binary or providing additional metadata), we shall come back here and reopen this with a proposed solution.


---
