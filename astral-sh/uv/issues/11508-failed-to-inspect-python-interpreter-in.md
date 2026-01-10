---
number: 11508
title: Failed to inspect Python interpreter in conjunction with sitecustomize.py
type: issue
state: open
author: rkoschmitzky
labels:
  - bug
  - needs-decision
assignees: []
created_at: 2025-02-14T13:22:13Z
updated_at: 2025-06-18T17:07:22Z
url: https://github.com/astral-sh/uv/issues/11508
synced_at: 2026-01-10T01:25:06Z
---

# Failed to inspect Python interpreter in conjunction with sitecustomize.py

---

_Issue opened by @rkoschmitzky on 2025-02-14 13:22_

### Summary

For some internal tooling we make use of a `sitecustomize.py` that invokes the discovery and execution of "startup" files. This execution might log to STDOUT when it executes those files. This becomes a problem with uv when it tries to check for a valid virtualenv for certain operations.

As the `sitecustomize.py` is executed as soon as the python interpreter starts, this seems to confuse uv when output appears in STDOUT. 

_(from activated virtualenv)_
![Image](https://github.com/user-attachments/assets/539c3b8e-9c81-4063-a741-3cf483f641be)

_(example with uv)_
![Image](https://github.com/user-attachments/assets/de10e75b-0954-4c1c-8d82-92c76ab6b3bb)

### Steps To Reproduce

1. create a directory and cd into it
2. call `uv venv`
3. create a _sitecustomize.py_ file that just holds `print("hello world")` and move it into corresponding `site-packages` directory of the _.venv_
4. install some package `uv pip install <some_package>`

### Platform

Windows 10

### Version

uv 0.5.31

### Python version

Python 3.10.11

---

_Label `bug` added by @rkoschmitzky on 2025-02-14 13:22_

---

_Comment by @charliermarsh on 2025-02-14 18:11_

Hmm... We'll need to decide whether to support this. I guess we'd have to, like, write to a file, then read the file (instead of using stdout)?

---

_Label `needs-decision` added by @charliermarsh on 2025-02-14 18:12_

---

_Comment by @zanieb on 2025-02-14 18:20_

We could also use a semi-unique marker like `****** BEGIN UV QUERY BLOB ******`

---

_Comment by @charliermarsh on 2025-02-16 01:36_

Or we could just always take the last line...?

---

_Comment by @rkoschmitzky on 2025-02-17 10:44_

Without having any knowledge about how uv is invoking python to retrieve interpreter details, but maybe it would be the easiest to circumvent this by calling python with the following flag? 

> -S     : don't imply 'import site' on initialization


---

_Comment by @konstin on 2025-02-17 11:31_

Using only the last line could work, that would be similar to what we're doing for PEP 517. (I haven't check `-S` yet)

---

_Comment by @rkoschmitzky on 2025-03-18 16:25_

> Using only the last line could work, that would be similar to what we're doing for PEP 517. (I haven't check `-S` yet)

@konstin Did you get the chance to check the `-S` flag? 

From what it looks like, [this runs in isolation](https://github.com/astral-sh/uv/blob/main/crates/uv-python/src/interpreter.rs#L748) and feels like a pretty easy and safe change to do.

Happy to gain more insight from you guys. Thanks :)

---

_Comment by @konstin on 2025-04-15 12:52_

We unfortunately must run without `-S` as `-S` removes directories from `sys.path` we need to read.

---

_Comment by @rkoschmitzky on 2025-06-18 15:58_

Since yesterday we run into the same issue caused by `azure-monitor-opentelemetry-exporter` due to the release from `1.0.0b37` to `1.0.0b38`.

![Image](https://github.com/user-attachments/assets/fc00fa58-a730-41e3-bb00-99a4cf729d0b)

---

_Comment by @charliermarsh on 2025-06-18 16:58_

@konstin -- Maybe we should just vendor everything into a single file and run with `-S`?

---

_Comment by @konstin on 2025-06-18 17:07_

iirc from https://github.com/astral-sh/uv/pull/11670, there are directories that are not ours that go away when using `-S`, but I'd have to check to see which platform it was that failed.

---
