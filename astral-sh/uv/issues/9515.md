```yaml
number: 9515
title: Specifying different dependency versions for different platforms
type: issue
state: closed
author: merajhashemi
labels:
  - question
assignees: []
created_at: 2024-11-29T00:24:06Z
updated_at: 2024-11-29T20:48:05Z
url: https://github.com/astral-sh/uv/issues/9515
synced_at: 2026-01-10T04:36:21Z
```

# Specifying different dependency versions for different platforms

---

_Issue opened by @merajhashemi on 2024-11-29 00:24_

Hello,
I've encountered an issue while trying to specify different versions of PyTorch for Intel macOS in my project.
Here is a minimal `pyproject.toml` that shows the issue:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "torch>=1.13.1,<2.3; sys_platform == 'darwin' and platform_machine == 'x86_64'",
    "torch>=1.13.1; sys_platform != 'darwin' or platform_machine == 'arm64'",
]
```

I’m trying to make sure that Intel macOS (darwin_x86_64) uses a specific version of PyTorch as there are no new wheels available for versions 2.3 and above. But the dependency resolver seems to apply this constraint universally when generating the lock file, resulting in version 2.2.2 being installed across all platforms.

Is there a better way to set these conditions, maybe in a `tool.uv` section? Any tips would be super helpful!

---
```bash
$ uv --version
uv 0.5.5
```
Related to #8358.

---

_Comment by @charliermarsh on 2024-11-29 16:51_

I'd probably suggest something like this:
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "torch>=<2.3; sys_platform == 'darwin' and platform_machine == 'x86_64'",
    "torch>=2.3; sys_platform != 'darwin' or platform_machine != 'x86_64'",
]
```

---

_Label `question` added by @charliermarsh on 2024-11-29 16:51_

---

_Comment by @merajhashemi on 2024-11-29 19:17_

Thanks @charliermarsh.
This seems to work as expected:
```toml
dependencies = [
    "torch>1.13,<2.3; sys_platform == 'darwin' and platform_machine == 'x86_64'",
    "torch>=2.3; sys_platform != 'darwin' or platform_machine != 'x86_64'",
]
```
However, our library is designed to support PyTorch versions greater than 1.13 across all platforms. When we specify the dependencies as follows:
```toml
dependencies = [
    "torch>1.13,<2.3; sys_platform == 'darwin' and platform_machine == 'x86_64'",
    "torch>=1.13; sys_platform != 'darwin' or platform_machine != 'x86_64'",
]
```
it still results in all platforms installing version 2.2.2.


---

_Comment by @charliermarsh on 2024-11-29 19:22_

Yeah, that's intended. If you provide constraints that can be solved with fewer total versions, uv will prefer to use fewer versions. In that case, 2.2.2 satisfies both of your constraints, so uv chooses 2.2.2 rather than creating a more heterogenous environment (i.e., an environment that's _less_ consistent across platforms).

If you're looking for a change in _that_ behavior, follow https://github.com/astral-sh/uv/pull/8686 and https://github.com/astral-sh/uv/issues/7190. But uv is doing the intended thing given those constraints.

---

_Closed by @charliermarsh on 2024-11-29 19:22_

---

_Comment by @merajhashemi on 2024-11-29 20:47_

Thank you, @charliermarsh. I've just noticed that this behavior doesn't align with the pip interface. When I execute `uv pip install .` with the following dependencies:
```toml
dependencies = [
    "torch>1.13,<2.3; sys_platform == 'darwin' and platform_machine == 'x86_64'",
    "torch>=1.13; sys_platform != 'darwin' or platform_machine != 'x86_64'",
]
```
it successfully installs the latest version of PyTorch on linux. Could we have other uv commands, such as `uv sync` and `uv lock`, behave similarly?

---
