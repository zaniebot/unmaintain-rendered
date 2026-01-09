---
number: 2389
title: Generating distributable python environments (e.g. for use in PySpark)
type: issue
state: open
author: brendan-morin
labels:
  - enhancement
assignees: []
created_at: 2024-03-12T18:52:17Z
updated_at: 2025-06-11T11:23:52Z
url: https://github.com/astral-sh/uv/issues/2389
synced_at: 2026-01-07T13:12:17-06:00
---

# Generating distributable python environments (e.g. for use in PySpark)

---

_Issue opened by @brendan-morin on 2024-03-12 18:52_

Hi! Let's start off - this is a low priority issue and more something to think about as you continue to build out functionality and your roadmap. I understand uv uses something like copy-on-write or symbolic linking to create the environments, which helps provide its speed. 

There is one use case where we have seen a really poor user experience, and that is in creating distributable python environments. In particular, ones that include a python interpreter with them so that all executors in a distributed environment can meet the same dependencies.

For some context, here is PySpark's documentation on the issue: https://spark.apache.org/docs/latest/api/python/user_guide/python_packaging.html

The gist of the issue is that regular venv environments tend not to be well suited for distribution because the symbolic linking, which does not guarantee your intended python version will be available on the executors. Right now, most folks I know who work through this use e.g. miniconda environments which bundle the interpreter as part of them.

Would be great to see if this was something uv eventually was capable of handling with a nicer user experience, especially if you get into the business of managing the python version for the environment as well.

---

_Label `enhancement` added by @zanieb on 2024-03-12 19:59_

---

_Comment by @konstin on 2024-03-13 12:21_

There is a `--link-mode copy` flag in `uv pip sync` and `uv pip install`, would that solve the part links not being portable?

---

_Comment by @zanieb on 2024-09-03 23:22_

I think we have a `uv venv --relocatable` flag now, does that suffice?

---

_Comment by @brendan-morin on 2024-09-04 16:54_

I can try experimenting with this. Does this mean the created venv would be self-contained include the full python interpreter? I suppose the test would be moving the venv to a system that does not have python installed - would we expect it to still work?

---

_Comment by @eruvanos on 2024-10-09 08:18_

I am writing small games in Python. while PyInstaller is a valid option, the created binaries require signing etc, so it is not always detected as a virus by MS.

I wonder if it would be a better option to ship it using uv in the background to get the required dependencies etc. Or just create a zip, which bundles python, env and a start script.

---

_Comment by @bakkiaraj on 2024-12-26 12:52_

--relocatable , atleast is linux (ubuntu 24.04) still create symbolic link to either system python (/usr/bin/python3) or the managed python from uv (/home/xx/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/bin/python3.13*) , So venv moveable / relocatable with in the same / similar machine  but not "redistributable"

---

_Comment by @jlevy on 2025-04-12 01:04_

So I looked at this problem more this week on macOS, ubuntu, and Windows and in particular on macOS there are issues with uv building a completely relocatable Python installation (even though in theory the managed Python packages are).

One is that on macOS dylibs [are patched](https://github.com/astral-sh/uv/blob/main/crates/uv-python/src/managed.rs#L545-L567) to have their own path. Another is that there is no `uv pip install --relocatable`.

However I think I've built a solution, a small tool that does some of the little hacks that seemed necessary:
https://github.com/jlevy/py-app-standalone

If you want to try it out, try `uvx py-app-standalone cowsay` (replacing cowsay with whatever pips you prefer). I'd be curious to hear if it works! Or if folks have better ideas or workarounds.

cc @eruvanos @brendan-morin @zanieb 

Edit: I renamed the tool to py-app-standalone after people told me the old name was confusing bc it had "pip" in it.

---

_Comment by @JalinWang on 2025-06-11 11:23_

[[Feature Request] allow exporting virtual environments ](https://github.com/astral-sh/uv/issues/6970)

---
