```yaml
number: 11274
title: uv selects an unavailable onnxruntime version (1.20.1) despite it not existing on PyPI for the given platform
type: issue
state: closed
author: markovejnovic
labels:
  - bug
assignees: []
created_at: 2025-02-06T08:18:39Z
updated_at: 2025-02-06T19:52:39Z
url: https://github.com/astral-sh/uv/issues/11274
synced_at: 2026-01-12T16:00:32Z
```

# uv selects an unavailable onnxruntime version (1.20.1) despite it not existing on PyPI for the given platform

---

_@markovejnovic_

### Summary

#### Preface

_Note: I have observed the following issue with `poetry` too. Considering I believe `uv` shares this issue with `poetry`, I am opening a ticket with `uv` too. This ticket will resemble the aforementioned https://github.com/python-poetry/poetry/issues/10151_

`uv` appears to select an `onnxruntime` version which is incompatible with my system. With a very minimal `pyproject.toml`:

```toml
[project]
name = "bugs"
version = "0.1.0"
description = ""
authors = []
requires-python = ">=3.9,<4.0"
dependencies = [
    "onnxruntime",
]
```

`uv` selects the `1.20.1` `onnxruntime` package. This is, however, incompatible with my system, as evidenced by pip index and I would expect a `1.19.2`:
```
$ uv run pip index versions onnxruntime
WARNING: pip index is currently an experimental command. It may be removed/changed in a future release without prior warning.
onnxruntime (1.19.2)
Available versions: 1.19.2, 1.19.0, 1.18.1, 1.18.0, 1.17.3, 1.17.1, 1.17.0, 1.16.3, 1.16.2, 1.16.1, 1.16.0, 1.15.1, 1.15.0, 1.14.1, 1.14.0, 1.13.1, 1.12.1, 1.12.0, 1.11.1, 1.11.0, 1.10.0, 1.9.0, 1.8.1, 1.8.0, 1.7.0
```

I have attached the [uv.lock file](https://github.com/user-attachments/files/18686398/uv_lock.txt). Notice
```toml
[[package]]
name = "onnxruntime"
version = "1.20.1"
```

I've installed `uv` as per the recommended way via the install script.

### Platform

Rocky Linux 9.4 (Blue Onyx)

### Version

uv 0.5.29

### Python version

Python 3.9.18

---

_Label `bug` added by @markovejnovic on 2025-02-06 08:18_

---

_Comment by @charliermarsh on 2025-02-06 13:57_

You can solve this with:
```toml
[project]
name = "bugs"
version = "0.1.0"
description = ""
authors = []
requires-python = "==3.9.*"
dependencies = [
    "onnxruntime",
]
```

---

_Comment by @markovejnovic on 2025-02-06 17:34_

Perhaps, I'll give it a shot when I'm at my computer. That being said, it does feel wrong that uv picked a version of the package that is [incompatible](https://pypi.org/project/onnxruntime/#files) with the current working interpreter.

Also I would like to avoid pinning the python version in my project.

---

_Comment by @charliermarsh on 2025-02-06 17:36_

Of course, but it's inherently a fairly hard problem. The package doesn't publish source distributions and has limited platform support. It's _impossible_ to come up with a resolution that covers all possible platforms. This is a more narrow case, but that's why it isn't working here. We see a version that has support for at least one of the platforms you care about (i.e., platform support for Python 3.10 onward), and we choose that.

---

_Comment by @charliermarsh on 2025-02-06 17:49_

(I agree the experience here should be better; just trying to explain what's going on and why it's difficult. We also don't privilege the currently-running interpreter at all, which I think is correct -- we're solving generically, ignoring your platform, Python version, etc., but also contributes here.)

---

_Comment by @charliermarsh on 2025-02-06 17:57_

I kind of expected this to work:
```toml
[project]
name = "bugs"
version = "0.1.0"
description = ""
authors = []
requires-python = ">=3.9,<4.0"
dependencies = [
    "onnxruntime",
]

[tool.uv]
environments = [
    "python_version == '3.9'",
    "python_version > '3.9'",
]
```

Which is a nicer solution in that you don't have to change your supported Python range. But it's not working as expected, so looking into that.

---

_Comment by @markovejnovic on 2025-02-06 19:51_

Okay so I don't think this is a uv issue. after discussing with the maintainers of poetry, I've learned that poetry relies on the python-requires value in onnxruntime's project declaration rather than which packages are actually available for my python distribution.

uv likely does the same and that seems like a natural and good behavior.

I'm closing this ticket consequently. Please refer to the poetry ticket for the exact discussion.

---

_Closed by @markovejnovic on 2025-02-06 19:51_

---

_Comment by @markovejnovic on 2025-02-06 19:52_

@dimbleby has kindly opened a PR to amend the issue on onnxruntime's side https://github.com/microsoft/onnxruntime/pull/23604

---
