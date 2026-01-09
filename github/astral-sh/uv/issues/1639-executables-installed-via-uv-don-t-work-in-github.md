---
number: 1639
title: "Executables installed via `uv` don't work in GitHub Actions on Windows"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-18T11:03:38Z
updated_at: 2024-02-19T13:12:39Z
url: https://github.com/astral-sh/uv/issues/1639
synced_at: 2026-01-07T13:12:16-06:00
---

# Executables installed via `uv` don't work in GitHub Actions on Windows

---

_Issue opened by @AlexWaygood on 2024-02-18 11:03_

Following #1523, standalone executables installed via `uv` seem to work fine on Windows locally. However, they still _don't_ seem to be working on Windows in the context of GitHub Actions. (By "standalone executables", I mean simply invoking `black` rather than `python -m black`, or simply invoking `coverage` rather than `python -m coverage`.) For a repro, see this PR, where CI is passing on MacOS and Linux, but not Windows:

- https://github.com/AlexWaygood/typeshed-stats/pull/190

The error occurs when the workflow tries to run `coverage run -m pytest`, and the error message is:

```
Error: The system cannot find the file specified.
 (from CreateProcessA(null(), child_cmdline.into_bytes_with_nul().as_mut_ptr(),
    null(), null(), 1, 0, null(), null(), addr_of!(*si),
    child_process_info.as_mut_ptr()))
Error: Process completed with exit code 1.
```

---

_Label `bug` added by @AlexWaygood on 2024-02-18 11:03_

---

_Label `windows` added by @AlexWaygood on 2024-02-18 11:03_

---

_Referenced in [astral-sh/uv#1612](../../astral-sh/uv/issues/1612.md) on 2024-02-18 11:04_

---

_Referenced in [AlexWaygood/typeshed-stats#190](../../AlexWaygood/typeshed-stats/pulls/190.md) on 2024-02-18 11:18_

---

_Comment by @MichaReiser on 2024-02-18 11:38_

That sounds like it can't find python for some reason. I can take a look 

---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-18 11:38_

---

_Comment by @MichaReiser on 2024-02-18 12:02_

I wonder if it's related to the venv "trick". 

The trampoline (it creates the call that failes above) should construct the following command:

```bash
<path_to_scripts>\python.exe <path_to_scripts>\black.exe
```

where `path_to_scripts` is constructed by taking the parent directory from `black.exe`'s absolute path. We know that it fails to resolve python because python emits a different error (see #1612) when the module can't be found. 

Now, I would expect that it can find the python.exe just fine because our activation script ensures that there's a python.exe next to the scripts. 

I won't have time to look into this before tomorrow but maybe something stands out for you why the python.exe wouldn't be found 

---

_Comment by @AlexWaygood on 2024-02-18 12:13_

> I wonder if it's related to the venv "trick".

Ah, that would make some sense, given that I wasn't able to repro this one locally — I'm not using the "trick" locally, of course!

---

_Comment by @AlexWaygood on 2024-02-18 12:38_

Unfortunately I really know very little about this area. What I _can_ tell you -- which you may already know, and may not be relevant -- is that the `python.exe` executable isn't located in the same place on Windows when a virtual environment isn't activated. When a virtual environment `.venv` is activated, `python.exe` is located at `.venv/Scripts/python.exe`. But my global Python installation (installed using the python.org installers) is located at `C:\Users\alexw\AppData\Local\Programs\Python\Python312\python.exe`. In GitHub Actions, it looks like it's located at `C:\hostedtoolcache\windows\Python\3.12.2\x64\python.exe`.

---

_Comment by @MichaReiser on 2024-02-18 12:43_

We can figure it out together. I know very little about how venv works on windows ;) 

>  is that the python.exe executable isn't located in the same place on Windows when a virtual environment isn't activated. 

I think this is the issue because our trampoline script assumes that the script and the python.exe are in the same directory which they're not according to your description. So we'll need another way to find the python installation. I need to look into what venv does but I wonder if we should parse out the shebang header from the python scrpt. 

---

_Comment by @AlexWaygood on 2024-02-18 13:08_

Hmmm... I tried using `uv` without the trick in this PR:

- https://github.com/AlexWaygood/typeshed-stats/pull/191

It gets much further: `coverage run -m pytest` correctly finds the `coverage` executable, and all tests but one are passing. But the specific test in my test suite that asserts that the standalone `typeshed-stats` executable (which has been installed via uv using `uv pip install -e .`) works as expected seems to be failing.

---

_Comment by @MichaReiser on 2024-02-18 16:16_

I'm just guessing but I get the impression that the tests run into the same problem because they run without activating a venv? 



---

_Comment by @ofek on 2024-02-18 23:08_

https://github.com/astral-sh/uv/issues/1310

---

_Comment by @MichaReiser on 2024-02-19 07:12_

Thanks @ofek but this is different than #1310. The python executable is correctly discovered. This issue is related to scripts, specifically when using `VIRTUAL_ENV=<global_path>`

---

_Comment by @MichaReiser on 2024-02-19 09:35_

I have a working workflow in https://github.com/AlexWaygood/typeshed-stats/pull/192 where I migrated most of the pipeline to use the native shell instead of `bash` on Windows (which matches the pip setup). 

It also works if you source the activation script instead of setting `VIRTUAL_ENV` and the path variables. https://github.com/AlexWaygood/typeshed-stats/pull/194

That makes me think that this is some path/environment variable issue rather than an issue with `uv` itself. However, using `uv` in github actions must become easier. It should just work  

---

_Comment by @AlexWaygood on 2024-02-19 13:12_

Agreed, I think we figured out that this isn't, fundamentally, an issue with `uv` itself (except perhaps that there are some missing features that are needed to make working with GitHub Actions easier).

Closing in favour of #1386

---

_Closed by @AlexWaygood on 2024-02-19 13:12_

---

_Referenced in [astral-sh/uv#1779](../../astral-sh/uv/issues/1779.md) on 2024-02-20 21:09_

---
