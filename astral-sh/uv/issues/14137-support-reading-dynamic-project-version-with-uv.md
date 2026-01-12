```yaml
number: 14137
title: "Support reading dynamic project version with `uv version`"
type: issue
state: open
author: BartSchuurmans
labels:
  - enhancement
assignees: []
created_at: 2025-06-18T19:22:06Z
updated_at: 2025-09-14T11:39:59Z
url: https://github.com/astral-sh/uv/issues/14137
synced_at: 2026-01-12T16:01:43Z
```

# Support reading dynamic project version with `uv version`

---

_@BartSchuurmans_

### Summary

When using dynamic project versions, for instance with [versioningit](https://github.com/jwodder/versioningit) or [hatch-vcs](https://github.com/ofek/hatch-vcs), it would be really convenient to be able to read the current project version (the version that would be used when `uv build` is called) by calling `uv version`.



### Example

```toml
[build-system]
requires = ["hatchling", "versioningit"]
build-backend = "hatchling.build"

[project]
name = "my-example-project"
dynamic = ["version"]

[tool.hatch.version]
source = "versioningit"
```

Current behavior:
```
$ uv version
error: We cannot get or set dynamic project versions in: pyproject.toml
```

Desired behavior:
```
$ uv version
my-example-project 1.1.0.post5+gc57875e.d20250618
```

---

_Label `enhancement` added by @BartSchuurmans on 2025-06-18 19:22_

---

_Comment by @zanieb on 2025-06-18 19:59_

We can support this, but it will require invoking the build backend to get the version.

---

_Comment by @BartSchuurmans on 2025-06-18 20:16_

I've been scanning the code a bit; how hard would this be to implement for a new contributor?

---

_Comment by @zanieb on 2025-06-18 21:14_

Probably not easy, but I'm actually not really sure how you'd go about it yet. If you're interested, I'd recommend posting a brief implementation plan here first and we can let you know if it sounds like the right approach.

---

_Comment by @bdellegrazie on 2025-06-20 12:44_

related #11718 and #14037 

---

_Comment by @zanieb on 2025-06-20 13:31_

@bdellegrazie note both of those are feature requests for the uv build backend whereas here we're talking about reading dynamic versions from other build backends.

---

_Comment by @blueraft on 2025-06-20 15:49_

If the project is dynamic, I think we could build a Plan for the project and read the version from the Plan fields. Wdyt? 

---

_Comment by @Gankra on 2025-06-20 17:17_

...wait I'm rereading the code and does `uv version --frozen` accidentally already support this?

---

_Comment by @blueraft on 2025-06-20 17:23_

I thought the same initially but dynamic versions are not recorded in the lockfile. 😅

```
❯ uv version --frozen
error: Failed to find the foo's version in the frozen lockfile

❯ cat uv.lock
version = 1
revision = 2
requires-python = ">=3.13"

[[package]]
name = "foo"
source = { editable = "." }
```

---

_Comment by @charliermarsh on 2025-06-20 17:24_

(The _not_ recording is intentional.)

---

_Comment by @konstin on 2025-06-20 17:30_

With static versions, we can read `pyproject.toml`, know the version and know whether the lockfile is up to date. With dynamic versions, this is more complex: The version could have changed since we last called the build backend and we want to update, or we could use the cached version. This mismatch is why we don't record the version in the lockfile (https://github.com/astral-sh/uv/issues/7533). Calling the build backend is slow, so we usually try to avoid it. We can show the dynamic version numbers in `uv version`, though I'm not sure if we want to show the cached version, or if `uv version --package foo` should call the build backend and behave like a hypothetical `uv version --package foo --refresh-package foo`.

---

_Comment by @Gankra on 2025-06-20 17:36_

Given we already have a locked/frozen distinction, and `uv version 1.2.3` (set mode) already does a full lock+sync, I don't think it's too unreasonable for `uv version` (get mode) to invoke a build (sync?) if it has to... but if `--frozen` is passed we should probably pull out the cached METADATA in the .venv (or refuse to support it at all).

---

_Comment by @dorinclisu on 2025-07-09 17:26_

Workaround when using hatch: `$ uvx hatch version` 

---

_Comment by @jharb-mirakl on 2025-07-31 09:44_

Workaround when using pdm: `uvx pdm show --version`

---

_Comment by @johnslavik on 2025-08-18 11:30_

> Workaround when using hatch: `$ uvx hatch version`

Especially neat when combined with a plugin. I'm using `uvx --with hatch-vcs hatchling version` and have 

```toml
[tool.hatch.version]
source = "vcs"
```

In my `pyproject.toml`

---

_Comment by @UnoYakshi on 2025-09-14 11:29_

I understand most of the workarounds and proposal here were, are, and will be about Python packages development and ecosystem.

However, my case differs a bit. I'm developing an application (FastAPI/Django/Litestar/Robyn/whatever) and use python-semantic-release for automatic SemVer in CI. No build system (setuptools/Hatch/PDM) is involved whatsoever. The source of truth becomes a variable in a Python file:
```toml
[project]
name = "project_name"
dynamic = ["version"]

# ...

[tool.semantic_release]
version_variables = ["src/core/__version__.py:__version__"]
```

I presume it's not that critical for a non-[Python ]build software, but getting errors on `uv version` with provided setup disturbs me a bit, as a developer.

Do I do the versioning correct way? Or should I switch to use `pyproject.toml:version` instead (PEP-621)? Do I just then import it via `importlib.metadata.version` like so?

```toml
[tool.semantic_release]
version_toml = ["pyproject.toml:project.version"]
```

```python
# src/core/__version__.py

from importlib.metadata import PackageNotFoundError, version

try:
    __version__ = version('project_name')
except PackageNotFoundError:
    __version__ = '0.0.0'
``` 

---
