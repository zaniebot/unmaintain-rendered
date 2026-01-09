---
number: 14861
title: Inconsistent behavior with workspaces and folder names
type: issue
state: closed
author: anuraaga
labels:
  - bug
assignees: []
created_at: 2025-07-24T01:26:56Z
updated_at: 2025-07-24T01:39:44Z
url: https://github.com/astral-sh/uv/issues/14861
synced_at: 2026-01-07T13:12:19-06:00
---

# Inconsistent behavior with workspaces and folder names

---

_Issue opened by @anuraaga on 2025-07-24 01:26_

### Summary

I currently have a workspace member in a folder called `example`, whose name I wanted to make less generic so named it `connecpy-example`.

https://github.com/i2y/connecpy/blob/a39928e99fdb7859c7557016db230408ad4ccde5/pyproject.toml#L56
https://github.com/i2y/connecpy/blob/a39928e99fdb7859c7557016db230408ad4ccde5/example/pyproject.toml#L2
https://github.com/i2y/connecpy/blob/a39928e99fdb7859c7557016db230408ad4ccde5/pyproject.toml#L23

This worked fine and I thought it meant there is decoupling between the name for the purposes of dependency declaration and the folder name. But when adding a new sibling to that project with a dependency on `connecpy-example`, it would not resolve.

```
❯ uv lock
  × No solution found when resolving dependencies:
  ╰─▶ Because connnecpy-example was not found in the package registry and connecpy-noextras depends on connnecpy-example, we can conclude that connecpy-noextras's requirements are
      unsatisfiable.
      And because your workspace requires connecpy-noextras, we can conclude that your workspace's requirements are unsatisfiable.
```

I decided to try changing the name of the project to match the folder name and surprisingly it worked fine.

https://github.com/anuraaga/connecpy/commit/3314c6eed829a627c5438d8fa10232476600193a#diff-a67e6a454244e74b80a1e3267d3472c63d4cdfd3eb295089d55f68c9f081d19dR2
https://github.com/anuraaga/connecpy/commit/3314c6eed829a627c5438d8fa10232476600193a#diff-f01a14c1c812435193b85c0ab8d513c9ef763e57182f057d84be540b7ed96066R7

If the project name has to match the folder name, that's completely fine but it seems there is some inconsistent behavior, where it sometimes works to have them not match and sometimes not.

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.8.2 (Homebrew 2025-07-22)

### Python version

Python 3.12.6

---

_Label `bug` added by @anuraaga on 2025-07-24 01:26_

---

_Referenced in [i2y/connecpy#61](../../i2y/connecpy/pulls/61.md) on 2025-07-24 01:29_

---

_Comment by @charliermarsh on 2025-07-24 01:30_

`members` takes a list of paths, so `members` have to match the directory name.

`[tool.uv.sources]` is keyed on package name.

So if the folder is `example` but the package name is `connecpy-example`, I'd expect that you'd need to put `example` (i.e., `./example`) in `members`, but `connecpy-example` in `dependencies` and `tool.uv.sources`.

---

_Comment by @anuraaga on 2025-07-24 01:39_

Yup that's what I was using and it was working fine with just two members. But now when I restore to the version that wasn't locking before, somehow it seems to be working now... Unfortunately I don't have a commit of the broken state but I'm pretty sure it's the same as it, and I'm not sure how I could come up with that error message otherwise. Is it possible to end up in a state where the contents of the lockfile can confuse resolution?

But without being to repro there probably isn't anything that can be done either way. Sorry for the noise.

---

_Closed by @anuraaga on 2025-07-24 01:39_

---
