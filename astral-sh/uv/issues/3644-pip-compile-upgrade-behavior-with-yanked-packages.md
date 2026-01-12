```yaml
number: 3644
title: "`pip compile --upgrade` behavior with yanked packages"
type: issue
state: open
author: zanieb
labels:
  - help wanted
assignees: []
created_at: 2024-05-17T18:39:01Z
updated_at: 2024-08-16T14:52:46Z
url: https://github.com/astral-sh/uv/issues/3644
synced_at: 2026-01-12T15:58:45Z
```

# `pip compile --upgrade` behavior with yanked packages

---

_@zanieb_

Prompted at https://github.com/astral-sh/uv/issues/3602#issuecomment-2115761269

When a yanked package is pinned in a lockfile and `pip compile --upgrade` is used and there is no new version of the package in the input range, should we

1. Downgrade to the unyanked version
2. Error that a yanked version is being used
3. Warn that a yanked version is being used
4. Silently continue to use the yanked version

Note the yanked package is _not_ pinned in the input requirements.

---

_Comment by @notatallshaw on 2024-05-17 20:26_

Thinking out loud, it seems like the most likely scenario this would happen in is if the compile originally ran and the package was not yanked, and then it was run at a later time and the package was yanked.

Given this is in the case where the yanked package is not pinned in the requirements, I would personally expect "1." to happen,  with maybe a warning. As it was presumably yanked for a reason and I'm not pinning it, so I would would trust the package authors reason for yanking.

If the user wants to pin their package to a yanked version, I think it makes sense to require them to do it in the input requirements.

---

_Comment by @zanieb on 2024-05-17 21:11_

Yeah that's my feeling as well. I would find it weird that `--upgrade` downgrades a package but 🤷‍♀️ yanks are rare.

---

_Comment by @notatallshaw on 2024-05-20 14:39_

Does `--upgrade` normally guarantee the versions pinned in the old output file will be lower bounds of the new output file?

For example if I had a direct dependency on `a`, and when I ran pip compile for the first time it created a pinning `b==2.0`, but then when I ran upgrade the next day the newer version of `a` now requires `b==1.0`, would `a` be upgraded and `b` downgraded or would `a` now never be able to upgrade?

I'm not that familiar with the behavior of `pip compile --upgrade` but I know `conda upgrade --all` will downgrade packages.

---

_Comment by @zanieb on 2024-05-20 15:43_

Honestly I'm not sure! I would be surprised if it provided that guarantee though, I presume we just no longer prefer the pinned versions and perform a resolution based on the initial bounds.

---

_Comment by @konstin on 2024-05-21 09:20_

I'd definitely want a warning, i've been using a broken release. Option 1 seems to be the most helpful behavior to me.

---

_Comment by @zanieb on 2024-05-21 14:38_

The next step is to verify our existing behavior, if anyone is interested.

---

_Label `help wanted` added by @zanieb on 2024-05-21 14:38_

---

_Comment by @notatallshaw on 2024-08-16 14:48_

FYI, the existing behavior is currently "1." (with no warning), I came across this last month with a real world example, matplotlib yanked 3.9.1: https://pypi.org/project/matplotlib/3.9.1/

My output file had `matplotlib==3.9.1` in it, and when I reran `uv pip compile --upgrade` I got  `matplotlib==3.9.0` .

---
