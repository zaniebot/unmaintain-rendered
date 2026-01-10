---
number: 14684
title: "`--upgrade-package` does not upgrade GutHub-sourced packages"
type: issue
state: open
author: msamo-n
labels:
  - bug
assignees: []
created_at: 2025-07-17T15:39:24Z
updated_at: 2025-08-08T13:27:56Z
url: https://github.com/astral-sh/uv/issues/14684
synced_at: 2026-01-10T01:25:47Z
---

# `--upgrade-package` does not upgrade GutHub-sourced packages

---

_Issue opened by @msamo-n on 2025-07-17 15:39_

### Question

Hello!

I have a dependency on a package, located in GitHub private repo. It's defined as:
```(toml)
[project]
dependencies = [
    "mypkg[all]",
]

[tool.uv.sources]
mypkg= { git = "ssh://git@github.com/...", branch = "master" }
```

When I run `uv sync --upgrade`, the package got updated (gets installed from the current `HEAD` of `master` branch).
When I run `uv sync --upgrade-package mypkg`, the package does not update (version pinned in `uv.lock` is used). It seems, that `uv` doesn't fetch from GitHub at all.

Is it somehow expected behavior?

### Platform

Ubuntu 22.04

### Version

uv 0.7.21

---

_Label `question` added by @msamo-n on 2025-07-17 15:39_

---

_Comment by @msamo-n on 2025-07-17 15:46_

BTW, if I specify a package as `mypkg @ git+ssh://git@github.com/...`, the latest `HEAD` seems fetched. But later `uv` fails as:
```
error: Requirements contain conflicting URLs for package `mypkg ` in all marker environments:
- git+ssh://git@github.com/...
- git+ssh://git@github.com/...
```

---

_Label `question` removed by @zanieb on 2025-07-17 15:48_

---

_Label `bug` added by @zanieb on 2025-07-17 15:48_

---

_Comment by @zanieb on 2025-07-17 15:48_

Sounds like a bug to me, thanks!

---

_Comment by @zanieb on 2025-07-17 16:08_

Can you share verbose logs for both cases please?

---

_Comment by @zanieb on 2025-07-17 16:09_

(A minimal reproduction would save me a lot of time)

---

_Comment by @msamo-n on 2025-07-17 17:16_

Well, now it looks even more interesting. I have a project, that depends on two GitHub-sourced packages. One is upgraded fine on its own (`-P`), but another gets upgraded only in "full upgrade" mode (`-U`).

Here is the dependent project:
```(toml)
[project]
name = "mypkg-dependant"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "tnc[pkgB]",
    "mypkg[all]",
    "flake8",
]

[tool.uv.sources]
mypkg = { git = "ssh://git@github.com/msamo-n/mypkg", branch = "another-branch" }
tnc = { git = "ssh://git@github.com/msamo-n/exp", branch = "uv-exp" }
```

I run:
```(bash)
uv sync -v -P mypkg 2>./upgrade_mypkg.log 
uv sync -v -P tnc 2>./upgrade_tnc.log
uv sync -v -U 2>./upgrade_all.log
```

Verbose logs are attached.

[upgrade_all.log](https://github.com/user-attachments/files/21301663/upgrade_all.log)
[upgrade_mypkg.log](https://github.com/user-attachments/files/21301665/upgrade_mypkg.log)
[upgrade_tnc.log](https://github.com/user-attachments/files/21301662/upgrade_tnc.log)

---

_Comment by @charliermarsh on 2025-08-01 02:32_

Ah ok, I think this is similar to https://github.com/astral-sh/uv/issues/14954. It looks like `tnc[pkgB]` adds another Git dependency (`pkgB`) from the same repo, and we end up respecting _that_ package's SHA. So you'd need to do `-P tnc -P pkgB`. It is indeed a bug.

---

_Comment by @charliermarsh on 2025-08-01 02:33_

It's pretty complicated though. If you pass `-P tnc`, should we _not_ upgrade `pkgB`? (If we _do_ want to upgrade `pkgB`, it gets much more complex because we need to understand that they're part of the same workspace or something.)

---

_Comment by @msamo-n on 2025-08-08 13:27_

> If you pass -P tnc, should we not upgrade pkgB?

Personally, I'd say that if the upgraded `tnc` depends on different version of `pkgB`, `pkgB` should be upgraded as well. In my case, as both packages are versioned together (being in the same repo), they get upgraded together as well.

Actually, my goal was to keep a few packages in the same repo and install them from the same commit (for consistency).


---
