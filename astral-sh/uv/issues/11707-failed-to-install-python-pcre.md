```yaml
number: 11707
title: Failed to install python-pcre
type: issue
state: closed
author: bojanz
labels:
  - needs-mre
assignees: []
created_at: 2025-02-22T09:09:29Z
updated_at: 2025-10-23T20:37:28Z
url: https://github.com/astral-sh/uv/issues/11707
synced_at: 2026-01-12T16:00:44Z
```

# Failed to install python-pcre

---

_@bojanz_

### Summary

On a project that just switched from Poetry, I am getting an error while building python-pcre:
```
 × Failed to build `python-pcre==0.7`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      cc: error: unrecognized command-line option ‘-fdebug-default-version=4’
      error: command '/usr/bin/cc' failed with exit code 1

      hint: This usually indicates a problem with the package or the build environment.
```
The same install works with just pip (`pip install python-pcre`).

cc is `cc (GCC) 14.2.1 20250110 (Red Hat 14.2.1-7)`.

I'd appreciate any hints towards what I could do to debug this.

### Platform

Fedora 41

### Version

0.6.2

### Python version

3.11.6

---

_Label `bug` added by @bojanz on 2025-02-22 09:09_

---

_Comment by @bojanz on 2025-02-22 14:34_

Some researching shows that fdebug-default-version is a clang-only flag, and that it is supposed to be stripped in [astral-sh/python-build-standalone](https://github.com/astral-sh/python-build-standalone). Perhaps that didn't reach my Python version?

---

_Comment by @charliermarsh on 2025-02-22 17:26_

Hmm, I was able to build without issue on my machine. Did you install Python with uv?

---

_Label `bug` removed by @charliermarsh on 2025-02-22 17:26_

---

_Label `needs-mre` added by @charliermarsh on 2025-02-22 17:26_

---

_Comment by @bojanz on 2025-02-22 18:00_

I did not use `uv python install`, I have Fedora's pre-installed Python 3.13, and uv pulled down the Python 3.11.6 as specified in `.python-version` so I'm not expecting the 3.13 to have been used.

Once I understood that the problem is in clang-specific flags, I ran `CC=clang uv sync` and it worked fine.

---

_Comment by @bojanz on 2025-02-22 18:18_

I see that the fix for clang-specific flags was released in https://github.com/astral-sh/python-build-standalone/releases/tag/20240107, which bumped the 3.11 version to 3.11.7. 

So when I request 3.11.6 I should get the build that used the [previous](https://github.com/astral-sh/python-build-standalone/releases/tag/20231002), still-buggy version, right? That would explain it.

---

_Comment by @charliermarsh on 2025-02-24 00:07_

That seems plausible, yeah.

---

_Comment by @bojanz on 2025-02-24 08:28_

Mystery solved.

Closing this issue cause there's no bug anymore. Leaving it to you to decide if you want to communicate somewhere that these older Python versions don't build properly with GCC.

Thanks.

---

_Closed by @bojanz on 2025-02-24 08:28_

---

_Comment by @arielnmz on 2025-10-23 19:41_

Can someone please elaborate on what is the problem and why does the Python version and the way it was built affect how the packages are built? 
I'm currently dealing with an issue in pycharm not being able to build the cython extensions for debugging and the error message is exactly the same.
In my case I'm using asdf to build and install python (3.12.12) and I also happen to use uv (0.7.12) to handle my env but NOT to build python. 
I do see that Pycharm is using the python bin from my .venv to build these extensions:

```
2025-10-23 12:51:24,544 [ 131627]   INFO - #c.j.p.d.PyCythonExtensionWarning - Compile Cython Extensions /home/ben/project/.venv/bin/python /home/ben/.local/share/JetBrains/Toolbox/apps/pycharm/plugins/python-ce/helpers/pydev/setup_cython.py build_ext --build-lib /home/ben/.cache/JetBrains/PyCharm2025.2/cythonExtensions --build-temp /home/ben/.cache/JetBrains/PyCharm2025.2/cythonExtensions/build
```

For now, switching the python version used in my project to my system's python build (python 3.13, Fedora 42) seems to affect the python version used for the build and this time it succeeds:

```
2025-10-23 13:10:38,149 [1285232]   INFO - #c.j.p.d.PyCythonExtensionWarning - Compile Cython Extensions /usr/bin/python3 /home/ben/.local/share/JetBrains/Toolbox/apps/pycharm/plugins/python-ce/helpers/pydev/setup_cython.py build_ext --build-lib /home/ben/.cache/JetBrains/PyCharm2025.2/cythonExtensions --build-temp /home/ben/.cache/JetBrains/PyCharm2025.2/cythonExtensions/build
```

What is going on here? I tried running `CC=clang asdf install python 3.12.12` but I don't have it installed but WHY does this have to be built in a certain way using a different cc to be able to take in different flags?

---

_Comment by @arielnmz on 2025-10-23 20:37_

Okay Claude 4 helped me figure this one out:

- The python binary "inherits" the flags that were used to build it and passes them to setuptools.
- Turns out I wasn't using asdf's build but uv's build (possibly from a while ago) because I had .python-version in my project.
- After rebuilding the .venv and specifying what python binary to use with `--python $(asdf which python)` it works now.

---
