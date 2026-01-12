```yaml
number: 2494
title: uv API does not match pip for non-pypi packages
type: issue
state: closed
author: charles-cooper
labels:
  - duplicate
assignees: []
created_at: 2024-03-17T14:52:36Z
updated_at: 2024-03-23T01:49:23Z
url: https://github.com/astral-sh/uv/issues/2494
synced_at: 2026-01-12T15:58:38Z
```

# uv API does not match pip for non-pypi packages

---

_@charles-cooper_

```
$ uv --version
uv 0.1.21
```

first of all, thank you for making such a great tool!

my issue is that the uv CLI does not match pip for locally installed packages or packages installed from url.

examples:
```
$ uv pip install .
error: Failed to parse `.`
  Caused by: URL requirement must be preceded by a package name. Add the name of the package before the URL (e.g., `package_name @ /path/to/file`).
```

```
$ uv pip install .[test]
error: Failed to parse `.[test]`
  Caused by: URL requirement must be preceded by a package name. Add the name of the package before the URL (e.g., `package_name @ /path/to/file`).
.[test]
^^^^^^^
```

```
$ uv pip install "git+https://github.com:vyperlang/titanoboa"
error: Failed to parse `git+https://github.com:vyperlang/titanoboa`
  Caused by: Expected one of `@`, `(`, `<`, `=`, `>`, `~`, `!`, `;`, found `+`
git+https://github.com:vyperlang/titanoboa
   ^
```

these can be worked around by including the package name, as in `uv pip install "my_package @ ."`, but it's a bit redundant and requires extra typing. it will also fail if you type the package name wrong, because uv infers the package name from the package metadata anyways:
```
$ uv pip install "asdlkj @ ."
error: Failed to build: asdlkj @ file:///home/charles/titanoboa
  Caused by: Package metadata name `titanoboa` does not match given name `asdlkj`
```

so the `"asdlkj @"` part can just be completely dropped i think. matching pip here, or even going further and allowing you to skip the `.` (so `uv pip install`) would be great.

---

_Comment by @zanieb on 2024-03-17 15:52_

Hi! All of these examples are #313 which we intend to address soon.

See that issue or our [pip-compatibility guide](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#direct-url-dependencies-without-package-names) for more context.

---

_Closed by @zanieb on 2024-03-17 15:52_

---

_Label `duplicate` added by @zanieb on 2024-03-17 15:52_

---

_Comment by @charliermarsh on 2024-03-22 21:27_

This is supported as of `v0.1.24`. You can `uv pip install .` directly, without including a package name.

---

_Comment by @charles-cooper on 2024-03-23 01:49_

awesome, thank you!

---
