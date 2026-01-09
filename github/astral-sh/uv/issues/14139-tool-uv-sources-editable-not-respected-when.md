---
number: 14139
title: "`tool.uv.sources.editable` not respected when conditional on `group`"
type: issue
state: closed
author: SpencerMcO
labels:
  - bug
assignees: []
created_at: 2025-06-18T21:29:12Z
updated_at: 2025-07-11T02:20:13Z
url: https://github.com/astral-sh/uv/issues/14139
synced_at: 2026-01-07T13:12:18-06:00
---

# `tool.uv.sources.editable` not respected when conditional on `group`

---

_Issue opened by @SpencerMcO on 2025-06-18 21:29_

### Summary

**Summary:**
I have a dependency in my `tool.uv.sources` section of my `pyproject.toml`, and I'm providing two entries for that dependency - one where `editable = true` using `group = 'dev'` and one where `editable = false` using `group = 'container'`. When I run `uv sync` (which includes the `dev` dependency group by default), everything works as expected - the dependency is installed in editable mode. However, when I run `uv sync --no-dev --group container`, nothing happens. It seems to think the dependency is already installed as intended. Even if I delete and recreate the virtual environment and run `uv sync --no-dev --group container`, it installs my dependency as an editable install, despite the explicit `editable = false` flag I gave it.

**Details & Minimal Reproduction:**
I have a monorepo where there's a "utils" folder/package I'm trying to install into each of the other "services" or apps in the monorepo. The folder structure/layout can be seen below:
```sh
foo
├── services
│   └── myapp
│       ├── main.py
│       ├── pyproject.toml
│       ├── README.md
│       └── uv.lock
└── utils
    ├── pyproject.toml
    ├── README.md
    ├── src
    │   └── utils
    │       ├── __init__.py
    │       └── main.py
    └── uv.lock
```

The `utils` directory was created with `uv init --package utils` and has the default files and layout. The only modifications I made were (1) moving the "hello world" code from the `__init__.py` to a `main.py`, (2) changing the "hello world" string that's printed, and (3) calling the main function within `if __name__ == '__main__':`.

The `myapp` directory was created with `uv init myapp`. The only changes I made to this directory include (1) modifying the `main.py`:
```python3
from utils.main import main as utils_main

def main():
    print("Hello from myapp!")
    utils_main()


if __name__ == "__main__":
    main()
```

(2) adding the `utils` dir as an editable dependency: `uv add --editable ../../utils`; and (3) updating the `pyproject.toml` to indicate dependency groups, conflicts, and a non-editable install of `utils` if the `container` dependency group was used:
```toml
[project]
name = "myapp"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "utils",
]

[dependency-groups]
dev = ["utils"]
container = ["utils"]

[tool.uv]
conflicts = [[{ group = 'dev' }, { group = 'container' }]]

[tool.uv.sources]
utils = [{ path = "../../utils", editable = true, group = 'dev' }, { path = "../../utils", editable = false, group = 'container' }]
```

Once all of this is set up, you can run `uv sync` to install the `dev` dependency group, and you can run `uv run main.py` from the `myapp` directory to verify it works. You can also modify the print statement inside of the `foo/utils/src/utils/main.py` file and re-run `uv run main.py` inside the `myapp` directory to verify that the print statement updates without needing to reinstall `utils`.

Next, we come to the issue - if you run `uv sync --no-dev --group container`, that should remove the `dev` dependency group's dependencies and install the `container` group's dependencies, which SHOULD install `utils` as a non-editable dependency. However, when I change the print statement in the `utils` script and run `uv run --no-sync main.py` from the `myapp` folder,  then the print statement changes when it shouldn't (since `utils` shouldn't be an editable dependency now).

Interestingly, if I comment out the line in the `myapp/pyproject.toml` that indicates `editable = true` (as seen below) and try running `uv sync --no-dev --group container` again, and then repeat the tests from the prior paragraph, THEN I get the behavior I expected - the print statement logged to the console doesn't change despite my edits to the `utils/src/utils/main.py` script.

```toml
[project]
name = "myapp"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "utils",
]

[dependency-groups]
dev = ["utils"]
container = ["utils"]

[tool.uv]
conflicts = [[{ group = 'dev' }, { group = 'container' }]]

[tool.uv.sources]
utils = [
#	{ path = "../../utils", editable = true, group = 'dev' },
	{ path = "../../utils", editable = false, group = 'container' }
]
```

### Platform

macOS 15 arm64

### Version

uv 0.7.13

### Python version

Python 3.12.8

---

_Label `bug` added by @SpencerMcO on 2025-06-18 21:29_

---

_Comment by @zanieb on 2025-06-18 21:32_

A couple questions...

Is it an invalidation issue? i.e., does it work when adding the `--reinstall` flag? When you do `uv run main.py` are you using `--no-sync` or similar to prevent us from syncing back to the default dependency groups?

Would you mind putting this in a Git repository so we can reproduce easily?

---

_Comment by @SpencerMcO on 2025-06-18 21:42_

> Is it an invalidation issue? i.e., does it work when adding the `--reinstall` flag? When you do `uv run main.py` are you using `--no-sync` or similar to prevent us from syncing back to the default dependency groups?

I tried:
* running `uv sync --no-dev --group container --reinstall`
* then `uv run --no-sync main.py`
* then modified the "utils" print statement
* then ran `uv run --no-sync main.py` again

and the console still shows the updated "utils" print statement (i.e., seems to be using an editable install).

> Would you mind putting this in a Git repository so we can reproduce easily?

Sure! Here's the link: https://github.com/SpencerMcO/uv-editable-install-repro

---

_Comment by @zanieb on 2025-06-18 22:18_

Hm https://github.com/SpencerMcO/uv-editable-install-repro/blob/8dcfef29fb6d852dce9d3d38b0b05615727cce1a/services/myapp/uv.lock#L19-L24

Your lockfile appears to be resolving to an editable path for both groups.

---

_Comment by @zanieb on 2025-06-18 22:18_

(We'll need to investigate further)

---

_Comment by @zanieb on 2025-06-18 22:35_

In the container, you could use `--no-editable` instead of doing this awkward thing with groups.

---

_Comment by @SpencerMcO on 2025-06-18 22:56_

> In the container, you could use `--no-editable` instead of doing this awkward thing with groups.

Just tested this out locally and in my container, and it looks like it's working! Thank you!!

What would you like me to do with the issue since your suggestion worked? Close the issue, or would you still like to investigate further?

---

_Comment by @zanieb on 2025-06-18 23:16_

There's still a bug here, so we can leave it open to investigate.

---

_Renamed from "tool.uv.sources "editable" not respected" to "`tool.uv.sources.editable` not respected when conditional on `group`" by @zanieb on 2025-06-18 23:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-22 01:42_

---

_Referenced in [astral-sh/uv#14197](../../astral-sh/uv/pulls/14197.md) on 2025-06-22 02:15_

---

_Comment by @charliermarsh on 2025-07-11 02:20_

The bug itself will be fixed in v0.8.0, just merged.

---

_Closed by @charliermarsh on 2025-07-11 02:20_

---
