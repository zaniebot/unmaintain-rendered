```yaml
number: 11521
title: uvx in 0.5.31 no longer runs command and instead complains about hyphens of its own imagining
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
assignees: []
created_at: 2025-02-14T20:30:53Z
updated_at: 2025-02-14T21:52:01Z
url: https://github.com/astral-sh/uv/issues/11521
synced_at: 2026-01-10T03:50:31Z
```

# uvx in 0.5.31 no longer runs command and instead complains about hyphens of its own imagining

---

_Issue opened by @hauntsaninja on 2025-02-14 20:30_

```
λ uv pip install uv==0.5.30
Using Python 3.11.8 environment at: /Users/shantanu/code/venvs/openai-fppa
Audited 1 package in 260ms
λ uvx change_wheel_version --help
usage: change_wheel_version [-h] [--local-version LOCAL_VERSION] [--version VERSION] [--delete-old-wheel] [--allow-same-version] [--platform-tag PLATFORM_TAG] wheel

positional arguments:
  wheel

options:
  -h, --help            show this help message and exit
  --local-version LOCAL_VERSION
  --version VERSION
  --delete-old-wheel
  --allow-same-version
  --platform-tag PLATFORM_TAG
                        Force the platform tag to this value. For example, 'py3-none-any'. See https://packaging.python.org/en/latest/specifications/platform-compatibility-tags.
λ uv pip install uv==0.5.31
Using Python 3.11.8 environment at: /Users/shantanu/code/venvs/openai-fppa
Resolved 1 package in 27ms
Uninstalled 1 package in 7ms
Installed 1 package in 9ms
 - uv==0.5.30
 + uv==0.5.31
λ uvx change_wheel_version --help
The executable `change-wheel-version` was not found.
warning: An executable named `change-wheel-version` is not provided by package `change-wheel-version`.
The following executables are provided by `change-wheel-version`:
- change_wheel_version
Consider using `uvx --from change-wheel-version <EXECUTABLE_NAME>` instead.
```

My guess is user input is getting normalised to have hyphens?

---

_Comment by @charliermarsh on 2025-02-14 20:32_

Thanks, I'll take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-14 20:32_

---

_Comment by @zanieb on 2025-02-14 20:33_

Check this out..

```
❯ uvx --from uv@0.6.0 uvx change_wheel_version --help
Installed 1 package in 3ms
Installed 4 packages in 9ms
The executable `change-wheel-version` was not found.
warning: An executable named `change-wheel-version` is not provided by package `change-wheel-version`.
The following executables are provided by `change-wheel-version`:
- change_wheel_version
Consider using `uvx --from change-wheel-version <EXECUTABLE_NAME>` instead.

❯ uvx --from uv@0.5.30 uvx change_wheel_version --help
Installed 1 package in 4ms
Installed 4 packages in 6ms
usage: change_wheel_version [-h] [--local-version LOCAL_VERSION] [--version VERSION] [--delete-old-wheel] [--allow-same-version] [--platform-tag PLATFORM_TAG] wheel

positional arguments:
  wheel

options:
  -h, --help            show this help message and exit
  --local-version LOCAL_VERSION
  --version VERSION
  --delete-old-wheel
  --allow-same-version
  --platform-tag PLATFORM_TAG
                        Force the platform tag to this value. For example, 'py3-none-any'. See https://packaging.python.org/en/latest/specifications/platform-compatibility-tags.
```

---

_Comment by @charliermarsh on 2025-02-14 20:34_

Sorry, I see the problem here.

---

_Label `bug` added by @charliermarsh on 2025-02-14 21:17_

---

_Closed by @charliermarsh on 2025-02-14 21:25_

---

_Comment by @hauntsaninja on 2025-02-14 21:52_

Thank you!

---
