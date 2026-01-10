---
number: 10255
title: "Bug: Circular dependencies at build-time cause hang without error"
type: issue
state: closed
author: CarrotManMatt
labels:
  - error messages
assignees: []
created_at: 2024-12-31T16:47:45Z
updated_at: 2025-01-01T14:21:44Z
url: https://github.com/astral-sh/uv/issues/10255
synced_at: 2026-01-10T01:24:51Z
---

# Bug: Circular dependencies at build-time cause hang without error

---

_Issue opened by @CarrotManMatt on 2024-12-31 16:47_

When a project uses a package for building that has the original project as a runtime dependency, all build/lock/sync commands hang without any message or error.

# Steps to reproduce

1. Create project `foo` with the following `pyproject.toml`

```toml
[build-system]
build-backend = "hatchling.build"
requires = ["hatchling"]

[project]
name = "foo"
version = "1.0.0"
description = "My package foo"

[tool.hatch.build]
only-packages = true

[tool.uv]
no-binary-package = ["foo"]
no-build = true
package = true
trusted-publishing = "always"
```

2. Create the package `foo` within the project
```bash
mkdir foo/ && touch foo/__init__.py
```

3. Build the project as a wheel & sdist
```bash
uv build --no-sources --build
```

4. Publish the project to testpypi (package names will need to be changed to avoid collisions)
```bash
uv publish --publish-url https://test.pypi.org/legacy/
```

5. Create a new project bar with the following pyproject.toml
```toml
[build-system]
build-backend = "hatchling.build"
requires = ["hatchling"]

[project]
dependencies = ["foo"]
name = "bar"
version = "2.0.0"
description = "My package bar (Has a dependency!)"

[tool.hatch.build]
only-packages = true

[tool.uv]
no-binary-package = ["foo"]
no-build = true
package = true
trusted-publishing = "always"
```

6. Create the package `bar` within the project
```bash
mkdir bar/ && touch bar/__init__.py
```

7. Build the project as a wheel only
```bash
uv build --no-sources --build --wheel
```

8. Publish the built wheel to testpypi (package names will need to be changed to avoid collisions)
```bash
uv publish --publish-url https://test.pypi.org/legacy/
```

9. Edit the `pyproject.toml` of the `foo` project
```toml
[build-system]
build-backend = "hatchling.build"
requires = ["hatchling", "bar"]
```

10. Run the following build command on project `foo`. Compared to step 3 this will now hang indefinetly
```bash
uv build --no-sources --build
```


I think this is a regression as the build of `foo` using `bar` as a build requirement worked successfully in uv version 0.5.11, but in 0.5.13 it fails (I have not verified this however). Is there something I am missing with this kind of circular dependency not being allowed? I would expect the build dependencies to be isolated and just use the existing published wheel, rather than installing the local project (which is what i think is happening)

---

_Referenced in [astral-sh/uv#10252](../../astral-sh/uv/issues/10252.md) on 2024-12-31 16:48_

---

_Comment by @charliermarsh on 2024-12-31 17:11_

Hmm. Why would this work, though? Even if we install `bar` from a wheel, we still have to install its requirements, which include `foo`. I think this should probably error due to a cycle (but not hang).

---

_Label `error messages` added by @charliermarsh on 2024-12-31 17:11_

---

_Comment by @CarrotManMatt on 2024-12-31 17:53_

Gotcha, many thanks for helping out. I think I didn't fully appreciate that there was a full dependency cycle during the build stage too, due to my setup possibly being outdated with the `foo` package version.

A more helpful error message rather than just hanging would be appreciated for people running into this issue in the future.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-31 21:19_

---

_Comment by @charliermarsh on 2025-01-01 01:00_

So I took the time to publish these as https://test.pypi.org/project/circular-one/ and https://test.pypi.org/project/circular-two/, and this actually works for me?

Specifically, this works `uv build --no-sources --build --index https://test.pypi.org/simple --index-strategy unsafe-best-match --verbose` given:

```toml
[project]
name = "circular-one"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "Charlie Marsh", email = "charlie.r.marsh@gmail.com" }
]
requires-python = ">=3.13.0"
dependencies = []

[build-system]
requires = ["hatchling", "circular-two"]
build-backend = "hatchling.build"

[tool.hatch.build]
only-packages = true

[tool.uv]
no-binary-package = ["circular-one"]
no-build = true
package = true
trusted-publishing = "always"
```

My guess is it might fail if I re-publish with:

```toml
[build-system]
requires = ["hatchling", "circular-two"]
build-backend = "hatchling.build"
```


---

_Comment by @charliermarsh on 2025-01-01 01:01_

Okay yeah, once I re-publish to create the circular dependency, it deadlocks as expected.

---

_Comment by @charliermarsh on 2025-01-01 01:13_

Notably, `pypa/build` also fails here: `PIP_NO_BINARY=circular-one PIP_EXTRA_INDEX_URL=https://test.pypi.org/simple python3.13 -m build`.

---

_Referenced in [astral-sh/uv#10258](../../astral-sh/uv/pulls/10258.md) on 2025-01-01 02:55_

---

_Closed by @charliermarsh on 2025-01-01 03:22_

---

_Closed by @charliermarsh on 2025-01-01 03:22_

---

_Comment by @CarrotManMatt on 2025-01-01 14:21_

Amazing, thank you so much! For improving the error messages!

---
