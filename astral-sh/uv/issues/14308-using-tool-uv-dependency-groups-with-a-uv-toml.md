---
number: 14308
title: "Using `tool.uv.dependency-groups` with a `uv.toml`"
type: issue
state: closed
author: mikeweltevrede
labels:
  - question
assignees: []
created_at: 2025-06-27T09:56:04Z
updated_at: 2025-07-20T22:55:02Z
url: https://github.com/astral-sh/uv/issues/14308
synced_at: 2026-01-10T01:25:43Z
---

# Using `tool.uv.dependency-groups` with a `uv.toml`

---

_Issue opened by @mikeweltevrede on 2025-06-27 09:56_

### Summary

When using the below `pyproject.toml`, running `uv sync --all-groups` fails (as expected) due to
> Because the requested Python version (>=3.9, <4.0) does not satisfy Python>=3.11 and sphinx==8.2.3 depends on Python>=3.11, we can conclude that sphinx==8.2.3 cannot be used. And because test-dependency-groups:docs depends on sphinx==8.2.3 and your project requires test-dependency-groups:docs, we can conclude that your project's requirements are unsatisfiable.

```toml
# pyproject.toml
[project]
name = "test-dependency-groups"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "<4.0,>=3.9"
dependencies = [
    "requests==2.27.1; python_version == '3.9'",
    "requests==2.28.1; python_version == '3.10'",
    "requests==2.31.0; python_version == '3.11'",
    "requests==2.32.2; python_version == '3.12'",
    "requests; python_version > '3.12'",
]

[dependency-groups]
docs = [
    "sphinx==8.2.3",
]
```

I wanted to solve this by using `tool.uv.dependency-groups` as introduced in #13735. Indeed it works if I add it to the `pyproject.toml` like so, `uv` is indeed able to compile:

```toml
# pyproject.toml
[tool.uv.dependency-groups]
docs = {requires-python = ">=3.11"}
```

However, there are two cases that do not work as expected:

# Error case 1: including `tool.uv.dependency-groups` in `pyproject.toml` while `uv.toml` exists
We have a `uv.toml` file for separation of concerns. When we include `tool.uv.dependency-groups` in `pyproject.toml` as the docs say and create an empty `uv.toml` file, and sync:

> warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The `[tool.uv]` section will be ignored in favor of the `uv.toml` file.

This still compiles successfully even though the `tool.uv.dependency-groups` section is not there. The warning message seems to indicate it would be ignored though, so I would **expect this to fail** like the first `pyproject.toml` **without** `[tool.uv.dependency-groups]`.

# Error case 2: including `dependency-groups` in `uv.toml`
We have a `uv.toml` file for separation of concerns. We include `dependency-groups` in `uv.toml` like so (and leave `pyproject.toml` as the base version):

```toml
# uv.toml
[dependency-groups]
docs = {requires-python = ">=3.11"}
```

I would **expect this to succeed** like the first `pyproject.toml` **with** `[tool.uv.dependency-groups]`. However, it fails with the same error message I mentioned first.

# Conclusion
1. The warning raised when including `tool.uv.dependency-groups` in `pyproject.toml` while `uv.toml` exists does not actually seem to be correct.
2. The error raised when including `dependency-groups` in `uv.toml` is not expected.

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.15 (4ed9c5791 2025-06-25)

### Python version

Python 3.12.10

---

_Label `bug` added by @mikeweltevrede on 2025-06-27 09:56_

---

_Comment by @konstin on 2025-06-27 10:15_

Why are you creating the `uv.toml` next to the `pyproject.toml`, what's in that file?

---

_Comment by @mikeweltevrede on 2025-06-27 11:02_

@konstin for separation of concerns, we like to use the `uv.toml` over the `pyproject.toml` just how we like to use `ruff.toml` and `.coveragerc` over `[tool.ruff]` and `[tool.coverage]` sections. Otherwise, it would blow up the `pyproject.toml`.

What we have in there, for example, is the `[[index]]` section to our corporate proxy.

---

_Comment by @zanieb on 2025-06-27 13:37_

I don't think we support reading this from a `uv.toml`, it's project-specific metadata.

I am a bit confused about our merging behavior here, especially that we warn we'll ignore it but use the configuration?

---

_Renamed from "tool.uv.dependency-groups" to "Using `tool.uv.dependency-groups` with a `uv.toml`" by @zanieb on 2025-06-27 13:37_

---

_Comment by @mikeweltevrede on 2025-06-27 13:55_

Thanks for updating the title @zanieb. Sorry, I forgot to make it a bit concrete.

Indeed then the support might not be there. I am fine with putting it in the `pyproject.toml` if that is currently the only supported way but then indeed the warning should not be emitted.

---

_Assigned to @Gankra by @Gankra on 2025-06-27 17:34_

---

_Referenced in [astral-sh/uv#14322](../../astral-sh/uv/pulls/14322.md) on 2025-06-27 18:26_

---

_Referenced in [astral-sh/uv#14325](../../astral-sh/uv/pulls/14325.md) on 2025-06-27 19:01_

---

_Comment by @Gankra on 2025-06-27 19:05_

* Case 1: #14325 
* Case 2: #14322 

---

_Comment by @mikeweltevrede on 2025-06-28 12:27_

@Gankra Thanks for the quick action :) I am not a Rust programmer, so the code in #14322 is not apparent to me that it would indeed allow specifying something in `pyproject.toml` while a `uv.toml` exists. Is that the case? Or would this mean that if you want to use `tool.uv.dependency-group` that you cannot use a `uv.toml`?

---

_Comment by @Gankra on 2025-07-02 12:36_

Yeah it's intentional that you have to use `tool.uv.dependency-group` here.

---

_Comment by @mikeweltevrede on 2025-07-04 11:36_

Alright, great. So it is still allowed to use both a `uv.toml` and `pyproject.toml` with `tool.uv.dependency-group`?

---

_Comment by @charliermarsh on 2025-07-11 01:52_

I think what you want is a `uv.toml`, and then a `pyproject.toml` with `[dependency-groups]` (instead of `tool.uv.dependency-group`, which is effectively deprecated in favor of the standards-complaint `[dependency-groups]`).

---

_Label `bug` removed by @charliermarsh on 2025-07-11 01:52_

---

_Label `question` added by @charliermarsh on 2025-07-11 01:52_

---

_Comment by @mikeweltevrede on 2025-07-11 16:00_

@charliermarsh I must say I am a bit surprised that a feature that was introduced only 1.5 months ago (#13735) is already deprecated right now. They also serve different purposes, where `tool.uv.dependency-groups` manages the metadata on the Python version while `[dependency-groups]` concerns libraries. But I am more than willing to move over if that would be better.

Could you share if there is something in the docs on how to combine

```toml
[dependency-groups]
docs = [
    "sphinx==8.2.3",
]
```

with

```toml
[tool.uv.dependency-groups]
docs = {requires-python = ">=3.11"}
```

One of them is a dictionary and the other is a list so it is not directly clear. Should `python` be defined like the rest of the libraries then like below, or something like that?

```toml
[dependency-groups]
docs = [
    "python>=3.11",
    "sphinx==8.2.3",
]
```

---

_Comment by @charliermarsh on 2025-07-11 16:06_

Sorry, ignore my prior comment. I was just wrong, I was misremembering `[tool.uv.dependency-groups]` as something else.


---

_Comment by @mikeweltevrede on 2025-07-11 16:08_

@charliermarsh Okay, no problem 😄 I think this is still to be marked as a bug then instead of a question?

---

_Comment by @Gankra on 2025-07-11 17:06_

Ah apologies -- what I should have said is that you must use a pyproject.toml for this setting. We consider all dependency-group and requires-python config to be "project" config and all "project" config is disallowed in uv.toml files.

---

_Comment by @Gankra on 2025-07-11 17:08_

However you can still have a uv.toml file while using this setting in your pyproject.toml -- the current warning is a lie.

---

_Closed by @charliermarsh on 2025-07-20 22:55_

---
