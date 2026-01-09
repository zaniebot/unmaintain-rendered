---
number: 7870
title: "Python interpreters inside `/.local/share/uv/python` cannot find brew libraries"
type: issue
state: closed
author: agustingomes-therapieland
labels: []
assignees: []
created_at: 2024-10-02T15:35:02Z
updated_at: 2025-10-31T03:45:28Z
url: https://github.com/astral-sh/uv/issues/7870
synced_at: 2026-01-07T13:12:17-06:00
---

# Python interpreters inside `/.local/share/uv/python` cannot find brew libraries

---

_Issue opened by @agustingomes-therapieland on 2024-10-02 15:35_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hello team,

First, let me thank you for your hard work so far.

I am starting to experiment with UV at work to improve the way we manage our Python environments and potentially more. As part of that R&D, on my Mac OS M1, I'm seeing that one of the project dependencies (WeasyPrint) is not able to find system libraries. [This bug](https://github.com/Kozea/WeasyPrint/issues/1448) is exactly the same symptom.

The reason I'm opening the issue here, is because if I use the Python installation from Brew (`/opt/homebrew/opt/python@3.12/bin/python3.12`) I don't face this issue, and the libraries seem to be found.

Can you share why the UV installed Python is not able to find the libraries?

Thanks in advance, and sorry if this is not the correct process for this type of issue.


---

_Comment by @zanieb on 2024-10-02 15:56_

I presume the Brew distribution is hard-coded to automatically load Brew libraries but external distributions will need to be told where to find them.

Related:

- https://github.com/astral-sh/uv/issues/7764
- https://github.com/astral-sh/uv/issues/6971

---

_Comment by @agustingomes-therapieland on 2024-10-02 16:14_

Thank you @zanieb for the reply.

I will close my issue in favor of the already opened issue.

Will also check in more detail the other closed issue, especially because of the library used there to reproduce the issue, which I suspect will help me find a workaround for my context.

---

_Closed by @agustingomes-therapieland on 2024-10-02 16:14_

---

_Reopened by @agustingomes-therapieland on 2024-10-02 16:15_

---

_Closed by @agustingomes-therapieland on 2024-10-02 16:15_

---

_Referenced in [astral-sh/uv#6971](../../astral-sh/uv/issues/6971.md) on 2024-10-02 20:14_

---

_Comment by @micahjsmith on 2025-09-11 03:28_

The only way I have found to use weasyprint generically within uv managed python is to link the weasyprint dependencies manually to a standard (non homebrew) library path. by generic use, I mean I can do the following

- `uv run weasyprint --version`
- `/bin/sh -c "uv run weasyprint --version"`
- create a pyinvoke task that runs a command that ends up importing weasyprint (even if I set the pyinvoke default shell to be the homebrew bash)\
- run a custom script that uses django management commands
- etc

Since there are so many entrypoints, its not feasible to patch in all places using the fallback env var as discussed in other threads

Here is my weasyprint-specific solution
```
sudo mkdir -p /usr/local/lib

pkgs=(
	"libgobject-2.0"
	"libpango-1.0"
	"libharfbuzz"
  "libpangoft2-1.0"
  "libfontconfig.1"
)
for pkg in "${pkgs[@]}";
do
	sudo ln -s -f $(brew --prefix)/lib/$pkg.dylib /usr/local/lib/$pkg.dylib
done
```

this works for weasyprint 66.0 on python 3.11.13 and uv 0.7.9

use at your own risk!

---

_Comment by @nitsujri on 2025-10-31 00:55_

I also arrived here due to weasyprint.

I came to the same conclusion as @micahjsmith, that there are just too many entrypoints, but any direct solution felt brittle or dirty (symlinks, .profile, direnv, preloader.py). So ended at a different solution - going back to `pyenv` so I could have it built w/ the references.

It feels like going backwards? would be nice if uv could build python too?

---

_Comment by @zanieb on 2025-10-31 03:45_

It seems pretty reasonable to symlink brew packages into a global lib directory if Apple is going to be a pain and block you from setting `DYLD_FALLBACK_LIBRARY_PATH`. You can also disable the system protections that don't let you set that variable — I understand why people wouldn't want to do that though.

---
