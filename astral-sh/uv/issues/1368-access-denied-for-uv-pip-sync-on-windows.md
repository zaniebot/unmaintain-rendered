```yaml
number: 1368
title: "Access denied for `uv pip sync` on Windows"
type: issue
state: closed
author: jrkerns
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-15T22:05:36Z
updated_at: 2024-12-17T13:12:07Z
url: https://github.com/astral-sh/uv/issues/1368
synced_at: 2026-01-12T15:58:26Z
```

# Access denied for `uv pip sync` on Windows

---

_@jrkerns_

Within a given venv, running `uv pip sync <requirement file>` fails with Access is denied. This is on Windows. 

```
(venv311) C:\Users\kraken>uv pip sync requirements.txt
error: failed to remove file `\\?\C:\Users\kraken\venv311\Scripts\uv.exe`
  Caused by: Access is denied. (os error 5)
```


---

_Comment by @MichaReiser on 2024-02-15 22:06_

Thank you for reporting. This is related to https://github.com/astral-sh/uv/issues/1327

---

_Label `bug` added by @MichaReiser on 2024-02-15 22:06_

---

_Comment by @jrkerns on 2024-02-15 22:07_

Ah, I looked for something similar but missed it. Thanks!

---

_Label `windows` added by @zanieb on 2024-02-15 22:08_

---

_Comment by @T-256 on 2024-03-05 12:09_

I think deleting executable is not allowed when it's running, but it can rename itself.
I think solution here is to rename self to temp dir.

---

_Comment by @inoa-jboliveira on 2024-04-24 16:59_

Just completing, the issue  also happens with `uv pip install uv --upgrade` meaning uv can't replace its own executable. See closed issue #3239 above.
It is blocking us from adopting it for devs using Windows

---

_Comment by @zanieb on 2024-04-24 17:14_

I would strongly recommend against managing uv as a Python package and use our installer and `uv self update` instead.

---

_Comment by @inoa-jboliveira on 2024-04-24 17:30_

@zanieb We have issues on windows with the path and installing uv globally is not always possible.
Also if there is a uv installed inside the env, it will take priority over the one outside since the command will be replaced on env activation.

You cannot run `uv self update` if uv was installed in the env as package. BTW, it makes way more sense to version control uv this way -- also version lock it

---

_Comment by @EricThomson on 2024-08-05 14:47_

Not a solution, but a workaround (and note I use `uv` within a conda environment). I hit this error when I installed `uv` _within_ a virtual environment, but when I installed it in my base conda environment, so I could then use it in every created virtual environment, I stopped getting the error. 

When I tried updating uv (or pip/setuptools) within the environment, that didn't help. 

I know this won't help all users, but maybe the handful who stumble in here searching for the  `Caused by: Access is denied. (os error 5)` that use conda. 🤷 

---

_Comment by @charliermarsh on 2024-08-05 14:50_

Yeah, in general you can "solve" this by installing uv _outside_ of whatever environment you're trying to manage. uv doesn't need to be in a given virtual environment in order to operate on it (it's just a Rust binary, it can run over any environment or Python interpreter on your system).

(Of course I'd prefer not to error here though.)


---

_Comment by @inoa-jboliveira on 2024-08-05 18:25_

> Yeah, in general you can "solve" this by installing uv _outside_ of whatever environment you're trying to manage. 

That is not easy without the right permissions unless I download the binary and run it locally. It makes managing UV version way more difficult IMO.

In the end I resort to `pip install uv` and then I can use `uv pip install <stuff>` and other commands.

This is one of the 4 open tickets here preventing me from fully adopting uv. It is very close, but I still rely on several workarounds and other compromises with pip. 

Would it be possible to check if the file you're going to write exists and then save a temp one then perform a rename operation?

```
fs::rename(temp_executable_path, &current_executable_path)
```

This operation would be atomic without first removing the original file which the Windows forbids. If that does not work, the solution would be somewhere in the lines of running UV from a temp executable.

---

_Comment by @T-256 on 2024-08-05 23:11_

> That is not easy without the right permissions unless I download the binary and run it locally

Does https://github.com/astral-sh/uv/pull/5455 solve your need? you can set `CARGO_HOME` env variable to where you have permissions before executing `uv-installer`; so uv will install there.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-08 02:42_

---

_Closed by @charliermarsh on 2024-11-08 02:57_

---

_Comment by @timonmerk on 2024-11-11 10:03_

Just wanted to mention that manually deleting the `Scripts\uv.exe` or `Scripts\ruff.exe` on Windows also fixes that error. I know it's not related to fixing the core of the problem. But just wanted to throw it out in case someone needs a quick fix

---

_Comment by @adam-grant-hendry on 2024-12-13 01:00_

`poetry` ran into this exact same problem: [#7610](https://github.com/python-poetry/poetry/issues/7610).

I'm betting using `uvx` will make the issue will go away:

```
uvx pip sync requirements.txt
```

---

_Comment by @charliermarsh on 2024-12-13 01:03_

Should also be fixed in newer releases.

---

_Comment by @adam-grant-hendry on 2024-12-13 01:13_

> Should also be fixed in newer releases.

Out of curiosity, what is the fix for this issue?

FYI, these may help:

[poetry Issue #1031](https://github.com/python-poetry/poetry/issues/1031)
[poetry PR #460](https://github.com/python-poetry/poetry-core/pull/460) (*Fixes 1031*)

---

_Comment by @charliermarsh on 2024-12-13 01:23_

We use a library called [`self-replace`](https://docs.rs/self-replace/latest/self_replace/) when targeting the currently-running binary.

---

_Comment by @adam-grant-hendry on 2024-12-13 01:25_

> We use a library called [`self-replace`](https://docs.rs/self-replace/latest/self_replace/) when targeting the currently-running binary.

Doy, just saw [#8914](https://github.com/astral-sh/uv/pull/8914). Sorry, my bad. And thank you!

---

_Comment by @arden13 on 2024-12-17 13:03_

Hey All,

I've been testing out uv for my work and am hitting what seems a related issue to this one. Running on a windows 11 machine I'm generating some base python environments and I hit this same error (os error 5) when running

`uv venv py311 --native-tls --python 3.11`

running this one time it fails. Running it a second time it succeeds.

```
(nipals_uv) PS C:\Users\username\git_repos\open_nipals> uv venv py311 --native-tls --python 3.11
  × Failed to copy to:
  │ C:\Users\username\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none
  ╰─▶ failed to rename file from C:\Users\username\AppData\Roaming\uv\python\.temp\.tmpu6Kxca\python
      to C:\Users\username\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none: Access    
      is denied. (os error 5)
(nipals_uv) PS C:\Users\username\git_repos\open_nipals> uv venv py311 --native-tls --python 3.11
Using CPython 3.11.11
Creating virtual environment at: py311
Activate with: py311\Scripts\activate
```

---

_Comment by @charliermarsh on 2024-12-17 13:11_

@arden13 -- That's a different issue -- you're looking for https://github.com/astral-sh/uv/issues/1327.

---

_Comment by @arden13 on 2024-12-17 13:12_

Thanks I'll go there

---
