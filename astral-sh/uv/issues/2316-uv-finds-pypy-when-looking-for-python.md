```yaml
number: 2316
title: UV finds PyPy when looking for Python
type: issue
state: closed
author: henryiii
labels:
  - bug
  - pypy
assignees: []
created_at: 2024-03-09T05:37:25Z
updated_at: 2024-05-23T16:17:42Z
url: https://github.com/astral-sh/uv/issues/2316
synced_at: 2026-01-10T05:31:37Z
```

# UV finds PyPy when looking for Python

---

_Issue opened by @henryiii on 2024-03-09 05:37_

If you have both CPython and PyPy installed, it seems like `uv` can find the wrong one when you ask for `-p python3.7` (up to 3.10, since that's the highest version of PyPy):

```
nox > Command C:\Program Files (x86)\pipx\venvs\nox\Scripts\uv.exe venv -p python3.7 'D:\a\nox\nox\.nox\github_actions_all_tests-3-7' failed with exit code 1:
Using Python 3.7.13 interpreter at: C:\hostedtoolcache\windows\PyPy\3.7.13\x86\python3.7.exe
Creating virtualenv at: D:\a\nox\nox\.nox\github_actions_all_tests-3-7
uv::venv::creation

  � Failed to create virtualenv
  \u251c\u2500\u25b6 failed to copy file from
  \u2502   \\?\C:\hostedtoolcache\windows\PyPy\3.7.13\x86\venvwlauncher.exe to
  \u2502   \\?\D:\a\nox\nox\.nox\github_actions_all_tests-3-7\Scripts\python.exe
  \u2570\u2500\u25b6 The system cannot find the file specified. (os error 2)
```

Virutalenv finds the correct one. This happens on GitHub Actions in https://github.com/wntrblm/nox/actions/runs/8209739709/job/22455867659?pr=799, only tried Windows - https://github.com/wntrblm/nox/pull/799.

---

_Comment by @charliermarsh on 2024-03-09 12:47_

Is this "wrong"? We have two `python3.7.exe` binaries, but I guess the argument is that we should break ties in favor of CPython?

---

_Comment by @charliermarsh on 2024-03-09 13:05_

Does it work as expected if you install Python 3.7 in the `setup-python` step (but not PyPy)?

---

_Label `bug` added by @charliermarsh on 2024-03-09 13:05_

---

_Label `pypy` added by @charliermarsh on 2024-03-09 13:05_

---

_Comment by @henryiii on 2024-03-09 17:20_

python3.7 should not find pypy3.7. Some installs of PyPy (like GitHub Actions) include a "compatibility" python3.7 executable so that if it is "activated", then "python" works, but it shouldn't be discovered if you ask for `python3.7`, only `pypy3.7`.

I believe we are making `3.7-3.12, pypy3.9, pypy3.10` (don't remember the exact syntax for multiple activations in one step, it's something like that) available by setup-python, and are ~~not explicitly listing pypy3.7~~, but it's still finding it. (Which is also wrong for 3.9 and 3.10, we are not looking for pypy if we ask for `python`!)

Maybe the rule could be if `python*` is accompanied by `pypy*`, then it's really pypy and the python shims should be ignored?

Might be worth checking how virtualenv does this, as it handles it correctly.

---

_Comment by @henryiii on 2024-03-09 17:21_

Correction: Yes, we are activating everything: https://github.com/wntrblm/nox/blob/08813c3c6b0d2171c280bbfcf219d089a16d1ac2/.github/workflows/action.yml#L44

It's just the default that was pypy3.9 and pypy3.10, but we override it for our tests.

Note that that field gets transformed before it is fed to setup-python, since we supported activating multiple Pythons before the official action supported it, and our syntax was slightly different.

---

_Comment by @charliermarsh on 2024-03-09 17:34_

Ok, this makes sense. I wasn’t really familiar with how PyPy is typically used / executed. Shouldn’t be too hard to fix.

---

_Comment by @charliermarsh on 2024-03-09 17:36_

What about when a user does “—python 3.7”? In that case, we probably _do_ need to allow PyPy?

---

_Comment by @henryiii on 2024-03-09 17:43_

Generally PyPy is a special case and you should be explicitly asking for it. It isn’t a drop in replacement for CPython due to the fact many libraries don’t support it.

---

_Comment by @henryiii on 2024-03-09 17:45_

I think virtualenv, tox, nox all require PyPy in the search to include PyPy. Setup-Python too.

---

_Comment by @charliermarsh on 2024-03-09 19:19_

Sounds good, this is helpful, thanks Henry. I’ll change it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-10 00:50_

---

_Comment by @charliermarsh on 2024-03-10 03:23_

I can think of two approaches:

1. Always filter out `pypy` unless the search includes `pypy` (e.g., `--python pypy37`).
2. When given a query like `python37`, first search excluding `pypy`; if nothing is found, search including `pypy`.

Neither of these would apply when in a PyPy virtual environment, since in that case, we _do_ want to find the `python` shims.


---

_Comment by @charliermarsh on 2024-03-10 03:24_

The questions would be:

- Should `--python python37` _ever_ return PyPy? Like, if it's the only Python installed, should it be returned?
- Should `--python 3.7` _ever_ return PyPy?

I would probably start by only matching PyPy if the user is explicit, e.g., the `--python pypy37` case.

---

_Comment by @henryiii on 2024-03-10 04:22_

I’d say no. PyPy should only match if you ask for PyPy. But @gaborbernat would have more experience and good advice here.

---

_Comment by @gaborbernat on 2024-03-10 04:49_


> - Should `--python python37` _ever_ return PyPy? Like, if it's the only Python installed, should it be returned?

Yes.

> - Should `--python 3.7` _ever_ return PyPy?
> 
> I would probably start by only matching PyPy if the user is explicit, e.g., the `--python pypy37` case.

Yes, if no other python is present.

The way I interpret it, python or the lack of the implementation basically translates to any implementation. However for historical reasons if multiple implementations are available cpython wins. 

This is what virtualenv does. 

---

_Comment by @henryiii on 2024-03-10 05:23_

Is `--python cpython37` valid? I've never specified that. A lot of packages don't support pypy.

>  if multiple implementations are available cpython wins. This is what virtualenv does.

I've never had pypy without cpython I guess. Matching virtualenv is fine.

So `py38, pypy38` (tox) or `"3.8", "pypy3.8"` (nox) may actually match pypy twice if CPython is missing?

---

_Comment by @gaborbernat on 2024-03-10 05:27_

> Is `--python cpython37` valid? I've never specified that. A lot of packages don't support pypy.

Yes. 
> So `py38, pypy38` (tox) or `"3.8", "pypy3.8"` (nox) may actually match pypy twice if CPython is missing?

Yes. 


---

_Comment by @henryiii on 2024-03-10 05:52_

Wow, thanks! Okay, so I think just matching virtualenv's CPython preference would be fine here. And I might start using "cpython". Need to check and see if that's supported everywhere in nox. :)

---

_Comment by @zanieb on 2024-05-22 14:54_

As of #3266 and #3706 we will discover all Python interpreters without regard for the implementation by default but allow opt-in to selecting only `cpython` or `pypy` implementations.

---

_Closed by @zanieb on 2024-05-22 14:54_

---

_Comment by @henryiii on 2024-05-23 05:35_

I still believe cpython should be chosen over pypy if there's a tie. It is very, very unlikely that PyPy is expected unless a user asked for PyPy, or if there is no matching CPython version. PyPy is not a drop in replacement for Python when it comes to packages; packages that provide wheels for CPython often have to build on PyPy if they are supported at all. This is also true for other implementations, I assume you'd at least be adding support for GraalPy soon, and again I think it should be opt-in unless it's the only matching interpreter.

---

_Comment by @zanieb on 2024-05-23 13:13_

@henryiii What does a tie mean? One has to come before the other in the PATH, right?

---

_Comment by @zanieb on 2024-05-23 13:15_

I'm pretty hesitant to do the approach where we search for _any_ CPython interpreter then fall back to other implementations like PyPy in a second search.

---

_Comment by @henryiii on 2024-05-23 16:17_

 My issue is that setting up multiple versions of Python:

```yaml
- uses: actions/setup-python@v5
  with:
    python-version: |
      3.10
      3.11
      3.12
      pypy3.10
```

Then running nox, build, or whatever requesting "3.10" may run pypy twice, getting pypy when requesting `3.10`. Virtualenv does this, selecting CPython over PyPy if both are found.

```python
@nox.session(
    python=[
        "3.10",
        "3.11",
        "3.12",
        "pypy3.10",
    ]
)
...
```

This also affects PEP 723, if you write:

```python
# /// script
# requires-python = "3.10.*"
# requires = ["numpy"]
# ///
```

This will either take several minutes to build or more likely fail if PyPy is selected, since numpy doesn't provide PyPy 3.10 wheels. (I'm not sure if they ever plan to, they claim PyPy is going away in the coming months)

---
