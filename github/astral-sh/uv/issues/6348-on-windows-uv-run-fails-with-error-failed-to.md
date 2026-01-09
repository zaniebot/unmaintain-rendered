---
number: 6348
title: "On Windows, `uv run` fails with \"error: Failed to spawn: `python`\""
type: issue
state: closed
author: pfmoore
labels:
  - bug
assignees: []
created_at: 2024-08-21T16:07:47Z
updated_at: 2025-05-23T20:48:25Z
url: https://github.com/astral-sh/uv/issues/6348
synced_at: 2026-01-07T13:12:17-06:00
---

# On Windows, `uv run` fails with "error: Failed to spawn: `python`"

---

_Issue opened by @pfmoore on 2024-08-21 16:07_

I just installed uv 0.3.0 using the installer script. This is on a Windows PC, with Python 3.8, 3.9, 3.10, 3.11 and 3.12 installed from the python.org installers. None of them have been added to `sys.path`, I use the `py` launcher (which by default selects the Python 3.12 installation). Apparently, I have the Windows Store version of Python 3.11 installed, but I never use it and it's not configured to be on my path by default (in "app execution aliases" I switched the "python.exe" and "python3.exe" aliases off).

With that background, if I write a small script `sp.py` as follows:

```
import sys
import os

print(f"{sys.executable=}")
print(f"{sys.path=}")
print(os.environ["PATH"].split(os.pathsep))
```

and run it using `uv run sp.py`, I get

```
❯ uv run sp.py
error: Failed to spawn: `python`
  Caused by: program not found
```

(by the way, it would be nice to have a way to manually run the Python interpreter that `uv run` will invoke - `uv run python` would be the obvious candidate, but it would need to work even if Python is not on `PATH` by default).

I'm happy to provide more details as to my environment - I'm sure this is related to my configuration, which is far from typical. But I'm reluctant to reconfigure things to "fix the problem", as what I'm doing *is* valid, if unusual, and in particular it's a deliberate choice on my part to not have a `python` command on `PATH` by default, and use the `py` launcher for everything.

---

_Comment by @zanieb on 2024-08-21 16:10_

Thank you for the thorough report! I believe we've talked about this before... I'm surprised we didn't fix it.

---

_Label `bug` added by @zanieb on 2024-08-21 16:10_

---

_Assigned to @charliermarsh by @zanieb on 2024-08-21 16:17_

---

_Comment by @zanieb on 2024-08-21 16:17_

The problem here is fairly obvious, @charliermarsh should have a fix up soon.

https://github.com/astral-sh/uv/blob/f10ccc488ee33ceedb4cfdca040c7e5033407edc/crates/uv/src/commands/project/run.rs#L777

---

_Comment by @charliermarsh on 2024-08-21 16:30_

Serious déjà vu but turns out I only fixed this for pager: https://github.com/astral-sh/uv/pull/5198

---

_Comment by @zanieb on 2024-08-21 16:33_

https://github.com/astral-sh/uv/issues/5838 / https://github.com/astral-sh/uv/pull/5849

---

_Referenced in [astral-sh/uv#6354](../../astral-sh/uv/pulls/6354.md) on 2024-08-21 16:41_

---

_Closed by @charliermarsh on 2024-08-21 17:14_

---

_Closed by @charliermarsh on 2024-08-21 17:14_

---

_Comment by @pfmoore on 2024-08-21 17:52_

Thanks! Just over half an hour from report to fix - impressive 🙂 

---

_Comment by @charliermarsh on 2024-08-21 17:53_

If it _doesn't_ fix it please feel welcome to ping here... I was able to replicate something similar on macOS but of course it's not totally identical to your setup.

---

_Comment by @pfmoore on 2024-08-21 17:59_

> If it doesn't fix it please feel welcome to ping here

Will do - I'll probably have to wait for a release as I don't have the time at the moment to build from source, but I'll check as soon as I can.

---

_Comment by @notatallshaw on 2024-08-22 00:04_

Releases also come fast around these parts: https://github.com/astral-sh/uv/releases/tag/0.3.1

---

_Comment by @pfmoore on 2024-08-22 08:31_

I no longer get the error (which is great, thanks @charliermarsh) but `uv run` picks the Store Python 3.11 to run my script, rather than my default python.org Python 3.12 installation (where by "default" I mean "the one the `py` launcher selects"). I can force the use of Python 3.12 via `--python 3.12`, but that's definitely not optimal :-( And I can see no way of selecting the python.org version of Python 3.11 over the Store version, short of providing the full path to the Python executable.

(On an unrelated note, I'd uninstall the Store version, if it wasn't for the fact that it acts as a really useful test case for issues like this...)

At a minimum, I would expect the *latest* installed (and registered as per [PEP 514](https://www.python.org/dev/peps/pep-0514/)) version of Python to be used by default, regardless of source. It would be more consistent if the behaviour of the `py` launcher was replicated, but that would require reading the `py.ini` [configuration file](https://docs.python.org/3.12/using/windows.html#customization-via-ini-files).

---

_Comment by @pfmoore on 2024-08-22 11:00_

On further investigation, if I switch off the app execution alias for `python3.11` (as well as the ones for `python3` and `python`, which I had already switched off) then `uv run` correctly finds the Python 3.12 installation.

That *almost* matches the docs [here](https://docs.astral.sh/uv/concepts/python-versions/#discovery-of-python-versions), which say:

> When searching for a system Python version, uv will use the first compatible version — not the newest version.

However, the earlier note about the "system Python" says

> A Python interpreter on the `PATH` as `python`, `python3`, or `python3.x` on macOS and Linux, or `python.exe` on Windows.

which suggests that `python3.x` is only for non-Windows systems.

While I don't think it's necessary for the documentation to give every detail of the discovery process, this presumes that discovery is intuitive - and IMO, defaulting to an older version of Python when there's no `python` command on the `PATH` (and hence no "default Python" at the command line) is *not* intuitive. I don't think the existence of `python3.x` on `PATH` should *ever* influence the decision on what the default Python version should be - by having an explicit version in the executable name, it's by definition not used as a default...

Again, I concede that this is a weird edge case caused by my specific config. I almost certainly should not have set things up so that I had `python3.11` on my path but no `python3` or `python`. But given that it's a possible configuration, I think it should be handled properly.

I've now switched off the `python3.11` alias, so I no longer have the issue raised here. But I can easily switch it back on if you need me to test any fixes.

---

_Comment by @zanieb on 2024-08-22 15:23_

There's an issue tracking PEP 514 support at https://github.com/astral-sh/uv/issues/1521

> While I don't think it's necessary for the documentation to give every detail of the discovery process, this presumes that discovery is intuitive - and IMO, defaulting to an older version of Python when there's no python command on the PATH (and hence no "default Python" at the command line) is not intuitive. I don't think the existence of python3.x on PATH should ever influence the decision on what the default Python version should be - by having an explicit version in the executable name, it's by definition not used as a default...

This makes sense to me. I think the problem is we look for all valid Python executable names on the PATH one directory at a time. Instead, `pythonX.Y` should only be used if we can't find any Python executable on the PATH with the name `python` or `python3` _or_ if you specifically request `--python X.Y`. This will be a little tricky to do in a performant manner, i.e., we don't want to list and parse the members of the directories on the PATH repeatedly. Opened https://github.com/astral-sh/uv/issues/6444 to track.

---

_Referenced in [astral-sh/uv#6444](../../astral-sh/uv/issues/6444.md) on 2024-08-22 15:27_

---

_Comment by @pfmoore on 2024-08-22 16:54_

Understood. As I say, my configuration is (was) unusual - it's not normal to have *just* the versioned `python3.x` executable without the unversioned one - so I wouldn't consider this as a priority (except maybe to point it out in the docs for now, as it *is* confusing when it happens).

---

_Referenced in [astral-sh/uv#10122](../../astral-sh/uv/issues/10122.md) on 2024-12-23 15:33_

---

_Comment by @magallardo on 2025-05-23 20:26_

I just got the following error.  UV has been working fine for me but running this command is failing

uv run pip install -e .
      Built forbiddenfruit==0.1.4
Installed 92 packages in 97ms
error: Failed to spawn: `pip`
  Caused by: No such file or directory (os error 2)

Please advise

---

_Comment by @konstin on 2025-05-23 20:48_

@magallardo Please do not comment on old closed issues, see https://github.com/astral-sh/uv/issues/9452. You're looking for `uv pip install`.

---
