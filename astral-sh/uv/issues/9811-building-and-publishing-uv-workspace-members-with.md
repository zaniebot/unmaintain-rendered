```yaml
number: 9811
title: Building and publishing uv workspace members with pinned version constraints
type: issue
state: open
author: mtnmbr
labels:
  - needs-design
assignees: []
created_at: 2024-12-11T14:49:10Z
updated_at: 2025-12-17T17:24:42Z
url: https://github.com/astral-sh/uv/issues/9811
synced_at: 2026-01-12T15:59:59Z
```

# Building and publishing uv workspace members with pinned version constraints

---

_@mtnmbr_

Hey,

first of all. Thanks a lot for your efforts on making the python tooling ecosystem better! I love ruff and uv :). Please keep up the great work.

I am currently migrating an existing monorepo from poetry using the [poetry-monorepo-dependency-plugin](https://pypi.org/project/poetry-monorepo-dependency-plugin/) to uv.

I thought that uv workspaces should be the way to handle this usecase. 

The current setup looks somewhat like this:

```
uv_monorepo
│   .gitignore
│   .python-version
│   pyproject.toml
│   README.md
│   uv.lock
│
├───package_a
│   │   pyproject.toml
│   │   README.md
│   │
│   └───src
│       └───package_a
│               py.typed
│               __init__.py
│
├───package_b
│   │   pyproject.toml
│   │   README.md
│   │
│   └───src
│       └───package_b
│               py.typed
│               __init__.py
│
└───package_c
    │   pyproject.toml
    │   README.md
    │
    └───src
        └───package_c
                py.typed
                __init__.py
```
uv tree gives the following output to show the inter_package dependecies:
```
#uv tree
Using CPython 3.12.1
Resolved 4 packages in 13ms
package-a v0.1.0
└── package-b v0.2.0
    └── package-c v0.3.0
uv-monorepo v0.1.0
```

So my previous setup did allow me to publish the packages individually to our internal pypi mirror with pinned versions of the dependencies.

My  main pyproject.toml looks like this atm. (in reality the workspace members are added to the toplevel virtual workspace package to be able to omit the `--al--packages` on `uv sync`):

```toml
[project]
name = "uv-monorepo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []


[tool.uv.workspace]
members = ["package_a", "package_b", "package_c"]

```

With e.g. the package_a pyproject.toml being:

```toml
[project]
name = "package-a"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "package-b",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
package-b = { workspace = true }

```

This demo setup has basically been fully created with several `uv init` calls.

The hope was that `uv build --package-a --wheel` would output a wheel that has the pinned lower version of the dependency `package-b` listed internally. At least this is what my previous setup did. 
Instead I get a METADATA file like this:
```
Metadata-Version: 2.3
Name: package-a
Version: 0.1.0
Summary: Add your description here
Requires-Python: >=3.12
Requires-Dist: package-b
```

When published to our pypi instance this would ofcource not resolve to the actual boundary of `package-b>=0.2.0,<0.3 ` that would be required here.

After bumping e.g. the patch version of `package-b` I would also assume that the version inside the METADATA is bumped to e.g. 0.2.1.


Is this use-case supported? What is the current approach to handle this properly? I am open for suggestions!

Best regards 
Michael


---

_Comment by @gracevivi523 on 2024-12-12 03:27_

I have opened [this issue](https://github.com/astral-sh/uv/issues/9626) and I think we are trying to achieve the same goal. 

---

_Comment by @zanieb on 2024-12-12 03:46_

Related

- https://github.com/astral-sh/uv/issues/8729
- https://github.com/astral-sh/uv/issues/8949

(though I think this request is distinct)

---

_Comment by @zanieb on 2024-12-12 03:49_

It would be nice to have some automated support for this, but, as described in the linked issues, it's sort of hard to do this in a spec-compliant way. We haven't designed a solution for this category of problem yet.

---

_Label `needs-design` added by @zanieb on 2024-12-12 03:49_

---

_Comment by @mtnmbr on 2024-12-12 07:47_

Hey, thanks for the pointing out the related topics. I must have overlooked https://github.com/astral-sh/uv/issues/8729 for some reason.

I think it's the one most related to my problem.

So as for now there is no standard way to do this. In the meanwhile do you have any suggestions on how to handle this?

I mean for version bumping I am using a separate tool anyways atm. So I might give it a try to manually add the version constraint in the dependency groups and make that tool update the other pyproject.toml files as well. 

Though maybe not intended it at least did not seem to break anything when adding version constraints on workspace members (funny enough not even if the version does not match the one in the workspace at all)

---

_Closed by @mtnmbr on 2024-12-12 12:10_

---

_Comment by @mtnmbr on 2024-12-12 13:37_

So this is not fully related to the marked issue but at least covered by it. Maybe this is even possible for only workspaces?

---

_Reopened by @mtnmbr on 2024-12-12 13:37_

---

_Comment by @Gr1N on 2025-05-16 10:03_

Hey!

We're facing the same usability problem with our mono repositories. We solved it with a custom script we run before release, which pins versions across all packages and dependencies inside the repo by the specified Git tag.

Here is an example of our solution:
```
# /// script
# requires-python = ">=3.12,<3.13"
# dependencies = [
#     "tomlkit>=0.13,<1.0",
# ]
# ///
import argparse
from pathlib import Path

import tomlkit


def main() -> None:
    parser = argparse.ArgumentParser()

    subparsers = parser.add_subparsers(metavar="COMMAND")
    subparsers.required = True

    freeze = subparsers.add_parser(
        "freeze", help="Freezes versions of packages in workspaces"
    )
    freeze.set_defaults(func=freeze_func)
    freeze.add_argument("--version", required=True)

    (args := parser.parse_args()).func(args)


def freeze_func(args: argparse.Namespace) -> None:
    for p in Path(".").glob("packages/*/pyproject.toml"):
        freeze_pyproject(p, version=args.version)


def freeze_pyproject(pyproject: Path, /, *, version: str) -> None:
    print(f'Freezing in "{pyproject}"')

    with pyproject.open() as f:
        pyproject_d = tomlkit.loads(f.read())

    for deps in (
        pyproject_d["project"]["dependencies"],
        *(pyproject_d["project"].get("optional-dependencies", {}).values()),
    ):
        freeze_dependencies(deps, version=version)

    with pyproject.open(mode="w") as f:
        f.write(tomlkit.dumps(pyproject_d))


def freeze_dependencies(deps: list[str], /, *, version: str) -> None:
    for i in range(len(deps)):
        if deps[i].startswith("basename"):
            deps[i] = f"{deps[i]}=={version}"


if __name__ == "__main__":
    main()
```

It would be great to have such an interface out of the box; we believe it can massively improve the experience with workspaces and be a good point of departure for extending the `uv version` command.

---

_Comment by @Sillocan on 2025-12-04 21:33_

I would even settle for the version being pinned with an `package-b==0.2.0`. Has anyone accomplished this using hatch build system hooks? (I think the uv build system doesnt have build hooks yet still?)

---

_Comment by @konstin on 2025-12-17 17:24_

Note that even with hatch, you need to declare dependencies as dynamic to support rewriting properly. This may get easier with PEP 808, which allows doing some of these properly. The flipside is that uv then always need to build to determine the workspace dependencies, which will noticeably slow down `uv sync`.

---
