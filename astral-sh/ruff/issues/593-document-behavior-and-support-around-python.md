```yaml
number: 593
title: Document behavior and support around python version
type: issue
state: closed
author: vikigenius
labels:
  - documentation
assignees: []
created_at: 2022-11-05T00:07:07Z
updated_at: 2022-11-28T04:50:25Z
url: https://github.com/astral-sh/ruff/issues/593
synced_at: 2026-01-12T15:54:40Z
```

# Document behavior and support around python version

---

_@vikigenius_

Looking at the README it is not exactly clear how `ruff` supports different python versions.

I saw the `target_version` option that talks about minimum supported python. But how low can we go? Does it support python 2 ?

Additionally is it supported to run ruff using `python -m ruff` to ensure that the correct version of ruff is run in the presence of global installations and multiple virtual environments?

---

_Label `documentation` added by @charliermarsh on 2022-11-05 03:14_

---

_Comment by @vikigenius on 2022-11-07 19:07_

@charliermarsh upon further testing I have learned that ruff does not actually support calling it as a module

For eg: `python -m flake8 <src>` works but `python -m ruff <src>` would complain about `no module named ruff`

Do you plan on supporting `python -m ruff` usage? I can create a separate issue for that.

All it needs is for there to actually be a `ruff` module that is the main entry point. For example flake8 has a top level `flake8` package and the following file https://github.com/PyCQA/flake8/blob/main/src/flake8/__main__.py that allows `python -m flake8` usage.

---

_Comment by @vikigenius on 2022-11-08 05:36_

Actually never mind I just realized that ruff is installed as just an `executable` and not a python module, thus there is no way to support `python -m`, still the concerns about python version etc. hold and should be documented.

---

_Comment by @charliermarsh on 2022-11-08 14:30_

Sorry for the delay here. Yeah, right now we're just shipping the binary and not an actual Python module. We could change this! It's totally possible for us to ship a Python module (in fact, I want to ship a Python API for the Ruff API too), it just adds a bit of complication so I've been putting it off.


---

_Comment by @charliermarsh on 2022-11-08 14:30_

Let's create a separate issue for `python -m ruff`, and use this for documenting target version support.


---

_Renamed from "Document behavior and support around python version and python -m option" to "Document behavior and support around python version" by @charliermarsh on 2022-11-08 14:36_

---

_Comment by @squiddy on 2022-11-09 05:45_

> it just adds a bit of complication so I've been putting it off.

Could you please elaborate on that? I've done some reading and doing #658 and #659 seems doable, maturin doesn't yet support building one wheel with both a binary and a library:

https://github.com/PyO3/maturin/issues/368



---

_Comment by @messense on 2022-11-09 09:03_

You don't need to ship both a binary and a library to support `python -m ruff`, you can add a `__main__.py` and `execv` the ruff executable.

For example, https://github.com/PyO3/maturin/blob/main/maturin/__main__.py

---

_Comment by @charliermarsh on 2022-11-09 14:47_

@squiddy - Interesting, I think I'd just assumed it was possible to build a single wheel for both a binary and a library. That's good to know. It seems from the issue that it may be possible to do at some point? I believe Maturin now supports shipping multiple _binaries_ in the same wheel.

---

_Comment by @charliermarsh on 2022-11-09 14:49_

@messense - Naive question, but in that scheme, can users still execute the Ruff executable directly? Or does invocation always go through Python?

In other words: would `ruff` still work, or would users _have_ to use `python -m ruff`? If the former, would calling `ruff` start a Python interpreter to call through to the Ruff executable? Or would it call the executable directly?



---

_Comment by @messense on 2022-11-09 14:54_

`ruff` still works as long as Python `scripts` directory is in `PATH`, it's same as current setup, no Python interpreter is involved when invoking `ruff` directly.

---

_Comment by @tekumara on 2022-11-21 00:38_

IIUC `target_version` is mostly used by the pyupgrade checks.

---

_Closed by @charliermarsh on 2022-11-28 04:50_

---
