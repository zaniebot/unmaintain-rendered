```yaml
number: 9735
title: Very large lockfile and clean up
type: issue
state: closed
author: jbcdnr
labels:
  - bug
assignees: []
created_at: 2024-12-09T09:50:22Z
updated_at: 2025-02-18T12:44:14Z
url: https://github.com/astral-sh/uv/issues/9735
synced_at: 2026-01-12T15:59:57Z
```

# Very large lockfile and clean up

---

_@jbcdnr_

We noticed that our `uv.lock` file grows in size, up to 2.5MB and 35k lines with a lot of `resolution-markers` and the time to `uv lock` explodes to more than 30 min and sometimes OOM. We have a pretty bad hygien for our `pyproject.toml` with  a lot of `*` versions and some conflicts so the resolver might have a hard time.

We came up with a simple script to extract all the versions from the lockfile and copy paste them into pyproject.toml, then rerun `lock` and revert pyproject right after. This speeds up the follow up `lock` calls and make the lock file only 0.4MB big. Is it expected behavior? Is our cleanup script actually something that exists as part of the `uv` command?

```python
import pathlib
import re

content = pathlib.Path("uv.lock").read_text()

print("dependencies = [")
for package_version_match in re.finditer(
    r'name = "([^"]+)"\nversion = "([\d\.]+)"', content
):
    package_name, package_version = package_version_match.groups()
    print(f'    "{package_name}=={package_version}",')
print("]")
```

Thank you for your help.

---

_Comment by @konstin on 2024-12-09 09:54_

It's hard to say without the input pyproject.toml; A lot of `resolution-markers` sounds like there's a lot of resolver forking that resolves multiple versions that you don't need. One option would be pinning those packages that appear multiple times in the lockfile (to coerce them into a single version) or to set [`tool.uv.environments`](https://docs.astral.sh/uv/concepts/projects/config/#limited-resolution-environments).

---

_Comment by @jbcdnr on 2024-12-09 15:44_

Unfortunately I cannot share the whole pyproject.toml. Thanks for the pointer at environments, most of the lockfile resolution-markers entries were for darwin/linux, so resolving only for linux should avoid that this happens again.

I am still curious if there could be a way to clean up the lockfile like we did but in `uv`.

---

_Comment by @charliermarsh on 2024-12-09 18:17_

Does `resolution-markers` contain duplicates?

---

_Comment by @charliermarsh on 2024-12-09 22:32_

My guess is that `resolution-markers` somehow grows exponentially over time... Are you able to share, like, a subset of the `pyproject.toml`?

---

_Comment by @charliermarsh on 2024-12-09 22:40_

Separately, do you have any `conflicts` defined in your `pyproject.toml`?

---

_Label `needs-mre` added by @charliermarsh on 2024-12-09 22:40_

---

_Comment by @jbcdnr on 2024-12-12 09:09_

> Does `resolution-markers` contain duplicates?

Yes, it was containing many lines like
```
resolution-markers = [
    "sys_platform == 'linux'",
    "sys_platform == 'linux'",
    "sys_platform == 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "sys_platform == 'darwin'",
    "sys_platform == 'darwin'",
    "sys_platform == 'darwin'",
    "sys_platform == 'linux'",
    "sys_platform == 'linux'",
    "sys_platform == 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "sys_platform == 'darwin'",
    "sys_platform == 'darwin'",
    "sys_platform == 'darwin'",
    "sys_platform == 'linux'",
    "sys_platform == 'linux'",
    "sys_platform == 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "sys_platform == 'darwin'",
    "sys_platform == 'darwin'",
    "sys_platform == 'darwin'",
...
```

---

_Comment by @jbcdnr on 2024-12-12 09:12_

> Separately, do you have any conflicts defined in your pyproject.toml?

Yes, we have a conflict defined between two extra, `torch-cpu` and `torch-gpu` that follow the PyTorch guide. We also had a hack for to support PyTorch dependency https://github.com/astral-sh/uv/issues/9734 but this is fixed in `0.5.8`.




---

_Comment by @jbcdnr on 2024-12-12 09:15_

> My guess is that resolution-markers somehow grows exponentially over time... Are you able to share, like, a subset of the pyproject.toml?

Unfortunately I cannot, but I can run what you want on our project. Even though I am not sure I can reproduce the growing lock file again, I would have to add some random package to force resolution a couple of times.

---

_Comment by @jbcdnr on 2024-12-12 09:33_

@charliermarsh it looks like bumping to 0.5.8 and deleting our hack for accelerate https://github.com/astral-sh/uv/issues/9734 also simplifies the resolution markers in our uv.lock file. So maybe this issue is non reproducible at main.

---

_Comment by @bluepichu on 2025-02-06 04:19_

I'm also running into this issue on 0.5.27, and I think I have a fairly minimal reproduction consisting of only the PyTorch setup on an otherwise-empty project:

`pyproject.toml`:
```toml
[project]
name = "resolution-markers-for-days"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
cpu = [
    "torch>=2.6.0"
]
cu124 = [
    "torch>=2.6.0"
]

[tool.uv]
conflicts = [
    [
        { extra = "cpu" },
        { extra = "cu124" },
    ],
]

[tool.uv.sources]
torch = [
    { extra = "cpu", index = "pytorch-cpu" },
    { extra = "cu124", index = "pytorch-cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

If I run `uv lock`, the resulting `uv.lock` starts with the following:

```
version = 1
requires-python = ">=3.12"
resolution-markers = [
    "extra != 'extra-27-resolution-markers-for-days-cpu' and extra == 'extra-27-resolution-markers-for-days-cu124'",
    "sys_platform != 'darwin' and extra == 'extra-27-resolution-markers-for-days-cpu' and extra != 'extra-27-resolution-markers-for-days-cu124'",
    "sys_platform == 'darwin' and extra == 'extra-27-resolution-markers-for-days-cpu' and extra != 'extra-27-resolution-markers-for-days-cu124'",
    "extra != 'extra-27-resolution-markers-for-days-cpu' and extra != 'extra-27-resolution-markers-for-days-cu124'",
]
conflicts = [[
    { package = "resolution-markers-for-days", extra = "cpu" },
    { package = "resolution-markers-for-days", extra = "cu124" },
]]
```

If I change anything about this file (e.g., set version to `0.1.1`) and run `uv sync --extra cpu`, then the start becomes:

```
version = 1
requires-python = ">=3.12"
resolution-markers = [
    "extra != 'extra-27-resolution-markers-for-days-cpu' and extra == 'extra-27-resolution-markers-for-days-cu124'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "sys_platform != 'darwin' and extra == 'extra-27-resolution-markers-for-days-cpu' and extra != 'extra-27-resolution-markers-for-days-cu124'",
    "python_version < '0'",
    "python_version < '0'",
    "sys_platform == 'darwin' and extra == 'extra-27-resolution-markers-for-days-cpu' and extra != 'extra-27-resolution-markers-for-days-cu124'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "extra != 'extra-27-resolution-markers-for-days-cpu' and extra != 'extra-27-resolution-markers-for-days-cu124'",
]
conflicts = [[
    { package = "resolution-markers-for-days", extra = "cpu" },
    { package = "resolution-markers-for-days", extra = "cu124" },
]]
```

If I do it again (bump version and run `uv sync --extra cpu`), then the `"python_version < '0'"` markers keep duplicating:

```
version = 1
requires-python = ">=3.12"
resolution-markers = [
    "extra != 'extra-27-resolution-markers-for-days-cpu' and extra == 'extra-27-resolution-markers-for-days-cu124'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "sys_platform != 'darwin' and extra == 'extra-27-resolution-markers-for-days-cpu' and extra != 'extra-27-resolution-markers-for-days-cu124'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "sys_platform == 'darwin' and extra == 'extra-27-resolution-markers-for-days-cpu' and extra != 'extra-27-resolution-markers-for-days-cu124'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "python_version < '0'",
    "extra != 'extra-27-resolution-markers-for-days-cpu' and extra != 'extra-27-resolution-markers-for-days-cu124'",
]
conflicts = [[
    { package = "resolution-markers-for-days", extra = "cpu" },
    { package = "resolution-markers-for-days", extra = "cu124" },
]]
```

Every run adds an additional 10 `python_version` markers.

Notably, setting `tool.uv.environments` to restrict the platform to Linux causes all of them to go away:

`pyproject.toml`:
```diff
  [tool.uv]
  conflicts = [
      [
          { extra = "cpu" },
          { extra = "cu124" },
      ],
  ]
+ environments = [
+     "sys_platform == 'linux'",
+ ]
  
  [tool.uv.sources]
  torch = [
      { extra = "cpu", index = "pytorch-cpu" },
      { extra = "cu124", index = "pytorch-cu124" },
  ]
```

Beginning of resulting `uv.lock`:

```
version = 1
requires-python = ">=3.12"
resolution-markers = [
    "sys_platform == 'linux'",
]
supported-markers = [
    "sys_platform == 'linux'",
]
conflicts = [[
    { package = "resolution-markers-for-days", extra = "cpu" },
    { package = "resolution-markers-for-days", extra = "cu124" },
]]
```


---

_Comment by @charliermarsh on 2025-02-06 13:05_

Thank you for the reproduction!

---

_Label `needs-mre` removed by @konstin on 2025-02-06 13:58_

---

_Label `bug` added by @konstin on 2025-02-06 13:58_

---

_Assigned to @BurntSushi by @konstin on 2025-02-06 15:10_

---

_Closed by @BurntSushi on 2025-02-18 12:44_

---
