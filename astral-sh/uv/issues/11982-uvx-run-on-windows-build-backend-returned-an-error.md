---
number: 11982
title: "uvx run on windows: build backend returned an error"
type: issue
state: closed
author: liquidcarbon
labels:
  - question
  - error messages
assignees: []
created_at: 2025-03-05T16:19:22Z
updated_at: 2025-07-07T06:37:57Z
url: https://github.com/astral-sh/uv/issues/11982
synced_at: 2026-01-10T01:25:13Z
---

# uvx run on windows: build backend returned an error

---

_Issue opened by @liquidcarbon on 2025-03-05 16:19_

### Summary

Problem on a fresh install:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

uv python install 3.12
Installed Python 3.12.9 in 7.32s
 + cpython-3.12.9-windows-x86_64-none
```

In an empty folder:
```
uv venv --python 3.12
.\.venv\Scripts\python.exe -VV
Python 3.12.9 (main, Feb 12 2025, 14:52:31) [MSC v.1942 64 bit (AMD64)]
```

So far so good.  Then:

```
uvx run pycowsay moo
  x Failed to build `run==0.2`
  |-> The build backend returned an error
  `-> Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "C:\Users\a\AppData\Local\uv\cache\builds-v0\.tmpzaSQDl\Lib\site-packages\setuptools\build_meta.py", line 334, in
      get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\a\AppData\Local\uv\cache\builds-v0\.tmpzaSQDl\Lib\site-packages\setuptools\build_meta.py", line 304, in
      _get_build_requires
          self.run_setup()
        File "C:\Users\a\AppData\Local\uv\cache\builds-v0\.tmpzaSQDl\Lib\site-packages\setuptools\build_meta.py", line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File "C:\Users\a\AppData\Local\uv\cache\builds-v0\.tmpzaSQDl\Lib\site-packages\setuptools\build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 12, in <module>
      NameError: name 'file' is not defined. Did you mean: 'filter'?

      hint: This usually indicates a problem with the package or the build environment.
```



### Platform

Microsoft Windows Server 2019 Standard 10.0.17763

### Version

0.6.4

### Python version

3.12.9

---

_Label `bug` added by @liquidcarbon on 2025-03-05 16:19_

---

_Comment by @charliermarsh on 2025-03-05 16:23_

I think you want `uvx pycowsay moo` (no `run`).

---

_Comment by @charliermarsh on 2025-03-05 16:33_

(`uvx` is an alias for `uv tool run`. So `uvx run pycowsay moo` is `uv tool run run pycowsay moo`.)

---

_Label `bug` removed by @charliermarsh on 2025-03-05 16:36_

---

_Label `question` added by @charliermarsh on 2025-03-05 16:36_

---

_Label `error messages` added by @charliermarsh on 2025-03-05 16:50_

---

_Comment by @charliermarsh on 2025-03-05 16:51_

We should add a dedicated error message for this.

---

_Comment by @liquidcarbon on 2025-03-05 16:59_

: facepalm : that was silly.  Thank you.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-05 17:02_

---

_Comment by @notatallshaw on 2025-03-05 17:36_

I've made this error more than once (along with `uv venv .` for some reason).

---

_Comment by @charliermarsh on 2025-03-05 17:44_

Honestly, I want to add a prompt that says "Did you mean...?" and then auto-corrects. But it's plausible that companies _could_ have a package named `run` and this would get in the way. Or the `run` package could be revived on some point at PyPI.

---

_Comment by @jromal on 2025-03-05 20:46_

The prompt and autocorrection seem like a good idea.  

If the "run" package or any other package that may conflict with the parameters exists, you could perhaps accept `uvx "run"`, clearly indicating that the package is called "run" by using quotes.  

Not sure if this is a very good or very bad idea. Or if it is possible. 
But I wouldn't sacrifice a better user experience just for a few packages. Those should be "implicitly" named.  


---

_Referenced in [astral-sh/uv#11992](../../astral-sh/uv/pulls/11992.md) on 2025-03-05 22:04_

---

_Closed by @charliermarsh on 2025-03-06 02:15_

---

_Closed by @charliermarsh on 2025-03-06 02:15_

---

_Comment by @Fortytwoo on 2025-07-07 06:37_

Oh it seems that I am still very unfamiliar with uv.

---
