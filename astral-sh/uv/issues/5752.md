```yaml
number: 5752
title: "`uv add` adds a dependency and then declares it invalid"
type: issue
state: closed
author: dimbleby
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-03T15:53:12Z
updated_at: 2024-08-03T17:35:55Z
url: https://github.com/astral-sh/uv/issues/5752
synced_at: 2026-01-10T04:53:49Z
```

# `uv add` adds a dependency and then declares it invalid

---

_Issue opened by @dimbleby on 2024-08-03 15:53_

```shell
$ uv init .
warning: `uv init` is experimental and may change without warning
Initialized project `foo` at `/home/dch/foo`      

$ uv add --extra-index-url="https://download.pytorch.org/whl/cpu" -- torch
warning: `uv add` is experimental and may change without warning
Using Python 3.10.12 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Resolved 10 packages in 2.50s
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: foo @ file:///home/dch/foo
  Caused by: Failed to build: `foo @ file:///home/dch/foo`
  Caused by: Build backend failed to build wheel through `build_editable()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/home/dch/.cache/uv/builds-v0/.tmpqaB24C/lib/python3.10/site-packages/hatchling/build.py", line 83, in build_editable
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
  File "/home/dch/.cache/uv/builds-v0/.tmpqaB24C/lib/python3.10/site-packages/hatchling/builders/plugin/interface.py", line 90, in build
    self.metadata.validate_fields()
  File "/home/dch/.cache/uv/builds-v0/.tmpqaB24C/lib/python3.10/site-packages/hatchling/metadata/core.py", line 261, in validate_fields
    self.core.validate_fields()
  File "/home/dch/.cache/uv/builds-v0/.tmpqaB24C/lib/python3.10/site-packages/hatchling/metadata/core.py", line 1353, in validate_fields
    getattr(self, attribute)
  File "/home/dch/.cache/uv/builds-v0/.tmpqaB24C/lib/python3.10/site-packages/hatchling/metadata/core.py", line 1224, in dependencies
    self._dependencies = list(self.dependencies_complex)
  File "/home/dch/.cache/uv/builds-v0/.tmpqaB24C/lib/python3.10/site-packages/hatchling/metadata/core.py", line 1203, in dependencies_complex
    raise ValueError(message) from None
ValueError: Dependency #1 of field `project.dependencies` is invalid: Expected end or semicolon (after version specifier)
    torch>=2.4.0+cpu
         ~~~~~~~^

$ uv version
uv 0.2.33
```

`torch>=2.4.0+cpu` was introduced by `uv` itself

---

_Comment by @dimbleby on 2024-08-03 15:56_

I guess strictly speaking it is hatchling declaring that the dependency is invalid, but it is right: per [PEP440](https://peps.python.org/pep-0440/#inclusive-ordered-comparison) "Local version identifiers are NOT permitted in this version specifier."

---

_Comment by @charliermarsh on 2024-08-03 15:59_

Thanks.

---

_Label `bug` added by @charliermarsh on 2024-08-03 15:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-03 15:59_

---

_Label `preview` added by @charliermarsh on 2024-08-03 15:59_

---

_Closed by @charliermarsh on 2024-08-03 17:35_

---

_Closed by @charliermarsh on 2024-08-03 17:35_

---
