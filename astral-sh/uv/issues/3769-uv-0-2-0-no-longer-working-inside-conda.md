```yaml
number: 3769
title: uv 0.2.0 no longer working inside conda environments
type: issue
state: closed
author: notatallshaw-gts
labels:
  - bug
assignees: []
created_at: 2024-05-22T20:16:19Z
updated_at: 2024-05-22T21:39:20Z
url: https://github.com/astral-sh/uv/issues/3769
synced_at: 2026-01-12T15:58:45Z
```

# uv 0.2.0 no longer working inside conda environments

---

_@notatallshaw-gts_

First install conda and activate base environment:

```
$ uv -V
uv 0.2.0

$ which python
~/miniforge3/bin/python

$ python -V
Python 3.10.6

$ uv pip install --dry-run db-dtypes==1.0.3
error: No Python default installation found in virtual environments

$ uv pip install --dry-run db-dtypes==1.0.3 -vvv
    0.001455s  INFO uv_interpreter::virtualenv Found active virtual environment (via CONDA_PREFIX) at: /home/STRIKETECH/dshaw/miniforge3
    0.001548s DEBUG uv_interpreter::discovery Ignoring Python interpreter at `/home/STRIKETECH/dshaw/miniforge3/bin/python`: system intepreter not allowed
error: No Python default installation found in virtual environments uv_requirements::specification::from_source source=db-dtypes==1.0.3
    0.001840s DEBUG uv_interpreter::discovery Searching for interpreter that fulfills Python @ default
    0.001883s DEBUG uv_interpreter::discovery Searching for interpreter that fulfills Python @ default
    0.001909s  INFO uv_interpreter::virtualenv Found active virtual environment (via CONDA_PREFIX) at: /home/STRIKETECH/dshaw/miniforge3
    0.002027s DEBUG uv_interpreter::discovery Ignoring Python interpreter at `/home/STRIKETECH/dshaw/miniforge3/bin/python`: system intepreter not allowed
error: No Python default installation found in virtual environments
```


But this is not my system interpreter.

---

_Comment by @zanieb on 2024-05-22 20:22_

Thanks this looks like a bug in our interpreter identification — `Interpreter::is_virtualenv` is returning false here so we treat it as a system installation.

Can you share the output of 

```
/home/STRIKETECH/dshaw/miniforge3/bin/python -c "import sys; print(sys.prefix); print(sys.base_prefix)"
```

---

_Label `bug` added by @zanieb on 2024-05-22 20:22_

---

_Comment by @charliermarsh on 2024-05-22 20:23_

It's possible that Conda environments don't adhere to that definition.

---

_Comment by @notatallshaw-gts on 2024-05-22 20:23_

```
$ /home/STRIKETECH/dshaw/miniforge3/bin/python -c "import sys; print(sys.prefix); print(sys.base_prefix)"
/home/STRIKETECH/dshaw/miniforge3
/home/STRIKETECH/dshaw/miniforge3
```

---

_Comment by @charliermarsh on 2024-05-22 20:23_

They're not actually virtual environments so I think that's reasonable by them.

---

_Comment by @zanieb on 2024-05-22 20:25_

Yeah that's fair. It's a little awkward for us when we're trying to ensure that you're working in a environment that's safe to mutate without opt-in.

---

_Comment by @notatallshaw-gts on 2024-05-22 20:26_

Yeah, conda environments act like system environments that the the user can create and throw away, you can create virtual environments from a conda environment to work around this, but it's a level of redundancy that is a waste for most use cases.

---

_Comment by @notatallshaw-gts on 2024-05-22 20:29_

> Yeah that's fair. It's a little awkward for us when we're trying to ensure that you're working in a environment that's safe to mutate without opt-in.

Btw, as the first step are you checking for the PEP 668 externally managed flag?

For any newish Linux distributions this is probably sufficient, older ones probably not 🙁.

---

_Comment by @zanieb on 2024-05-22 20:35_

Yeah we do check for that, but that's another level of opt-in (matching pip's interface).

I've got a fix in progress at #3771 

---

_Assigned to @zanieb by @zanieb on 2024-05-22 20:53_

---

_Closed by @zanieb on 2024-05-22 21:27_

---

_Comment by @zanieb on 2024-05-22 21:39_

Release on its way at #3774 

---
