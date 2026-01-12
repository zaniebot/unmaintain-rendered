```yaml
number: 8813
title: uv picks old versions when using unbound dependencies
type: issue
state: closed
author: olzhasar-reef
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-11-04T18:42:29Z
updated_at: 2024-11-05T13:28:04Z
url: https://github.com/astral-sh/uv/issues/8813
synced_at: 2026-01-12T15:59:35Z
```

# uv picks old versions when using unbound dependencies

---

_@olzhasar-reef_

We are currently migrating our projects to `uv` from `pdm` and we are facing weird version resolution issues when specifying dependencies without lower bounds.
Consider the following minimal `pyproject.toml`:
```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "django-stubs[compatible-mypy]",
    "djangorestframework-stubs[compatible-mypy]",
]
```

```shell
uv lock
```

This produces a lock file with the `djangorestframework-stubs` version `1.4.0`, whereas the latest version is `3.15.1`.
```

[[package]]
name = "djangorestframework-stubs"
version = "1.4.0"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "coreapi" },
    { name = "django-stubs" },
    { name = "mypy" },
    { name = "requests" },
    { name = "typing-extensions" },
]
sdist = { url = "https://files.pythonhosted.org/packages/ff/b8/1c9cb32a9dcd3eb3f11cbb97465ad4bab17e8a54ebb3435661574d868908/djangorestframework-stubs-1.4.0.tar.gz", hash = "sha256:037f0582b1e6c79366b6a839da861474d59210c4bfa1d36291545cb6ede6a0da", size = 31984 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/c6/e5/814c1819452746193b99acb97c8b5205d8fe7b01f11957376b9448794ca0/djangorestframework_stubs-1.4.0-py3-none-any.whl", hash = "sha256:f6ed5fb19c12aa752288ddc6ad28d4ca7c81681ca7f28a19aba9064b2a69489c", size = 51155 },
]
```
 
 Interestingly, if we set the lower boundary the resolution works fine without any conflicts and the new version is being picked:
 
 ```toml
 [project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "django-stubs[compatible-mypy]",
    "djangorestframework-stubs[compatible-mypy]>=3",
]
 ```

```
[[package]]
name = "djangorestframework-stubs"
version = "3.15.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "django-stubs" },
    { name = "requests" },
    { name = "types-pyyaml" },
    { name = "types-requests" },
    { name = "typing-extensions" },
]
sdist = { url = "https://files.pythonhosted.org/packages/f3/00/40ee7702b69d32bd88240468a7cdc4b52aca42e14513d04ec8e16b5c1c59/djangorestframework_stubs-3.15.1.tar.gz", hash = "sha256:34539871895d66d382b6ae3655d9f95c1de7733cf50bc29097638d367ed3117d", size = 35195 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/a4/f4/463fe341a7fe4b79da9fef65327b8e8d696639eba592f9fced6b6b8593ff/djangorestframework_stubs-3.15.1-py3-none-any.whl", hash = "sha256:79dc9018f5d5fa420f9981eec9f1e820ecbd04719791f144419cdc6c5b8e29bd", size = 54399 },
]
```

```shell
uv version
uv 0.4.29 (85f9a0d0e 2024-10-30)
```
MacOS

---

_Comment by @zanieb on 2024-11-04 19:54_

Hi! I presume this is a case of https://github.com/astral-sh/uv/issues/8157 in which the resolver could be using better heuristics.

---

_Label `duplicate` added by @zanieb on 2024-11-04 19:54_

---

_Label `question` added by @zanieb on 2024-11-04 19:54_

---

_Closed by @charliermarsh on 2024-11-05 13:28_

---
