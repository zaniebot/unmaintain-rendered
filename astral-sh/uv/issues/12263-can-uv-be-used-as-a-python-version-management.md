```yaml
number: 12263
title: Can UV be used as a Python version management tool at the system layer?
type: issue
state: open
author: lushi516
labels:
  - question
assignees: []
created_at: 2025-03-18T08:44:39Z
updated_at: 2025-10-27T10:43:42Z
url: https://github.com/astral-sh/uv/issues/12263
synced_at: 2026-01-12T16:00:59Z
```

# Can UV be used as a Python version management tool at the system layer?

---

_@lushi516_

### Question

I have reviewed the documentation and understand that the current UV cannot be used as a system level Python version management tool because it does not support `pyenv global version` and pyenv shim like features.

UV can only be used for projects based on UV technology stack. Creating a virtual environment through UV script or UV can indirectly call Python, but it is not possible to directly call Python or switch Python versions at the system level. I think UV cannot replace Pyenv in non UV technology stack applications.



### Platform

window11/wsl ubuntu 20 LTS

### Version

uv 0.6.6

---

_Label `question` added by @lushi516 on 2025-03-18 08:44_

---

_Comment by @konstin on 2025-03-28 19:32_

We're tracking support for adding Python shims in https://github.com/astral-sh/uv/issues/6265, which should capture most of this.

---

_Comment by @Da1sypetals on 2025-10-24 15:24_

Will this feature eventually come into existence? I am considering waiting for it or using pyenv instead.

---

_Comment by @konstin on 2025-10-27 10:43_

We have a lot of Python management features, including Python symlinks to `~/.local/bin` and global pins with `.python_version`, e.g. [`uv python pin`](https://docs.astral.sh/uv/reference/cli/#uv-python-pin), all explained here: https://docs.astral.sh/uv/concepts/python-versions/.  pyenv has a very different approach to managing Python versions, which may not map exactly to uv's features.

---
