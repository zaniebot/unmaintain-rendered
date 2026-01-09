---
number: 3333
title: "Feature request: multiple --python-platform arguments"
type: issue
state: closed
author: dimostenis
labels:
  - needs-design
assignees: []
created_at: 2024-05-01T19:08:03Z
updated_at: 2024-05-06T16:34:18Z
url: https://github.com/astral-sh/uv/issues/3333
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature request: multiple --python-platform arguments

---

_Issue opened by @dimostenis on 2024-05-01 19:08_

### Quickly:

Multiple platforms like:

```sh
uv pip compile pyproject.toml --python-platform=macos --python-platform=linux
```

### Slowly:

In our projects we develop on *macos*/*windows*, but deploy on *linux*.
And often platform envs cant just be the same. Typically when working with [pytorch](https://pypi.org/project/torch/) and friends. 
For convenience we use [pip-compile-cross-platform](https://pypi.org/project/pip-compile-cross-platform/) to generate *requirements.txt* and then `uv` when installing.

It would be cool if you allow multiple `--python-platform` arguments.

Requirements file is ready by leveraging its constraint syntax I believe. Like shown below.

Excerpt from our *requirements.txt* generated with [pip-compile-cross-platform](https://pypi.org/project/pip-compile-cross-platform/):

```python
...
mkl==2021.4.0 ; python_version >= "3.12" and python_version < "4.0" and platform_system == "Windows"
...
nvidia-nvtx-cu12==12.1.105 ; platform_system == "Linux" and platform_machine == "x86_64" and python_version >= "3.12" and python_version < "4.0"
...
tbb==2021.12.0 ; python_version >= "3.12" and python_version < "4.0" and platform_system == "Windows"
...
```

If you would support it, we could use `uv` **everywhere** & for **everything**:)

---

Thx for your contribution to our python community 🚀

---

_Label `needs-design` added by @charliermarsh on 2024-05-02 14:31_

---

_Comment by @hauntsaninja on 2024-05-06 07:48_

You might be interested in the script in this issue: https://github.com/astral-sh/uv/issues/2679#issuecomment-2068208602

---

_Comment by @dimostenis on 2024-05-06 15:45_

This is excellent. However I needed to add `platform_machine` (`x86_64` / `aarch64`) to the mix and then it breaks (as it breaks our venvs without it in original post). **Maybe correctly**.

Look at the tail of my output.

```python
...
...
nvidia-nvtx-cu12==12.1.105; platform_system == "linux" and platform_machine == "x86_64"
tbb==2021.12.0; platform_system == "win32" and platform_machine == "x86_64"
torch==2.3.0; (platform_system == "win32" and platform_machine == "x86_64") or (platform_system == "darwin" and platform_machine == "aarch64") or (platform_system == "linux" and platform_machine == "x86_64") or (platform_system == "linux" and platform_machine == "aarch64")
torch==2.2.2; platform_system == "darwin" and platform_machine == "x86_64"
uvloop==0.19.0; (platform_system == "darwin" and platform_machine == "aarch64") or (platform_system == "darwin" and platform_machine == "x86_64") or (platform_system == "linux" and platform_machine == "x86_64") or (platform_system == "linux" and platform_machine == "aarch64")
```

Mind doubled `torch`, which effectively leads to ignoring `torch` on any other arch than `"darwin" + "x86_64"`.

I mean - it may actually be correct, as I am combining 5 requirements architectures (trying to cover company's dev machines + cloud):

- `x86_64-pc-windows-msvc`
- `aarch64-apple-darwin`
- `x86_64-apple-darwin`
- `x86_64-manylinux_2_28`
- `aarch64-manylinux_2_28`

If I go with `pip-compile-cross-platform`, it goes like `torch==2.3.0 ; python_version >= "3.12" and python_version < "4.0"` for all archs. And it works, but honestly I cant try it for `darwin/x86_64` 🙈.

As said above - maybe this is correct that there is no unified env across all archs. But if it could, it would be nice if `uv` could handle it.

---

## Reproducing my steps

### Env requirements

These are *requirements.in*

```
boto3
colorama
fastapi
flair
gunicorn
httpx
jarowinkler
jsonschema
orjson
pandas
pre-commit
pydantic
pydantic-settings
pytest
python-json-logger
rapidfuzz
scipy<1.13  # gensim uses scipy.triu which is in 1.13, gensim fixed that but did not publish new release yet
snowflake-connector-python[pandas]
torch
transformers
```

### Modified version of https://github.com/astral-sh/uv/issues/2679#issuecomment-2068208602

Because it was missing `platform_machine` and that did not work with our venv. Specifically all `nvidia-...` packages are `x86_64` only.

`scripts/env.py`

```python
"""
Based on https://github.com/astral-sh/uv/issues/2679#issuecomment-2068208602
"""

import argparse
import copy
import subprocess
import sys
import tempfile
from collections import defaultdict
from functools import cache
from pathlib import Path
from typing import Literal
from typing import TypedDict

from packaging.markers import Marker
from packaging.requirements import Requirement

PYTHON_VERSION = ".".join(map(str, sys.version_info[:2]))  # eg. 3.12

type Arch = Literal[
    "x86_64-pc-windows-msvc",
    "aarch64-apple-darwin",
    "x86_64-apple-darwin",
    "x86_64-manylinux_2_28",
    "aarch64-manylinux_2_28",
]


class Platform(TypedDict):
    platform_machine: Literal["x86_64", "aarch64"]
    platform_system: Literal["windows", "darwin", "linux"]


# company dev machines + k8s pods
ENVIRONMENTS: dict[Arch, Platform] = {
    "x86_64-pc-windows-msvc": {
        "platform_machine": "x86_64",
        "platform_system": "windows",
    },
    "aarch64-apple-darwin": {
        "platform_machine": "aarch64",
        "platform_system": "darwin",
    },
    "x86_64-apple-darwin": {
        "platform_machine": "x86_64",
        "platform_system": "darwin",
    },
    "x86_64-manylinux_2_28": {
        "platform_machine": "x86_64",
        "platform_system": "linux",
    },
    "aarch64-manylinux_2_28": {
        "platform_machine": "aarch64",
        "platform_system": "linux",
    },
}


@cache
def arch_to_marker(arch: Arch) -> str:
    return (
        f'platform_system == "{ENVIRONMENTS[arch]["platform_system"]}"'
        f' and platform_machine == "{ENVIRONMENTS[arch]["platform_machine"]}"'
        # f' and python_version == "{PYTHON_VERSION}"'
    )


def archs_to_marker(archs: list[Arch]) -> Marker:
    return Marker(" or ".join(f"({arch_to_marker(env)})" for env in archs))


def strip_comments(s: str) -> str:
    if "#" in s:
        return s[: s.index("#")].strip()
    return s.strip()


def parse_requirements_txt(req_file: Path) -> list[str]:
    entries = []
    for line in req_file.read_text().split("\n"):
        entry = strip_comments(line)
        if entry:
            entries.append(entry)
    return entries


def cross_environment_lock(src_file: Path, system_python: bool) -> str:
    environments: tuple[Arch, ...] = tuple(ENVIRONMENTS.keys())

    cmd: list[str] = [
        "uv",
        "pip",
        "compile",
        "--no-header",
        "--no-annotate",
        "--python-version",
        PYTHON_VERSION,
    ]

    if system_python:
        cmd.append("--system")  # because I run it in container
    else:
        cmd.extend(["--python", sys.executable])  # force venv use

    with tempfile.TemporaryDirectory() as tmpdir:
        joined: dict[Requirement, list[Arch]] = defaultdict(list)
        for i, environment in enumerate(environments):
            out_file = Path(tmpdir) / f"{src_file}.{i}"
            env_cmd: list[str] = [
                *cmd,
                str(src_file),
                "--output-file",
                str(out_file),
                "--python-platform",
                environment,
            ]
            subprocess.check_call(env_cmd, stdout=subprocess.DEVNULL)

            for r in parse_requirements_txt(out_file):
                joined[Requirement(r)].append(environment)

        common = [r for r, envs in joined.items() if len(envs) == len(environments)]
        cross_environment = [
            (r, envs) for r, envs in joined.items() if len(envs) != len(environments)
        ]
        cross_environment.sort(key=lambda r: r[0].name)

        output = [str(r) for r in common]
        for _req, archs in cross_environment:
            req = copy.copy(_req)  # make a copy
            joint_marker = archs_to_marker(archs=archs)
            if req.marker is None:
                req.marker = joint_marker
            else:
                # Note that uv currently doesn't preserve markers, so this branch is unlikely
                # https://github.com/astral-sh/uv/issues/1429
                req.marker._markers = [
                    req.marker._markers,
                    "and",
                    joint_marker._markers,
                ]
            output.append(str(req))

        return "\n".join(output)


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("src_file")
    parser.add_argument("--output-file")
    parser.add_argument(
        "--system-python",
        help="Dont check if in venv.",
        action="store_true",
    )
    args = parser.parse_args()

    output = cross_environment_lock(
        src_file=Path(args.src_file),
        system_python=args.system_python,
    )
    print(output)
    if args.output_file:
        Path(args.output_file).write_text(output)


if __name__ == "__main__":
    main()
```

And I run it inside container (to try different archs):

```sh
python -m scripts.env requirements.in --system-python
```


---

_Comment by @zanieb on 2024-05-06 16:04_

This is an interesting idea. I worry that it doesn't make sense for us to prioritize when we're developing a dedicated cross-platform lock file:

- https://github.com/astral-sh/uv/issues/3350

---

_Comment by @dimostenis on 2024-05-06 16:31_

Oh. I missed that. That should solve it - using `uv` for cross platform reqs generation 👌🏼

Hope in my example case it will pick single version of `torch` for all archs tho (on same python of course).

I guess we can close this issue.
Thanks.

---

_Closed by @dimostenis on 2024-05-06 16:31_

---

_Comment by @zanieb on 2024-05-06 16:34_

cc @BurntSushi to note that request :)

---

_Referenced in [aws/bedrock-agentcore-starter-toolkit#361](../../aws/bedrock-agentcore-starter-toolkit/pulls/361.md) on 2025-11-20 20:39_

---
