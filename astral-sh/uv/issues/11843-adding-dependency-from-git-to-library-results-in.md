```yaml
number: 11843
title: Adding dependency from git to library results in error when installing with pip
type: issue
state: closed
author: yalap13
labels:
  - question
assignees: []
created_at: 2025-02-27T23:22:23Z
updated_at: 2025-05-14T20:14:15Z
url: https://github.com/astral-sh/uv/issues/11843
synced_at: 2026-01-12T16:00:47Z
```

# Adding dependency from git to library results in error when installing with pip

---

_@yalap13_

### Question

I am working on a library dependent on a library only available from the Github repository. Since the source of the dependency is in `[tool.uv.sources]`, pip is unable to install the library as it tries to find a distibution on PyPI.

I could modify the pyproject.toml to include the source in the `project.dependencies` section as introduced in PEP633 but is there a way for uv to automatically add the source in the `project.dependencies` section or is there something else I have overlooked?

I am adding the dependency with `uv add git+url`.

Thanks in advance!

### Platform

Linux 6.12.15-200.fc41.x86_64 x86_64 GNU/Linux

### Version

uv 0.6.3

---

_Label `question` added by @yalap13 on 2025-02-27 23:22_

---

_Comment by @zanieb on 2025-02-27 23:26_

You can use `--raw-sources` to avoid transforming it to a `tool.uv.sources` entry.

---

_Comment by @yalap13 on 2025-02-28 00:03_

This works as long as I manually add the following to the `pyproject.toml`:
```
[tool.hatch.metadata]
allow-direct-references = true
```

Is there a way to configure the project so that the default behaviour is to be compatible with a pip installation?

---

_Closed by @yalap13 on 2025-05-14 20:14_

---
