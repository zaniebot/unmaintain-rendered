---
number: 10774
title: "BUG (?): resolving with lowest-direct resolution and different pins for project vs. dev dependencies"
type: issue
state: closed
author: znicholls
labels:
  - question
  - great writeup
assignees: []
created_at: 2025-01-20T09:34:27Z
updated_at: 2025-01-20T14:26:23Z
url: https://github.com/astral-sh/uv/issues/10774
synced_at: 2026-01-07T13:12:18-06:00
---

# BUG (?): resolving with lowest-direct resolution and different pins for project vs. dev dependencies

---

_Issue opened by @znicholls on 2025-01-20 09:34_

I thought this was related to #10755, but now I don't think it is.

In short, if you have a dependency that is 'loose' in the project's dependencies but then pinned as a dev dependency, resolving with different strategies doesn't seem to work as expected.

uv version: uv 0.5.11 (c4d0caaee 2024-12-19) (I just checked and this appears to still happen with uv 0.5.21 (3478c068b 2025-01-17))
operating system: MacOS M2 chip

## First example

<details><summary>`pyproject.toml`</summary>
<p>

```toml
# pyproject.toml
[project]
name = "mwe"
version = "0.1.0"
description = "Minimal working example to demonstrate resolving issue"
authors = [
    { name = "Zebedee Nicholls", email = "zebedee.nicholls@climate-energy-college.org" },
]
requires-python = ">=3.9"
dependencies = [
    "attrs>=22.1.0",
]

[dependency-groups]
docs = [
    "attrs==24.3.0",
]
```

</p>
</details> 

<details><summary>Command output</summary>
<p>

With the `--no-dev` flag and `--resolution lowest-direct`, the lowest version of attrs is nonetheless taken from the docs (i.e. dev) dependency group. I would have thought I should get attrs v22.1.0 here.

```sh
$ uv tree --resolution lowest-direct --no-dev
Using CPython 3.13.1 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Resolved 2 packages in 0.68ms
mwe v0.1.0
└── attrs v24.3.0  # should be attrs v22.1.0 no?
```

</p>
</details> 

<details><summary>Command output with verbose</summary>
<p>

```sh
$ uv tree --resolution lowest-direct --no-dev --verbose
DEBUG uv 0.5.11 (c4d0caaee 2024-12-19)
DEBUG Found workspace root: `/Users/znicholls/Desktop/uv-mwe`
DEBUG Adding current workspace member: `/Users/znicholls/Desktop/uv-mwe`
DEBUG Using Python request `>=3.9` from `requires-python` metadata
DEBUG Searching for Python >=3.9 in managed installations or search path
DEBUG Searching for managed installations at `/Users/znicholls/.local/share/uv/python`
DEBUG Found `cpython-3.13.1-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
Using CPython 3.13.1 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: mwe @ file:///Users/znicholls/Desktop/uv-mwe
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.1
DEBUG Solving with target Python version: >=3.9
DEBUG Adding direct dependency: mwe*
DEBUG Adding direct dependency: mwe:docs*
DEBUG Searching for a compatible version of mwe @ file:///Users/znicholls/Desktop/uv-mwe (*)
DEBUG Adding transitive dependency for mwe==0.1.0: mwe:docs==0.1.0
DEBUG Searching for a compatible version of mwe @ file:///Users/znicholls/Desktop/uv-mwe (*)
DEBUG Adding transitive dependency for mwe==0.1.0: attrs>=22.1.0
DEBUG Searching for a compatible version of mwe @ file:///Users/znicholls/Desktop/uv-mwe (==0.1.0)
DEBUG Adding transitive dependency for mwe==0.1.0: attrs>=24.3.0, <24.3.0+
DEBUG Found fresh response for: https://pypi.org/simple/attrs/
DEBUG Searching for a compatible version of attrs (>=24.3.0, <24.3.0+)
DEBUG Selecting: attrs==24.3.0 [compatible] (attrs-24.3.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/89/aa/ab0f7891a01eeb2d2e338ae8fecbe57fcebea1a24dbb64d45801bfab481d/attrs-24.3.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f2/bc/d817287d1aa01878af07c19505fafd1165cd6a119e9d0821ca1d1c20312d/attrs-22.1.0-py2.py3-none-any.whl.metadata
DEBUG Tried 2 versions: attrs 1, mwe 1
DEBUG all marker environments resolution took 0.001s
Resolved 2 packages in 1ms
mwe v0.1.0
└── attrs v24.3.0
```

</p>
</details> 

## An even more explicit example

Even if I explicitly exclude the docs group, the dependency still affects the output.

<details><summary>`pyproject.toml`</summary>
<p>

```toml
# pyproject.toml
[project]
name = "mwe"
version = "0.1.0"
description = "Minimal working example to demonstrate resolving issue"
authors = [
    { name = "Zebedee Nicholls", email = "zebedee.nicholls@climate-energy-college.org" },
]
requires-python = ">=3.9"
dependencies = [
    "attrs>=22.1.0",
]

[dependency-groups]
docs = [
    "attrs==24.3.0",
]
```

</p>
</details> 

<details><summary>Command output</summary>
<p>

With the `--no-group docs` flag and `--resolution lowest-direct`, the lowest version of attrs is nonetheless taken from the docs dependency group. I would have thought I should get attrs v22.1.0 here.

```sh
$ uv tree --resolution lowest-direct --no-group docs
Using CPython 3.13.1 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Resolved 2 packages in 0.44ms
mwe v0.1.0
└── attrs v24.3.0
```

</p>
</details> 

<details><summary>Command output with verbose</summary>
<p>

```sh
$ uv tree --resolution lowest-direct --no-group docs --verbose
DEBUG uv 0.5.11 (c4d0caaee 2024-12-19)
DEBUG Found workspace root: `/Users/znicholls/Desktop/uv-mwe`
DEBUG Adding current workspace member: `/Users/znicholls/Desktop/uv-mwe`
DEBUG Using Python request `>=3.9` from `requires-python` metadata
DEBUG Searching for Python >=3.9 in managed installations or search path
DEBUG Searching for managed installations at `/Users/znicholls/.local/share/uv/python`
DEBUG Found `cpython-3.13.1-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
Using CPython 3.13.1 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: mwe @ file:///Users/znicholls/Desktop/uv-mwe
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.1
DEBUG Solving with target Python version: >=3.9
DEBUG Adding direct dependency: mwe*
DEBUG Adding direct dependency: mwe:docs*
DEBUG Searching for a compatible version of mwe @ file:///Users/znicholls/Desktop/uv-mwe (*)
DEBUG Adding transitive dependency for mwe==0.1.0: mwe:docs==0.1.0
DEBUG Searching for a compatible version of mwe @ file:///Users/znicholls/Desktop/uv-mwe (*)
DEBUG Adding transitive dependency for mwe==0.1.0: attrs>=22.1.0
DEBUG Searching for a compatible version of mwe @ file:///Users/znicholls/Desktop/uv-mwe (==0.1.0)
DEBUG Adding transitive dependency for mwe==0.1.0: attrs>=24.3.0, <24.3.0+
DEBUG Found fresh response for: https://pypi.org/simple/attrs/
DEBUG Searching for a compatible version of attrs (>=24.3.0, <24.3.0+)
DEBUG Selecting: attrs==24.3.0 [compatible] (attrs-24.3.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f2/bc/d817287d1aa01878af07c19505fafd1165cd6a119e9d0821ca1d1c20312d/attrs-22.1.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/89/aa/ab0f7891a01eeb2d2e338ae8fecbe57fcebea1a24dbb64d45801bfab481d/attrs-24.3.0-py3-none-any.whl.metadata
DEBUG Tried 2 versions: attrs 1, mwe 1
DEBUG all marker environments resolution took 0.001s
Resolved 2 packages in 1ms
mwe v0.1.0
└── attrs v24.3.0
```

</p>
</details> 

---

_Renamed from "BUG: resolving with lowest-direct resolution and different pins for project vs. dev dependencies" to "BUG (?): resolving with lowest-direct resolution and different pins for project vs. dev dependencies" by @znicholls on 2025-01-20 09:34_

---

_Comment by @konstin on 2025-01-20 09:50_

uv resolves optional and non-optional dependencies together and saves that to `uv.lock`, so that sync with or without optional dependencies only adds or removes packages, but doesn't change the versions. This avoids combinatorial explosions with different `[dependency-groups]` and `[optional-dependencies]` and gives a concise lockfile. An alternative is `uv pip compile`. or if you specifically need different versions with different groups and `uv.lock`, you can try https://docs.astral.sh/uv/reference/settings/#conflicts.

---

_Label `question` added by @konstin on 2025-01-20 09:51_

---

_Label `great writeup` added by @konstin on 2025-01-20 09:51_

---

_Comment by @znicholls on 2025-01-20 10:21_

Ah that is very clever and makes total sense now that I read this. `uv pip compile` works exactly as expected:

```sh
$ uv pip compile pyproject.toml --resolution lowest-direct
Resolved 1 package in 1ms
# This file was autogenerated by uv via the following command:
#    uv pip compile pyproject.toml --resolution lowest-direct
attrs==22.1.0
    # via mwe (pyproject.toml)
```

---

_Comment by @znicholls on 2025-01-20 10:23_

Thanks for the quick reply!

---

_Closed by @charliermarsh on 2025-01-20 14:26_

---
