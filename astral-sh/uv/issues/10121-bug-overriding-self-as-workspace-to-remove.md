---
number: 10121
title: "bug: Overriding self as workspace (to remove constraints on self from dependencies) prevents uv from installing dev-deps"
type: issue
state: open
author: pawamoy
labels:
  - bug
assignees: []
created_at: 2024-12-23T14:47:25Z
updated_at: 2024-12-26T14:30:24Z
url: https://github.com/astral-sh/uv/issues/10121
synced_at: 2026-01-10T01:24:50Z
---

# bug: Overriding self as workspace (to remove constraints on self from dependencies) prevents uv from installing dev-deps

---

_Issue opened by @pawamoy on 2024-12-23 14:47_

Follow-up of https://github.com/astral-sh/uv/issues/8148.

```toml
[build-system]
requires = ["pdm-backend"]
build-backend = "pdm.backend"

[project]
name = "repro-uv-8148-a"
description = ""
authors = [{name = "Timothée Mazzucotelli", email = "dev@pawamoy.fr"}]
license = {text = "ISC"}
readme = "README.md"
requires-python = ">=3.9"
keywords = []
dynamic = ["version"]
dependencies = []

[tool.pdm]
version = {source = "scm"}

[tool.pdm.build]
package-dir = "src"

[dependency-groups]
dev = [
    "repro-uv-8148-b>=0.1",
]

[tool.uv]
# Tell uv to ignore constraints on the main package.
# This is needed when the current project doesn't have Git tags (fork, CI).
override-dependencies = ["repro-uv-8148-a"]
sources = { repro-uv-8148-a = { workspace = true } }
```

When I tested the solution to #8148 by installing uv from sources, running `uv sync` in https://github.com/pawamoy-forks/repro-uv-8148-a was correctly installing the development dependencies, i.e. `repro-uv-8148-b`.

I just upgraded from my version installed from sources (0.5.4+) to uv 0.5.11, and dev-deps aren't installed anymore.

To reproduce:

```bash
git clone git@github.com:pawamoy-forks/repro-uv-8148-a
cd repro-uv-8148-a
uv sync
```

Observe that the development dependency `repro-uv-8148-b` was not installed in `.venv`.

Logs:

```console
% uv sync --verbose        
DEBUG uv 0.5.11
DEBUG Found project root: `/media/data/dev/repro-uv-8148-a`
DEBUG No workspace root found, using project root
DEBUG Using Python request `>=3.9` from `requires-python` metadata
DEBUG Searching for Python >=3.9 in managed installations or search path
DEBUG Searching for managed installations at `/home/pawamoy/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0rc3-linux-x86_64-gnu`
DEBUG Found `cpython-3.13.0rc3-linux-x86_64-gnu` at `/home/pawamoy/.local/share/uv/python/cpython-3.13.0rc3-linux-x86_64-gnu/bin/python3.13` (managed installations)
DEBUG Skipping pre-release cpython-3.13.0rc3-linux-x86_64-gnu
DEBUG Found managed installation `cpython-3.11.7-linux-x86_64-gnu`
DEBUG Found `cpython-3.11.7-linux-x86_64-gnu` at `/home/pawamoy/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11` (managed installations)
Using CPython 3.11.7
Creating virtual environment at: .venv
DEBUG Assessing Python executable as base candidate: /home/pawamoy/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11
DEBUG Using base executable for virtual environment: /home/pawamoy/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11
DEBUG Using request timeout of 30s
DEBUG No static `pyproject.toml` available for: repro-uv-8148-a @ file:///media/data/dev/repro-uv-8148-a (PyprojectToml(DynamicField("version")))
DEBUG Acquired lock for `/media/data/.cache/uv/sdists-v6/editable/7fd03d4f10b5ac48`
DEBUG Using cached metadata for: repro-uv-8148-a @ file:///media/data/dev/repro-uv-8148-a
DEBUG No workspace root found, using project root
DEBUG Released lock at `/media/data/.cache/uv/sdists-v6/editable/7fd03d4f10b5ac48/.lock`
DEBUG Solving with installed Python version: 3.11.7
DEBUG Solving with target Python version: >=3.9
DEBUG Adding direct dependency: repro-uv-8148-a*
DEBUG Adding direct dependency: repro-uv-8148-a*
DEBUG Searching for a compatible version of repro-uv-8148-a @ file:///media/data/dev/repro-uv-8148-a (*)
DEBUG Tried 1 versions: repro-uv-8148-a 1
DEBUG all marker environments resolution took 0.000s
Resolved 1 package in 1ms
DEBUG Using request timeout of 30s
DEBUG Directory source requirement already cached: repro-uv-8148-a==0.1.dev5+g9507e17.d20241223 (from file:///media/data/dev/repro-uv-8148-a)
Installed 1 package in 0.66ms
 + repro-uv-8148-a==0.1.dev5+g9507e17.d20241223 (from file:///media/data/dev/repro-uv-8148-a)
```

**This behavior started in 0.5.7.**

Related:

- https://github.com/astral-sh/uv/pull/9455

Possibly related:

- https://github.com/astral-sh/uv/pull/9716
- https://github.com/astral-sh/uv/pull/9717
- https://github.com/astral-sh/uv/pull/9705
- https://github.com/astral-sh/uv/pull/9714

---

_Comment by @charliermarsh on 2024-12-23 15:10_

I honestly don't know if what you're doing here should work. I don't know if a package should be able to override itself.

---

_Comment by @charliermarsh on 2024-12-23 15:15_

I think I have some idea of what's going wrong here though.

---

_Comment by @pawamoy on 2024-12-23 15:22_

I don't know either! But I mean... why not? We have this escape hatch for dependencies, so when the project itself is a dependency of a dependency, we should be able to override it too 🤷 I override self because that's the only thing I can do here, but this could be called differently, and done differently by uv, something like --if-the-only-conflict-is-on-my-project-install-it-anyway 😂 

---

_Comment by @charliermarsh on 2024-12-23 15:23_

I think there _is_ a bug here, which we'll fix, I'm just not sure I want to call this officially supported :D

---

_Comment by @pawamoy on 2024-12-23 15:23_

Understood 😄 🤞 

---

_Comment by @pawamoy on 2024-12-23 15:26_

I'm actually surprised I'm the only one reporting this. Must be the combo self-cyclic-dep + dynamic-version + no-tags 😅 

---

_Comment by @charliermarsh on 2024-12-23 15:31_

I think it's specifically about overriding a workspace member that has dependency groups.

---

_Comment by @charliermarsh on 2024-12-23 15:32_

I think it's just super uncommon to use an override with a workspace member?

---

_Comment by @pawamoy on 2024-12-23 15:45_

Probably. That's just the only way I get to fix this issue 😕 The ideal solution would require of level of communication between the frontend and the backend that is not currently possible:

- frontend verifies constraints as usual
- but if and only if the backend fails to compute the right version for the current project due to missing SCM data, then ignore the current project version (consider the constraints satisfied)

---

_Comment by @pawamoy on 2024-12-23 15:48_

I could definitely just document in my CONTRIBUTING files that contributors *must* pull tags, but that's equally weird IMO, I've never seen any such requirement 😅 Not hard per-se, just unusual for contributors.

```bash
git clone ...; cd ...
git remote add upstream https://...
git pull upstream --tags
```

Heh, maybe I could put these into my `make setup` command (somehow detecting that `origin` isn't the main repo).

---

_Comment by @pawamoy on 2024-12-23 15:53_

Honestly, feel free to close and reconsider if more people chime in. I've bothered you enough with this 🙇 

---

_Comment by @pawamoy on 2024-12-23 22:54_

Hey Charlie, I found my brain and crafted a pdm-backend solution instead:


```toml
[tool.pdm.version]
source = "call"
getter = "scripts.get_version:get_version"
```

```python
# scripts/get_version.py
import re
from pathlib import Path

from pdm.backend.hooks.version import Version, SCMVersion, get_version_from_scm, default_version_formatter

_root = Path(__file__).parent.parent
_changelog = _root / "CHANGELOG.md"
_changelog_version_re = re.compile(r"^## \[(\d+\.\d+\.\d+)\].*$")
_default_scm_version = SCMVersion(Version("0.0.0"), None, False, None, None)


def get_version():
    scm_version = get_version_from_scm(_root) or _default_scm_version
    if scm_version.version <= Version("0.1"):  # Missing Git tags?
        if _changelog.exists():
            with _changelog.open() as file:
                for line in file:
                    if match := _changelog_version_re.match(line):
                        scm_version = scm_version._replace(version=Version(match.group(1)))
                        break
    return default_version_formatter(scm_version)
```

Basically, it's dynamic versioning based on SCM + falling back onto grepping the CHANGELOG.md file.

I will let you close this since you mentioned you found an issue, but consider this resolved on my end 🙂

Thank you so much for your patience, your kind responses, and your help 💟 

---

_Label `bug` added by @charliermarsh on 2024-12-26 14:30_

---

_Referenced in [astral-sh/uv#8148](../../astral-sh/uv/issues/8148.md) on 2025-02-21 15:44_

---
