---
number: 7785
title: Support getting the project version (like Rye and poetry) from the pyproject.toml
type: issue
state: closed
author: itamark-ug
labels:
  - duplicate
assignees: []
created_at: 2024-09-29T20:42:44Z
updated_at: 2025-02-19T15:33:30Z
url: https://github.com/astral-sh/uv/issues/7785
synced_at: 2026-01-07T13:12:17-06:00
---

# Support getting the project version (like Rye and poetry) from the pyproject.toml

---

_Issue opened by @itamark-ug on 2024-09-29 20:42_

Hi,
In both poetry and Rye there is an option to run `poetry version` or `rye version` and get the project version from the pyproject.toml.

Here is a link to the rye docs:
https://rye.astral.sh/guide/commands/version/

Here is a link to the poetry docs:
https://python-poetry.org/docs/cli/#version

for example, for a project named my-little-helper with a version 2.0.22 the command will emit:
`my-little-helper 2.0.22`
In poetry there is also an option to add the --short option and the result will be `2.0.22`

The motivation for that is to be able to extract the version for automatic tagging a successful package build in a github action.
But I can think of other useful scenarios.

Thanks

---

_Comment by @danyeaw on 2024-10-05 19:29_

I agree that this would be really useful. A workaround to get the version could be:

```
uv tool install pyproject-parser[cli]
pyproject-info project.version
```

---

_Comment by @itamark-ug on 2024-10-06 08:35_

> I agree that this would be really useful. A workaround to get the version could be:
> 
> ```
> uv tool install pyproject-parser[cli]
> pyproject-info project.version
> ```

Thanks, we are already using it in our code, for now (till uv will support it) I will use your workaround in our build action. Thanks 

---

_Comment by @zanieb on 2024-10-06 14:42_

Duplicate of https://github.com/astral-sh/uv/issues/6298

---

_Closed by @zanieb on 2024-10-06 14:42_

---

_Label `duplicate` added by @zanieb on 2024-10-06 14:42_

---

_Comment by @zanieb on 2024-10-06 14:42_

We're interested, but need to design the interface.

---

_Comment by @david-waterworth on 2024-10-08 01:39_

Note `pyproject-info project.version` returns the version with the surrounding quotes, I used `pyproject-info project.version | xargs` to remove them.

---

_Comment by @magnus-bakke on 2025-02-19 15:33_

> I agree that this would be really useful. A workaround to get the version could be:
> 
> ```
> uv tool install pyproject-parser[cli]
> pyproject-info project.version
> ```

For anyone else trying this and getting: `BadConfigError: Unexpected top-level key 'dependency-groups'. Only 'build-system', 'project' and 'tool' are allowed.`

Use a later tag from https://github.com/repo-helper/pyproject-parser/tags
This worked for me:

```
uv tool install pyproject-parser[cli]>=v0.13.0b1
pyproject-info project.version
```


---

_Referenced in [astral-sh/uv#6298](../../astral-sh/uv/issues/6298.md) on 2025-05-18 21:53_

---
