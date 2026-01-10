---
number: 12529
title: Command to update metadata in PEP 723 script from project
type: issue
state: open
author: dbohdan
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-03-28T13:10:58Z
updated_at: 2025-04-01T20:50:55Z
url: https://github.com/astral-sh/uv/issues/12529
synced_at: 2026-01-10T01:25:21Z
---

# Command to update metadata in PEP 723 script from project

---

_Issue opened by @dbohdan on 2025-03-28 13:10_

### Summary

Python scripts with inline script metadata are incredibly convenient to use but not so convenient to develop because Python tooling expects a project. My preferred solution to this problem has been to develop PEP 723 scripts as regular Python projects that have `pyproject.toml`. This lets you use your normal tooling like Ruff, Pyright, and Poe like in any other project. You get a place to specify Ruff ignore rules and development dependencies like type stubs.

A drawback of this approach is *duplicated dependencies*: you must have the dependencies in both the PEP 723 script itself and `pyproject.toml`. It is better to have a single source of truth. uv could help by implementing a command to copy the list of dependencies and the required Python version from `pyproject.toml` to the script. What I mean is, when you ran `uv foo script.py`, `project.dependencies` and `project.requires-python` from `pyproject.toml` would be copied to the metadata comment block in `script.py`.

There are other potential solutions to create a workflow for PEP 723 scripts that enables tooling and development dependencies. One would be for uv to directly use the dependencies and Python version from a script's metadata with development dependencies from `pyproject.toml`. Such a solution seems more complex and more potentially surprising to the user.

### Example

I migrated a project from [shiv](https://github.com/linkedin/shiv) to PEP 723 recently and wrote some code to experiment with this. It creates a separate `bundle.py`, but with a little more effort it could replace the inline metadata in the source.

```python
#! /usr/bin/env python3
# License: https://dbohdan.mit-license.org/@2025/license.txt

import re
import tomllib
from pathlib import Path
from string import Template

import tomli_w

DEPENDENCIES = "dependencies"
PROJECT = "project"
REQUIRES_PYTHON = "requires-python"

DST = Path("bundle.py")
PYPROJECT = Path("pyproject.toml")
SRC = Path("main.py")

BUNDLE = Template(
    """
#! /usr/bin/env -S uv run --quiet --script
# /// script
$toml
# ///

$code
""".strip()
)


def main() -> None:
    with PYPROJECT.open("rb") as f:
        pyproject = tomllib.load(f)

    toml = tomli_w.dumps(
        {
            DEPENDENCIES: pyproject[PROJECT][DEPENDENCIES],
            REQUIRES_PYTHON: pyproject[PROJECT][REQUIRES_PYTHON],
        },
        indent=2,
    )

    code = SRC.read_text()
    code = re.sub(r"^#![^\n]+\n", "", code)

    bundle = BUNDLE.substitute(
        toml="\n".join(f"# {line}" for line in toml.splitlines()),
        code=code,
    )

    DST.write_text(bundle)


if __name__ == "__main__":
    main()
```

### See also

An [HN discussion](https://news.ycombinator.com/item?id=43500124#43500814) today prompted me to open this issue.

---

_Label `enhancement` added by @dbohdan on 2025-03-28 13:10_

---

_Comment by @zanieb on 2025-04-01 19:42_

I think the solution here is rather to add PEP 723 script support to editors, LSPs, and other tools.

It's a fun idea, but I don't think we'll invest in duplicating dependencies across `pyproject.toml` and scripts.

---

_Label `wish` added by @zanieb on 2025-04-01 19:43_

---

_Comment by @dbohdan on 2025-04-01 20:50_

Fair enough. There is a reasonable argument that this can only be a stopgap measure. If PEP 723 scripts are widely adopted, this feature will become unnecessary because tools will directly support PEP 723; if PEP 723 isn't widely adopted, maintaining this feature might not be worth the effort.

---
