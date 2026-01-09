---
number: 12588
title: dependencies are assumed to be the same among wheels
type: issue
state: closed
author: njzjz
labels:
  - question
assignees: []
created_at: 2025-03-31T15:38:43Z
updated_at: 2025-04-01T12:56:04Z
url: https://github.com/astral-sh/uv/issues/12588
synced_at: 2026-01-07T13:12:18-06:00
---

# dependencies are assumed to be the same among wheels

---

_Issue opened by @njzjz on 2025-03-31 15:38_

### Summary

In a Linux environment, run:

```sh
mkdir test-tf
cd test-tf
uv init
uv add tensorflow-cpu==2.19.0
```

The following error is triggered:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of tensorflow-intel{sys_platform == 'win32'}==2.19.0 and tensorflow-cpu==2.19.0 depends on
      tensorflow-intel{sys_platform == 'win32'}==2.19.0, we can conclude that tensorflow-cpu==2.19.0 cannot be used.
      And because your project depends on tensorflow-cpu==2.19.0, we can conclude that your project's requirements are
      unsatisfiable.
```

However, in a Windows environment, there is no error after running `uv pip install tensorflow-cpu`.

I checked the metadata of wheels and found that Linux wheels and Windows wheels have different dependencies:

- In [the Linux wheel](https://pypi-browser.org/package/tensorflow-cpu/tensorflow_cpu-2.19.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl), there is `tensorflow-intel ==2.19.0 ; platform_system == "Windows"` dependency.
- However, in the [Windows wheel](https://pypi-browser.org/package/tensorflow-cpu/tensorflow_cpu-2.19.0-cp310-cp310-win_amd64.whl), there is no `tensorflow-intel` dependency.

Thus, with `pip` or `uv pip`, there is no issue. However, it seems that `uv add` or `uv sync` assumes that all wheels have the same dependency, and thus cause the error.



### Platform

Ubuntu 24.04

### Version

 0.6.10

### Python version

3.12.2

---

_Label `bug` added by @njzjz on 2025-03-31 15:38_

---

_Referenced in [deepmodeling/deepmd-kit#4679](../../deepmodeling/deepmd-kit/issues/4679.md) on 2025-03-31 15:40_

---

_Comment by @charliermarsh on 2025-03-31 15:42_

Unfortunately this is a hard requirement of uv (and Poetry too), and likely won't change. Likely the best you can do here is define static dependency metadata for TensorFlow: https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata

---

_Label `bug` removed by @charliermarsh on 2025-04-01 12:56_

---

_Label `question` added by @charliermarsh on 2025-04-01 12:56_

---

_Closed by @charliermarsh on 2025-04-01 12:56_

---

_Referenced in [tensorflow/tensorflow#94854](../../tensorflow/tensorflow/pulls/94854.md) on 2025-06-05 05:39_

---
