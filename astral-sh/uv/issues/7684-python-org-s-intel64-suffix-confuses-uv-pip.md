```yaml
number: 7684
title: "python.org's -intel64 suffix confuses uv pip"
type: issue
state: open
author: hynek
labels:
  - enhancement
  - uv python
assignees: []
created_at: 2024-09-25T11:51:19Z
updated_at: 2024-09-25T16:01:45Z
url: https://github.com/astral-sh/uv/issues/7684
synced_at: 2026-01-12T15:59:15Z
```

# python.org's -intel64 suffix confuses uv pip

---

_@hynek_

So this might sound a bit special, but I suspect there's more then the one of me.

Context is that I have an ARM-based Mac and I need to develop certain packages and applications under Intel emulation.

This is fairly easy with the official python.org builds that offer a `python3.12-intel64` which is always Intel, so creating a virtualenv solves all my problems.

Therefore, when using project mode, I use the following in Direnv:

```
export UV_PYTHON=python3.12-intel64

uv sync
```

Problems arise with `uv pip`. For one, if I'm in one of those projects and run `uv pip list`, I get what I suspect is the global output:


```
❯ uv pip list
Package Version
------- -------
pip     24.2
```

Of course, `env UV_PYTHON=python3.12 uv pip list` or `env UV_PYTHON=.venv uv pip list` work

Trying to install something (because, for example I want some debugger that isn't part of the project) fails rather opaquely:

```
❯ uv pip install ipdb
error: No virtual environment found for executable name `python3.12-intel64`; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```

Since `python3.12-intel64` is an official Python thing – would it be thinkable to add special support to uv for it?

---

```
❯ uv --version
uv 0.4.16 (e81ed8ec5 2024-09-24)
```

---

_Comment by @zanieb on 2024-09-25 14:09_

The `uv pip install ipdb` failure is that you didn't include `--system` to opt-in to mutating a non-virtual environment. We can improve the error message there.

Similarly, `uv pip list` will read from a system environment if asked to and I presume `$PY` expands to the full path to the system interpreter?


---

_Comment by @hynek on 2024-09-25 14:22_

> The `uv pip install ipdb` failure is that you didn't include `--system` to opt-in to mutating a non-virtual environment. We can improve the error message there.

yeah but I also didn’t want to. That part is 100% correct. I wanted it to look at .venv but uv pip decided that it doesn’t match Python3.12-intel64 which it totally does. My working theory for this whole ticket is that it gets confused by the suffix. 


> Similarly, `uv pip list` will read from a system environment if asked to and I presume `$PY` expands to the full path to the system interpreter?

I’m super sorry for copy-pasta — I didn’t notice it and will edit it accordingly. The effective UV_PYTHON is `python3.12-intel64`. 



---

_Comment by @zanieb on 2024-09-25 14:25_

Are you activating your virtual environment? When you provide a Python executable name i.e. `python3.12-intel64` we _only_ look for it on the `PATH`. We could consider that a request for a general Python interpreter instead and do full discovery, but we don't have any infrastructure for filtering interpreters by their architecture right now. Are there other variants than `intel64`?

---

_Comment by @hynek on 2024-09-25 14:42_

Ah I can only speculate since I’m on a full train, but yes I do activate my virtualenvs and I don’t think they have an `python3.12-intel64` but that’s something you know better than me. :)

But yeah that was my whole suspicion and hope that since the -intel64 suffix is official and since I don’t think I can achieve the same thing with uv tooling (?), there might be the possibility to give it special treatment? Eg ignoring it when checking a venv until you add some kind of arch intelligence.

Or do you have any other suggestions how to achieve what I need? This can’t be such an exotic need? 

---

_Comment by @zanieb on 2024-09-25 14:59_

I think you want to avoid setting `UV_PYTHON` and pass it per-command instead. Then, when in your virtual environments, `uv pip` will work as expected.

Regardless, we should add first-class support for this in discovery. I just don't know when we'll get to it.

Related https://github.com/python/cpython/issues/88175

---

_Label `enhancement` added by @zanieb on 2024-09-25 14:59_

---

_Label `uv python` added by @zanieb on 2024-09-25 14:59_

---

_Comment by @hynek on 2024-09-25 16:01_

> I think you want to avoid setting `UV_PYTHON` and pass it per-command instead. Then, when in your virtual environments, `uv pip` will work as expected.

Fair enough. I'd prefer to keep doing it because it works flawlessly with `uv sync` / `uv lock` and I don't have to pass it in two places (.envrc for Direnv `uv sync` and Justfile for recreating the venv).

But I can wait and live with workarounds for now – thanks! :)  I also wanted to make sure that it doesn't break for sync/lock at some point. ;)

---
