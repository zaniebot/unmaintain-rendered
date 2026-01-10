```yaml
number: 5200
title: Write migration guides
type: issue
state: open
author: zanieb
labels:
  - documentation
assignees: []
created_at: 2024-07-19T00:40:41Z
updated_at: 2025-08-13T08:46:12Z
url: https://github.com/astral-sh/uv/issues/5200
synced_at: 2026-01-10T03:32:44Z
```

# Write migration guides

---

_Issue opened by @zanieb on 2024-07-19 00:40_

e.g from Poetry and pip-tools to uv

---

_Label `documentation` added by @zanieb on 2024-07-19 00:40_

---

_Comment by @chrisrodrigue on 2024-07-23 14:28_

`pdm` is pretty similar to poetry but it could still be worth having one for it as well.

`pdm` supports defining scripts/commands in `pyproject.toml` so there might not be a direct translation of that feature (would be cool if `uv` could do that though!)

---

_Comment by @zanieb on 2024-07-23 14:42_

We want to do that eventually, we're trying to keep the scope down so we can get some stable features in peoples hands :)

I don't think we'll include a guide for every package manager out there, e.g., `pdm` is great but has ~2.5% of the monthly downloads of Poetry (1m vs 40m). In general, migration guides are a lot of work to maintain since we need to keep tabs on _other_ tools — I'd like to keep the scope small.

---

_Comment by @michaelmior on 2024-08-23 15:31_

Would be nice to have pipenv included here if possible. It's got around the same number of monthly downloads as pip-tools.

---

_Comment by @ucg8j on 2024-08-29 11:58_

@zanieb glad to see this issue created. Massive plus one on prioritising by monthly downloads. If there was a `poetry` migration guide I'd imagine you'd see quite an uptick in `uv` usage. Myself included! I want to get an idea of what is involved, common edge cases etc, before investing the time doing it.

---

_Comment by @wu-clan on 2024-08-29 15:32_

I'd like to express my thoughts.

So far it seems that `uv init --from-project <path>` can copy dependencies from an existing project

In fact, if I use this command, which in the vast majority of cases I believe I do in order to migrate the project to uv, then why add the ` ---from-project ` parameter?

I'm a fan of `pdm`, which determines whether to create or migrate by recognizing the current existence of `pyproject.toml`, which I think is great, and if this is implemented, the user can do a quick migration with `uv init`, but consider the other commands for compatibility: `uv init --lib`...

Please don't try to reject `pdm`, `fastapi`, `pydantic` all use it!


---

_Comment by @Mapiarz on 2024-11-11 14:32_

FWIW here's a small `poetry -> uv` migration guide that should get most people started. I do realize it's not good enough to slap in the docs. There are a couple of problems: it depends on pdm, dependencies of dependencies can change, the locking script I attach is far from perfect/complete and requires manual edits afterwards.

First, you need to convert your poetry compatible `pyproject.toml` to be compatible with uv. You can actually do it quite easily with `pdm` (originally suggested [here][1]).

 1. `uvx pdm import pyproject.toml`
 2. Remove all poetry sections in the pyproject (i.e. `[tool.poetry...]` sections)
 3. Replace
```
[tool.pdm.dev-dependencies]
dev = [
```
with
```
[tool.uv]
dev-dependencies = [
```

Done. Your `pyproject.toml` file is now compatible with UV and you can do `uv lock` or `uv sync`.

Having said that, it is likely that UV will resolve completely different versions of packages than what you already had in your `poetry.lock`. That doesn't have to be a problem, but can. It was a problem in my case.

To work around this, I used a small script to hardcode all the minor/patch versions of my packages in `pyproject.toml` before converting to uv compatible format. Then, after conversion, I would create the uv.lock with `uv lock`. Then, I'd undo the hardcoded versions in the original pyproject.toml and convert again. Because uv by default will prefer versions from the lock, you now have all your versions pinned as they were in poetry and mostly the same packages in the lock (dependencies of dependencies can be different). It should be safe to do `uv sync` without worrying about things breaking due to versions change.

Below is the script I used to hardcode minor/patch versions in pyproject.toml. It's not perfect and will create incorrect results for dependencies with extras/from git etc. but these are easy to tweak manually.

```python
from pathlib import Path

import toml

# Load poetry.lock and pyproject.toml
lockfile_path = Path("poetry.lock")
pyproject_path = Path("pyproject.toml")

# Parse the lock file
with lockfile_path.open("r") as lockfile:
    lock_data = toml.load(lockfile)

# Parse pyproject.toml
with pyproject_path.open("r") as pyproject_file:
    pyproject_data = toml.load(pyproject_file)

# Build a map of package names to their specific versions from poetry.lock
locked_versions = {package["name"]: package["version"] for package in lock_data["package"]}


# Helper function to update dependencies in a given section
def update_dependencies(dependencies):
    for dep_name, dep_constraint in dependencies.items():
        if dep_name in locked_versions:
            dependencies[dep_name] = locked_versions[dep_name]
            print(f"Updated {dep_name} to version {locked_versions[dep_name]}")
        else:
            print(f"{dep_name} not found in poetry.lock, skipping.")


# Update dependencies in pyproject.toml
update_dependencies(pyproject_data["tool"]["poetry"]["dependencies"])
update_dependencies(pyproject_data["tool"]["poetry"]["group"]["dev"]["dependencies"])

# Write the updated pyproject.toml
with pyproject_path.open("w") as pyproject_file:
    toml.dump(pyproject_data, pyproject_file)

print("pyproject.toml has been updated with specific versions.")
```


  [1]: https://x.com/tiangolo/status/1839686030007361803

---

_Comment by @philiporlando on 2024-11-24 20:40_

Thank you for opening this issue and for providing this initial poetry migration guide.

I'm unfortunately seeing the below error when trying to migrate [this package](https://github.com/philiporlando/fgdb_to_gpkg):

```bash
uvx pdm import pyproject.toml
[AttributeError]: 'Array' object has no attribute 'items'
```

Any insight is appreciated!

---

_Comment by @mkniewallner on 2024-12-26 21:03_

For those interested, I've just open-sourced https://github.com/mkniewallner/migrate-to-uv. It can migrate projects using either Poetry or Pipenv to uv, migrating project metadata, dependencies (keeping same version ranges), dependency groups, sources, markers, ...

You can try it on an existing project with:
```shell
uvx migrate-to-uv
```

More package managers could be supported in the future (`pip-tools`, `pip`, `setuptools`, ...), if there is interest for them.

---

_Comment by @charliermarsh on 2024-12-26 21:57_

Awesome, thanks for sharing @mkniewallner 

---

_Comment by @zanieb on 2025-03-05 19:33_

A couple related issues

- https://github.com/astral-sh/uv/issues/11987
- https://github.com/astral-sh/uv/issues/11986

---

_Comment by @johnthagen on 2025-03-21 18:34_

I just migrated an example project from Poetry to `uv`. In case any of my notes or changes are useful as a reference to someone:

- https://github.com/johnthagen/python-blueprint/issues/238
- https://github.com/johnthagen/python-blueprint/pull/255

---

_Comment by @dd-ssc on 2025-03-27 07:47_

I just googled `convert poetry to uv` --> [first hit](https://stackoverflow.com/q/79118841) --> [first answer](https://stackoverflow.com/a/79316385):

```
$ uvx migrate-to-uv
   ...
```

--> some manual tweaks --> done ✅

Not sure if this requires a migration guide 🤷🏻‍♂️

---

_Comment by @zanieb on 2025-06-04 16:41_

A pip migration guide is ready for review https://github.com/astral-sh/uv/pull/12382

---

_Comment by @gabkdlly on 2025-08-13 08:46_

Maybe also: pyenv to uv migration guide.

---
