---
number: 8406
title: "uv build failing to build wheel correctly when project.name != package folder name"
type: issue
state: closed
author: david-waterworth
labels:
  - question
assignees: []
created_at: 2024-10-21T04:48:34Z
updated_at: 2024-10-22T15:54:35Z
url: https://github.com/astral-sh/uv/issues/8406
synced_at: 2026-01-10T01:24:28Z
---

# uv build failing to build wheel correctly when project.name != package folder name

---

_Issue opened by @david-waterworth on 2024-10-21 04:48_

I have a project with a name (i.e. foo) that's not the same as the source folder / package name (`src/bar`). 

```
[project]
name = "foo"
version = "1.2.3"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/bar"]

[tool.hatch.build.targets.sdist]
packages = ["src/bar"]
```

If I run `uvx hatch build` I get a wheel and sdist that contain the files I expect - i.e. the wheel `{project.name}-{version}-py3-none-any.whl`  contains

```
{project.name}-{version}.distinfo/
    METADATA
    RECORD
    WHEEL
bar/
     {contents of src/bar}
```

But if I run `uv build` the sdist is correct, but the wheel only contains the project metadata - i.e. the contents of the archive `{project.name}-{version}-py3-none-any.whl` is

```
{project.name}-{version}.distinfo/
    METADATA
    RECORD
    WHEEL
```
i.e. `bar/` is missing

It works if I set the project name the same as the packages folder. I would expect that `uv build` and `uvx hatch build` would behave exactly the same?

I'm using uv v0.4.24


---

_Comment by @david-waterworth on 2024-10-21 07:42_

The sdist  works fine, the wheel is built correctly when I use hatch but not uv with the same pyproject file. Both have the same configSent from my iPhoneOn 21 Oct 2024, at 6:37 pm, bluss ***@***.***> wrote:﻿
Could you create the sdist and check if it has the contents you expect. I think it's missing files because of your sdist config.

—Reply to this email directly, view it on GitHub, or unsubscribe.You are receiving this because you authored the thread.Message ID: ***@***.***>

---

_Comment by @konstin on 2024-10-21 08:43_

Could this be another case of https://github.com/astral-sh/uv/pull/8220? What version of uv are you using?

---

_Comment by @charliermarsh on 2024-10-21 14:49_

I assume `uv build --wheel` and `uv build --sdist` produce the right artifacts? (That's not the same as running `uv build`, which builds the wheel from the sdist.)

---

_Comment by @david-waterworth on 2024-10-21 21:54_

@charliermarsh yes it does
@konstin  v0.4.24 - (and that link explains why  .gitignore keeps appearing in that folder...)

---

_Comment by @charliermarsh on 2024-10-21 22:08_

Does `python -m build` work on your project, or does it produce similar builds? (https://github.com/pypa/build)

---

_Comment by @david-waterworth on 2024-10-21 22:56_

@charliermarsh no that does the same thing as `uv build` and only includes the metadata in the wheel, but the source is included in the sdist

---

_Comment by @charliermarsh on 2024-10-21 23:25_

@david-waterworth -- You may have a real issue with your project then. `pypa/build` is effectively the reference implementation for the spec. My guess is that `uvx hatch build` builds the wheel and the sdist from source, rather than building the wheel from the sdist as is the default in `pypa/build` and `uv`. You'll either need to figure out why your sdist isn't working as expected or use `uv build --sdist --wheel` (but, again, your sdist may have a real problem?).

---

_Label `question` added by @charliermarsh on 2024-10-21 23:25_

---

_Comment by @david-waterworth on 2024-10-22 00:58_

[foo.zip](https://github.com/user-attachments/files/17468438/foo.zip)

I created a minimal example - maybe there is something wrong with the sdist, I've not actually tried to install it. Also https://hatch.pypa.io/latest/config/build/#packages doesn't appear to explicitly cover the case where project.name doesn't equal package name - maybe it's not supported, the accepted answer to https://stackoverflow.com/questions/77070305/how-to-distribute-a-python-package-where-import-name-is-different-than-project-n was to switch to setuptools?

---

_Comment by @david-waterworth on 2024-10-22 01:02_

Adding

```
[tool.hatch.build.targets.wheel]
packages = ["src/bar"]
include = [
  "src/bar/**",
]
```

Seems to work

---

_Comment by @bluss on 2024-10-22 04:51_

The config `tool.hatch.build.targets.sdist.packages` looks out of place to me, and without it, I think it's doing the right thing?

"""
The packages option is semantically equivalent to only-include (which takes precedence) except that the shipped path will be **collapsed** to only include the final component.
"""

When it is used on sdist, `src/bar` from the original source becomes `bar` in the sdist. When you build a wheel from that, there is no `src` anymore, so the wheel config becomes incorrect too?  (The right way -> just omit the sdist config or change it to not use `packages`).

A thing to keep in mind here is that there are two workflows that are both used. One is source -> sdist -> wheel and the other is source -> sdist and source -> wheel.  `hatch build` may well be using a different default than `build` and `uv build` - just check all their settings. They can all do one or the other, but source -> sdist -> wheel should be preferred. And it has basically caught an error in your sdist construction here for you, which is why wheels should be built that way..

---

_Comment by @charliermarsh on 2024-10-22 15:54_

I'm going to close for now as I don't _think_ it's uv-specific.

---

_Closed by @charliermarsh on 2024-10-22 15:54_

---
