---
number: 10778
title: "Bug? Can't run uv sync on package with --no-build"
type: issue
state: closed
author: Chasarr
labels:
  - question
assignees: []
created_at: 2025-01-20T14:08:30Z
updated_at: 2025-01-20T16:32:29Z
url: https://github.com/astral-sh/uv/issues/10778
synced_at: 2026-01-10T01:24:57Z
---

# Bug? Can't run uv sync on package with --no-build

---

_Issue opened by @Chasarr on 2025-01-20 14:08_

When trying to create a new package with say `uv init --package`, add a dependency, say the progress package, with `uv add progress`. My pyprojects.toml looks like this:
```toml
[project]
name = "hatch-demo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "My Name", email = "my@e.mail" }
]
requires-python = ">=3.12"
dependencies = [
    "progress>=1.6",
]

[project.scripts]
hatch-demo = "hatch_demo:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

```

Company policy is to ensure that we don't use source distributions (only prebuilt binary wheel dependencies) which we used to do with `pip install -r requirements.txt --no-compile`. We are now trying to switch over to uv, but `uv sync --no-build` returns
`error: Distribution `hatch-demo==0.1.0 @ editable+.` can't be installed because it is marked as `--no-build` but has no binary distribution`

Although I can see why this returns an error, the purpose of --no-build for us should be to not build dependencies when building our package, not to inhibit us from building the package itself. We could technically do `uv sync --no-build-package pkg1 --no-build-package pkg2 ... --no-build-package pkgN` for all dependencies, but that doesn't seem like a very elegant solution.

Not sure if a fix for this problem aligns with the goals of uv.

TLDR: I want to 1) uv sync dependencies with --no-build and then 2) build the package with said dependencies.

---

_Comment by @charliermarsh on 2025-01-20 14:21_

I would suggest `uv sync --no-build --no-install-project` to skip building and installing `hatch-demo`.

---

_Label `question` added by @charliermarsh on 2025-01-20 14:22_

---

_Comment by @Chasarr on 2025-01-20 16:23_

Thanks for guiding me to the right answer! To avoid running build on packages in my CI environment, I just enter

```sh
uv sync --no-build --no-install-project # Fetches prebuilt wheels
uv sync --frozen # builds project with previously synced dependencies
uv run --frozen <MY_PACKAGE> # Now CI can perform tests on the package before publishing to internal Python package index
```

Thanks a lot!

---

_Closed by @Chasarr on 2025-01-20 16:23_

---

_Comment by @charliermarsh on 2025-01-20 16:32_

No worries! Thank you for following up.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-20 16:32_

---
