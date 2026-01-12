```yaml
number: 11179
title: uv install from wheel does not use index specified in pyproject.toml
type: issue
state: closed
author: alksndrglk
labels:
  - question
assignees: []
created_at: 2025-02-03T09:46:20Z
updated_at: 2025-02-11T03:16:18Z
url: https://github.com/astral-sh/uv/issues/11179
synced_at: 2026-01-12T16:00:30Z
```

# uv install from wheel does not use index specified in pyproject.toml

---

_@alksndrglk_

### Question

_pyproject.toml_ 

```
[project]
name = "storekeeper"
version = "1.0.0"
description = "S3 and DAV clients"
readme = "README.md"
requires-python = ">=3.11,<3.14"
authors = []
classifiers = [
    "Development Status :: 5 - Production/Stable",
    "Framework :: AsyncIO",
    "Intended Audience :: Developers",
    "Intended Audience :: Information Technology",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3 :: Only",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12",
    "Programming Language :: Python :: 3.13",
    "Topic :: Internet :: WWW/HTTP",
    "Topic :: Software Development :: Libraries"
]
dependencies = [
    "ahttp==1.0.2",
    "aiohttp-s3-client==1.0.0",
]

[[tool.uv.index]]
name = "ahttp_repo"
url = "https://<GITLAB_URL>/api/v4/projects/1035/packages/pypi/simple"
explicit = true

[tool.uv.sources]
ahttp = { index = "ahttp_repo" }
```

building simply with ```uv build```

trying to install from local distribution with ```uv tool install --verbose /storekeeper/dist/storekeeper-1.0.0-py3-none-any.whl```

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of ahttp==1.0.2 and storekeeper==1.0.0 depends on ahttp==1.0.2, we can conclude that storekeeper==1.0.0 cannot be used.
      And because only storekeeper==1.0.0 is available and you require storekeeper, we can conclude that your requirements are unsatisfiable.
```

**What is the proper way to force uv use index from pyproject.toml?** Am I doing something wrong?

### Platform

macOS 15.3 (24D60)

### Version

uv 0.5.7 (3ca155ddd 2024-12-06)

---

_Label `question` added by @alksndrglk on 2025-02-03 09:46_

---

_Comment by @notatallshaw on 2025-02-03 16:15_

The tool section of pyproject.toml is related to the source.

Once you build a package into a wheel then any installer should read from the wheels metadata, not from the pyproject.toml.

And the wheels metadata, by design, has no way to specify what index it's dependencies are sourced from, that's up to the client installing it.


---

_Comment by @alksndrglk on 2025-02-04 09:23_

Thank you for the answer.

Am I clear that I am facing something similar described in [1808#](https://github.com/astral-sh/uv/issues/1808#issuecomment-2327626020) ?

Even if I change ```pyproject.toml``` like so

```
dependencies = [
    "ahttp @ https://< GITLAB_URL >/api/v4/groups/989/-/packages/pypi/files/9a231837b0c0749c7a200790ba78808cdf89329a1a449d034658134a8f09856a/ahttp-1.0.2-py3-none-any.whl",
    "aiohttp-s3-client==1.0.0",
]
```

Installation using ```uv install <link>``` resolve all dependencies.

But when I add desired package ```storekeeper``` to pyproject.toml, uv can not create lock file.
```
error: Package `ahttp` attempted to resolve via URL: https://< GITLAB_URL >/api/v4/groups/989/-/packages/pypi/files/9a231837b0c0749c7a200790ba78808cdf89329a1a449d034658134a8f09856a/ahttp-1.0.2-py3-none-any.whl. 
URL dependencies must be expressed as direct requirements or constraints. 
Consider adding `ahttp @ https://< GITLAB_URL >/api/v4/groups/989/-/packages/pypi/files/9a231837b0c0749c7a200790ba78808cdf89329a1a449d034658134a8f09856a/ahttp-1.0.2-py3-none-any.whl` to your dependencies or constraints file.
```

---

_Comment by @alksndrglk on 2025-02-04 11:33_

Found [2511#](https://github.com/astral-sh/uv/issues/2511) and also [transitive url dependencies](https://docs.astral.sh/uv/pip/compatibility/#transitive-url-dependencies). 

Now I think it makes me clear.

---

_Closed by @charliermarsh on 2025-02-11 03:16_

---
