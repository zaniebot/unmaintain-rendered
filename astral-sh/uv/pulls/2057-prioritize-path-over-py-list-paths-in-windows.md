```yaml
number: 2057
title: "Prioritize `PATH` over `py --list-paths` in Windows selection"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/win
created_at: 2024-02-28T22:02:05Z
updated_at: 2024-02-29T15:10:46Z
url: https://github.com/astral-sh/uv/pull/2057
synced_at: 2026-01-10T14:54:43Z
```

# Prioritize `PATH` over `py --list-paths` in Windows selection

---

_Pull request opened by @charliermarsh on 2024-02-28 22:02_

`uv --system` is failing in GitHub Actions, because `py --list-paths` returns all the pre-cached Pythons:

```
-V:3.12 *        C:\hostedtoolcache\windows\Python\3.12.2\x64\python.exe
-V:3.12-32       C:\hostedtoolcache\windows\Python\3.12.2\x86\python.exe
-V:3.11          C:\hostedtoolcache\windows\Python\3.11.8\x64\python.exe
-V:3.11-32       C:\hostedtoolcache\windows\Python\3.11.8\x86\python.exe
-V:3.10          C:\hostedtoolcache\windows\Python\3.10.11\x64\python.exe
-V:3.10-32       C:\hostedtoolcache\windows\Python\3.10.11\x86\python.exe
-V:3.9           C:\hostedtoolcache\windows\Python\3.9.13\x64\python.exe
-V:3.9-32        C:\hostedtoolcache\windows\Python\3.9.13\x86\python.exe
-V:3.8           C:\hostedtoolcache\windows\Python\3.8.10\x64\python.exe
-V:3.8-32        C:\hostedtoolcache\windows\Python\3.8.10\x86\python.exe
-V:3.7           C:\hostedtoolcache\windows\Python\3.7.9\x64\python.exe
-V:3.7-32        C:\hostedtoolcache\windows\Python\3.7.9\x86\python.exe
```

So, our default selector returns the first entry here. But none of these are actually in `PATH` except the one that the user installed via `actions/setup-python@v5` -- that's the point of the action, that it puts the correct versions in `PATH`.

It seems to me like we should prioritize `PATH` over `py --list-paths`. Is there a good reason not to do this?

Closes: https://github.com/astral-sh/uv/issues/2056

---

_Converted to draft by @charliermarsh on 2024-02-28 22:02_

---

_Comment by @charliermarsh on 2024-02-28 22:02_

\cc @AlexWaygood 

---

_Renamed from "Debug GitHub Actions failures for Python 3.10 on Windows" to "Prioritize `PATH` over `py --list-paths` in Windows selection" by @charliermarsh on 2024-02-29 01:11_

---

_Review requested from @konstin by @charliermarsh on 2024-02-29 01:13_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-02-29 01:13_

---

_Marked ready for review by @charliermarsh on 2024-02-29 01:13_

---

_Label `bug` added by @charliermarsh on 2024-02-29 01:13_

---

_Comment by @ofek on 2024-02-29 01:37_

This seems like a good idea to me and what users would expect to happen.

---

_Comment by @zanieb on 2024-02-29 02:10_

I thought we did this already, maybe it's gone back and forth a bit during development. Makes sense to me.

---

_Comment by @charliermarsh on 2024-02-29 02:12_

(Continuing from the issue...) I'm surprised because virtualenv does seem to do registry-based lookups before PATH. But I suppose for virtualenv it matters less, since the priority is getting the right _version_?

---

_Comment by @charliermarsh on 2024-02-29 02:15_

I see Micha's change that added the fallback to `PATH`: https://github.com/astral-sh/uv/pull/1711.

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-29 02:15_

---

_Comment by @ofek on 2024-02-29 02:28_

Also great because the overhead of a separate process is eliminated in most cases! I'm very excited for this.

---

_Review comment by @MichaReiser on `.github/workflows/system-install.yml`:71 on 2024-02-29 07:21_

Remove?

---

_@MichaReiser approved on 2024-02-29 07:30_

> This seems like a good idea to me and what users would expect to happen.

I think this is a bit more nuanced and depends on your workflow and how you installed python. Users using `py` (over `python`) may have an additional installation on their `PATH`.  For them, picking `PATH` over the default used by `py` can be surprising. 

The reason `virtual` doesn't run into this problem is that it always tries to use the current Interpreter (in which it is running) first. It then doesn't matter whether a user uses `python` or `py` or a shim to run python. 

The challenge we face is that we can't rely on the "current python" instance, so we have to approximate it as best as we can and using `PATH` first probably does the right thing in more cases than using `py` first, with the exception for users using `py` with another python installation on `PATH`. 

> Also great because the overhead of a separate process is eliminated in most cases! I'm very excited for this.

We planned to re-implement the registry lookup in Rust, in which case the PATH traversal is most likely slower, especially when we can't find a python installation on PATH and need to fall-back to `py`

---

_@MichaReiser reviewed on 2024-02-29 07:32_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:189 on 2024-02-29 07:32_

This comment is outdated. We should remove this branch. 

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:191 on 2024-02-29 07:33_

This message is outdated. We already tried searching the PATh. We should keep the graceful recovery 

---

_@MichaReiser reviewed on 2024-02-29 07:33_

---

_Comment by @konstin on 2024-02-29 08:45_

A minor feature we're losing is that `py` is intended to be the main tool on windows and has a friendly UX (`py -0`) that avoids dealing with `PATH` on windows (which is hidden in a GUI behind another GUI, it's nothing like adding a line in your `.bashrc` on linux).

From [PEP 514](https://peps.python.org/pep-0514/):

> This PEP defines a schema for the Python registry key to allow third-party installers to register their installation, and to allow tools and applications to detect and correctly display all Python environments on a user’s machine. No implementation changes to Python are proposed with this PEP.

> Python environments are not required to be registered unless they want to be automatically discoverable by external tools. 

In this sense github actions is at odds with the PEP, the versions shouldn't be in the registry if not selected by the setup python action (or setup python should at least ensure that `py` has the right default).

I think either choice of precedence between `PATH` and `py` is fine for us to make, both are standards, one of posix-and-windows ubiquity, the other of PEP 514, and if the user wants a specific python version they can pass it explictly. In this case i agree that we can't break github actions.

---

_Review comment by @konstin on `.github/workflows/system-install.yml`:4 on 2024-02-29 08:47_

```suggestion
```

(Am i understanding correctly that this should only be running manually?)

---

_@konstin approved on 2024-02-29 08:47_

---

_Comment by @AlexWaygood on 2024-02-29 10:38_

I agree that this is the right way to go.

FWIW, I always tick the "add to PATH" box when installing Python on Windows. I have a vague memory of things going wrong (or, not working the way I expected to) the one time I didn't do that. But that was a very long time ago, and it might very well be that I've just been cargo-culting from old tutorials that predated PEP-514 this whole time `¯\_(ツ)_/¯`

---

_Comment by @ofek on 2024-02-29 14:08_

FYI I wrote a bit here https://github.com/astral-sh/uv/issues/2056#issuecomment-1971209233 the relevant part is:

> I have been exclusively a Windows user my entire life. I cannot express enough how few people use the `py` launcher and how often the registry stuff is unused.

🙂 

---

_Review comment by @charliermarsh on `.github/workflows/system-install.yml`:4 on 2024-02-29 14:50_

Yup! This is just set so I can test it during development of this PR. It will be removed prior to landing.

---

_@charliermarsh reviewed on 2024-02-29 14:50_

---

_Label `windows` added by @charliermarsh on 2024-02-29 15:00_

---

_Merged by @charliermarsh on 2024-02-29 15:06_

---

_Closed by @charliermarsh on 2024-02-29 15:06_

---

_Branch deleted on 2024-02-29 15:06_

---

_@charliermarsh reviewed on 2024-02-29 15:10_

---

_Review comment by @charliermarsh on `.github/workflows/system-install.yml`:71 on 2024-02-29 15:10_

(Intentionally kept this change.)

---
