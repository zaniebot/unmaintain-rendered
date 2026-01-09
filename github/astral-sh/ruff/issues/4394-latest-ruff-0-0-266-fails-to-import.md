---
number: 4394
title: Latest ruff 0.0.266 fails to import
type: issue
state: closed
author: jaraco
labels: []
assignees: []
created_at: 2023-05-12T18:21:46Z
updated_at: 2023-05-12T21:18:21Z
url: https://github.com/astral-sh/ruff/issues/4394
synced_at: 2026-01-07T13:12:14-06:00
---

# Latest ruff 0.0.266 fails to import

---

_Issue opened by @jaraco on 2023-05-12 18:21_

I was working on jaraco/jaraco.packaging#11 when suddenly the tests started failing with `ModuleNotFoundError`. I traced it to the recent 0.0.266 release:

```
 ~ $ pip-run -v ruff -- -c "import ruff"
Collecting ruff
  Using cached ruff-0.0.266-py3-none-macosx_10_9_x86_64.macosx_11_0_arm64.macosx_10_9_universal2.whl (10.1 MB)
Installing collected packages: ruff
Successfully installed ruff-0.0.266
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'ruff'
```

---

_Comment by @zanieb on 2023-05-12 18:23_

Confirmed this reproduces on `0.0.266` with `python -c "import ruff"` but not on `0.0.265`. @jaraco might be worth updating your title to note that just the Python import is broken. `ruff` runs fine.

---

_Renamed from "Latest ruff 0.0.266 is broken" to "Latest ruff 0.0.266 fails to import" by @jaraco on 2023-05-12 18:25_

---

_Comment by @jaraco on 2023-05-12 18:25_

I can see the binary there, but no library:

```
 ~ $ pip-run ruff
Python 3.11.3 (main, Apr  7 2023, 20:13:31) [Clang 14.0.0 (clang-1400.0.29.202)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', '/var/folders/sx/n5gkrgfx6zd91ymxr2sr9wvw00n8zm/T/pip-run-h27rhwxt', '/opt/homebrew/Cellar/python@3.11/3.11.3/Frameworks/Python.framework/Versions/3.11/lib/python311.zip', '/opt/homebrew/Cellar/python@3.11/3.11.3/Frameworks/Python.framework/Versions/3.11/lib/python3.11', '/opt/homebrew/Cellar/python@3.11/3.11.3/Frameworks/Python.framework/Versions/3.11/lib/python3.11/lib-dynload', '/opt/homebrew/Cellar/python@3.11/3.11.3/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages']
>>> import pathlib
>>> libs = pathlib.Path(sys.path[1])
>>> list(libs.iterdir())
[PosixPath('/var/folders/sx/n5gkrgfx6zd91ymxr2sr9wvw00n8zm/T/pip-run-h27rhwxt/bin'), PosixPath('/var/folders/sx/n5gkrgfx6zd91ymxr2sr9wvw00n8zm/T/pip-run-h27rhwxt/__pycache__'), PosixPath('/var/folders/sx/n5gkrgfx6zd91ymxr2sr9wvw00n8zm/T/pip-run-h27rhwxt/sitecustomize.py'), PosixPath('/var/folders/sx/n5gkrgfx6zd91ymxr2sr9wvw00n8zm/T/pip-run-h27rhwxt/ruff-0.0.266.dist-info')]
>>> list(libs.joinpath('bin').iterdir())
[PosixPath('/var/folders/sx/n5gkrgfx6zd91ymxr2sr9wvw00n8zm/T/pip-run-h27rhwxt/bin/ruff')]
```


---

_Comment by @jaraco on 2023-05-12 18:29_

Can I expect a rapid mitigation (fix or yank) or should I invest in working around the issue?

---

_Comment by @charliermarsh on 2023-05-12 18:31_

I'm surprised that `import ruff` worked before, but I'm guessing this is related to our Maturin upgrade? Gimme a few mins to look into it. Worst-case I will revert the Maturin change and cut a new release.

---

_Comment by @zanieb on 2023-05-12 18:34_

@charliermarsh looks like the `site-packages/ruff` directory is missing.

@jaraco why do you use `import ruff`?

---

_Comment by @edgarrmondragon on 2023-05-12 18:36_

fwiw this is causing problems for [`nbqa-ruff`](https://github.com/nbQA-dev/nbQA) too


---

_Comment by @charliermarsh on 2023-05-12 18:36_

Alright, I'll go ahead and revert while we figure this out.

`python -m ruff file.py` is also failing.

---

_Comment by @charliermarsh on 2023-05-12 18:38_

Yanked for now.

---

_Referenced in [astral-sh/ruff#4397](../../astral-sh/ruff/pulls/4397.md) on 2023-05-12 18:41_

---

_Referenced in [astral-sh/ruff#4398](../../astral-sh/ruff/pulls/4398.md) on 2023-05-12 18:48_

---

_Comment by @charliermarsh on 2023-05-12 18:51_

I'll have a new release out within the next hour.

---

_Comment by @jaraco on 2023-05-12 19:00_

> Yanked for now.

Super helpful and unblocked my work. Thanks for the super-rapid response.

---

_Comment by @charliermarsh on 2023-05-12 19:03_

Thanks for raising the issue so quickly, and apologies for the churn.

I did do some manual testing of the wheels prior to cutting the release, since we upgraded Maturin, but I didn't test the use of Ruff-as-module, and it turns out we didn't have any automated testing for that either, which @madkinsz has already improved :)

---

_Comment by @charliermarsh on 2023-05-12 21:06_

Fixed in v0.0.267.

---

_Closed by @charliermarsh on 2023-05-12 21:06_

---

_Comment by @jaraco on 2023-05-12 21:18_

> @jaraco why do you use `import ruff`?

I use [pytest-ruff](https://pypi.org/project/pytest-ruff), which [imports ruff](https://github.com/buserbrasil/pytest-ruff/blob/895a6dd2164e9d020cdf5f8c717b6216de6f825b/pytest_ruff.py#L4).

---
