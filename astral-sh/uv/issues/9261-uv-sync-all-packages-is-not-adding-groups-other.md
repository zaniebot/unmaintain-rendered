```yaml
number: 9261
title: "\"uv sync --all-packages\" is not adding groups other than dev"
type: issue
state: closed
author: inoa-jboliveira
labels: []
assignees: []
created_at: 2024-11-20T03:07:47Z
updated_at: 2024-11-20T03:27:13Z
url: https://github.com/astral-sh/uv/issues/9261
synced_at: 2026-01-12T15:59:45Z
```

# "uv sync --all-packages" is not adding groups other than dev

---

_@inoa-jboliveira_

According to [docs](https://docs.astral.sh/uv/reference/cli/#uv-sync), `--all-packages` should

> Sync all packages in the workspace.

I thought this meant **all** dependency-groups too. I recently moved from extras to the new groups which seemed to suit my needs better but then I did not realize it was removing packages in other dependency groups we have with special meaning.

See below pip was added in group build

```
$ cat pyproject.toml
[project]
name = "bugsync"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "numpy>=2.1.3",
]

[dependency-groups]
dev = [
    "pytest>=8.3.3",
]
build = [
    "pip>=24.3.1",
]
``` 

Explicit sync of group build adds it, but --all-packages removes it!

```
$ uv sync --group build
Resolved 8 packages in 0.85ms
Installed 1 package in 101ms
 + pip==24.3.1

$ uv sync --all-packages
Resolved 8 packages in 0.83ms
Uninstalled 1 package in 30ms
 - pip==24.3.1
```


So is it ALL packages or really some packages? And how do I enforce all the packages across extras and groups passing defining each group in the sync command?

$ uv version
uv 0.5.3 (56d362208 2024-11-19)


---

_Comment by @charliermarsh on 2024-11-20 03:09_

`--all-packages` syncs all the _workspace members_, similar to how `--package` syncs a specific workspace member.

---

_Comment by @inoa-jboliveira on 2024-11-20 03:17_

Thank you for swift reply! My mistake, I had no idea workspace meant something else in this context. This command seemed to be introduced at the same time as dependency groups and in my head was the more complete version of "--all-extras" to include also groups.

I believe --all-packages is not a great name for something like this, but I digress 

How can one sync all the dependencies including extras and groups of an app?

---

_Comment by @inoa-jboliveira on 2024-11-20 03:20_

I think I saw somewhere a discussion about a "--all" flag, but I can´t find it anymore nor the option exists

---

_Comment by @charliermarsh on 2024-11-20 03:23_

Yeah we plan to rename it to `--all-members` in the future.

`--all-extras` exists, but `--all-groups` is still in PR (#8892).

---

_Comment by @inoa-jboliveira on 2024-11-20 03:27_

Thank you. --all-groups would be enough for me, as I could combine it with --all-extras

The #8892 PR seems a bit stale, but hopefully it gets merged.

I will close this issue since it is a duplicate of #8594 then

---

_Closed by @inoa-jboliveira on 2024-11-20 03:27_

---
