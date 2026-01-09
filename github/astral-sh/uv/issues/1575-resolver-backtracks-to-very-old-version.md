---
number: 1575
title: Resolver backtracks to very old version unnecessarily 
type: issue
state: closed
author: hynek
labels:
  - resolver
  - great writeup
assignees: []
created_at: 2024-02-17T05:23:13Z
updated_at: 2024-07-09T09:30:54Z
url: https://github.com/astral-sh/uv/issues/1575
synced_at: 2026-01-07T13:12:16-06:00
---

# Resolver backtracks to very old version unnecessarily 

---

_Issue opened by @hynek on 2024-02-17 05:23_

I've noticed that when installing packages that contain type hints, those hints don't get installed.

For example take https://github.com/hynek/svcs

If I install the dev dependencies using `uv pip install --reinstall --editable .[dev]` I get Mypy and FastAPI installed.

But, if I run `mypy tests/typing/fastapi.py`, I get:

```
tests/typing/fastapi.py:12: error: Skipping analyzing "fastapi": module is installed, but missing library stubs or py.typed marker  [import-untyped]
tests/typing/fastapi.py:13: error: Skipping analyzing "fastapi.responses": module is installed, but missing library stubs or py.typed marker  [import-untyped]
tests/typing/fastapi.py:13: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
tests/typing/fastapi.py:60: error: Untyped decorator makes function "view" untyped  [misc]
tests/typing/fastapi.py:69: error: Untyped decorator makes function "view2" untyped  [misc]
Found 4 errors in 1 file (checked 1 source file)
```

Running `pip install --force-reinstall fastapi` fixes it:

```
Success: no issues found in 1 source file
```

Between the installations, I can observe how `$VIRTUAL_ENV/lib/python3.12/site-packages/fastapi/py.typed` is missing and appears after installing it using pip. If I reinstall using *uv*, it vanishes again.

---

```console
❯ uv --version
uv 0.1.3
❯ python -m platform
macOS-14.3.1-arm64-arm-64bit
```

---

_Comment by @charliermarsh on 2024-02-17 05:25_

Oh wow, that's interesting.

---

_Label `bug` added by @zanieb on 2024-02-17 05:26_

---

_Label `help wanted` added by @zanieb on 2024-02-17 05:26_

---

_Comment by @zanieb on 2024-02-17 05:26_

Thank you! 

---

_Label `great writeup` added by @zanieb on 2024-02-17 05:28_

---

_Comment by @hynek on 2024-02-17 05:32_

hey running into weird edge cases is my only talent ;)

---

_Comment by @charliermarsh on 2024-02-17 05:40_

I haven't figured out why yet, but something in the requirements is leading us to choose a _really_ old version of FastAPI. Like, it looks like we first try to pin `starlette`, but we start with a version that's too new for FastAPI, so then we try all the FastAPI versions, and maybe we eventually find a FastAPI version that doesn't require `starlette` _at all_.

---

_Comment by @charliermarsh on 2024-02-17 05:41_

E.g., if you change the `starlette` requirement to `starlette==0.36.3`, you end up with a much better FastAPI version. Will need to think on how to hint to the resolver that this is bad.

---

_Comment by @hynek on 2024-02-17 05:44_

Uh you're right. If I install using `.[dev]` I get `fastapi==0.1.17`. If I install `uv pip install fastapi` I get `fastapi==0.109.2`

(I'm gonna leave editing of the bug title to you)

---

_Label `help wanted` removed by @zanieb on 2024-02-17 05:49_

---

_Label `resolver` added by @zanieb on 2024-02-17 05:49_

---

_Renamed from "py.typed file markers are not installed" to "Resolver backtracks to very old version unnecessarily " by @zanieb on 2024-02-17 05:50_

---

_Comment by @abextm on 2024-02-17 07:36_

I have something similar happening with `torchvision`, but only when installed from the cu118 repo. Running `uv pip install torch torchvision --verbose --index-url https://download.pytorch.org/whl/cu118` fails. It looks like it is repeatedly selecting older versions until it hits one that has a dependency it can't resolve at all, at which point it bails.

[log.txt](https://github.com/astral-sh/uv/files/14318050/log.txt)

```
$ uv --version
uv 0.1.3
$ python -m platform
Linux-6.7.4-arch1-1-x86_64-with-glibc2.39
```


---

_Comment by @charliermarsh on 2024-02-17 14:23_

Yeah, unfortunately uv is producing a valid resolution (I assume), it's just not identical to pip's because the resolver ends up taking a different path. (There are many valid resolutions, even under the constraint that you want to choose the latest available versions.) If we iterated over the packages in a different order, we might end up with the same thing. But I agree that the outcome here seems bad, and we should try to hint at the resolver to take a different path.

---

_Comment by @mpizenberg on 2024-02-17 20:39_

So this is a case where the resolution path produces a very undesirable solution, but another path produces a much more desirable one. Interesting. The main issue is probably that pubgrub doesn't explore the whole solution space and stops as soon as one solution is found. And as soon as one package is pinned it won't ever change the chosen version except if it reaches a dead-end and backtracks.

In a way, we can say that the pyproject is not correctly specified, because it says it's compatible with any fastapi version, which is not true. It's only that the current solver in pip chooses something that fits the bill. In my opinion the correct answer is to fix the dependency specification inside svcs.

---

_Comment by @mpizenberg on 2024-02-17 20:48_

However, if we think of things that can help, I imagine the order in which dependencies are specified could give an indication of the importance they have to the person who specified them. This way the priority of a package can be influenced by this order? It will definitely not generalize to all situations and for sure make the solver slower in general. So meh ...

---

_Comment by @notatallshaw on 2024-02-18 02:43_

> I haven't figured out why yet, but something in the requirements is leading us to choose a _really_ old version of FastAPI. Like, it looks like we first try to pin `starlette`, but we start with a version that's too new for FastAPI, so then we try all the FastAPI versions, and maybe we eventually find a FastAPI version that doesn't require `starlette` _at all_.

I see fastapi has an upper bound on starlette: https://github.com/tiangolo/fastapi/blob/master/pyproject.toml#L44

I will again suggest that it *might* be better when you have two requirements and one of the packages upper bounds the other requirements that you prefer backtracking on the package that is upper bounded: https://github.com/astral-sh/uv/issues/1398#issuecomment-1947572237

With the caveat that this is my intuition on what would produce better results and I've not had a chance to try and implement it myself yet and prove it.





---

_Comment by @notatallshaw on 2024-02-18 02:47_

> The main issue is probably that pubgrub doesn't explore the whole solution space and stops as soon as one solution is found

FYI, it's easy to produce a set of requirments where the whole solution space is larger than the number of atoms in the obserable universe. All real world dependency resolution algorithms will stop pretty quickly once they have a solution.

---

_Comment by @mpizenberg on 2024-02-18 16:22_

> All real world dependency resolution algorithms will stop pretty quickly once they have a solution.

Indeed, what I meant to say is that some solvers have a notion of "good" solution, like SMT solvers (such as microsoft Z3). But pubgrub does not. It only has local decision making strategies. So like "what should we try NOW"? but no notion of optimality of a solution.

---

_Label `bug` removed by @charliermarsh on 2024-02-22 03:01_

---

_Comment by @hynek on 2024-02-22 06:53_

Could it be that this has been fixed? Or is the fix just not general enough?

`pipx run --spec uv==0.1.6 uv pip install --reinstall -e .[dev]` gives me `fastapi==0.1.17`

`pipx run --spec uv==0.1.7 uv pip install --reinstall -e .[dev]` gives me `fastapi==0.109.2`

Which is as of writing the [latest version](https://pypi.org/project/fastapi/).

*svcs* which has quite… interesting… dependencies doesn't seem to flip-flop dependencies from a `pip install -Ue .[dev]` anymore:

<details>

```
Built 1 editable in 385ms
Resolved 55 packages in 17ms
Installed 55 packages in 45ms
 - aiohttp==3.9.3
 + aiohttp==3.9.3
 - aiosignal==1.3.1
 + aiosignal==1.3.1
 - annotated-types==0.6.0
 + annotated-types==0.6.0
 - anyio==4.3.0
 + anyio==4.3.0
 - attrs==23.2.0
 + attrs==23.2.0
 - blinker==1.7.0
 + blinker==1.7.0
 - cachetools==5.3.2
 + cachetools==5.3.2
 - certifi==2024.2.2
 + certifi==2024.2.2
 - chardet==5.2.0
 + chardet==5.2.0
 - click==8.1.7
 + click==8.1.7
 - colorama==0.4.6
 + colorama==0.4.6
 - distlib==0.3.8
 + distlib==0.3.8
 - fastapi==0.109.2
 + fastapi==0.109.2
 - filelock==3.13.1
 + filelock==3.13.1
 - flask==3.0.2
 + flask==3.0.2
 - frozenlist==1.4.1
 + frozenlist==1.4.1
 - h11==0.14.0
 + h11==0.14.0
 - httpcore==1.0.4
 + httpcore==1.0.4
 - httpx==0.27.0
 + httpx==0.27.0
 - hupper==1.12.1
 + hupper==1.12.1
 - idna==3.6
 + idna==3.6
 - iniconfig==2.0.0
 + iniconfig==2.0.0
 - itsdangerous==2.1.2
 + itsdangerous==2.1.2
 - jinja2==3.1.3
 + jinja2==3.1.3
 - markupsafe==2.1.5
 + markupsafe==2.1.5
 - multidict==6.0.5
 + multidict==6.0.5
 - mypy==1.8.0
 + mypy==1.8.0
 - mypy-extensions==1.0.0
 + mypy-extensions==1.0.0
 - packaging==23.2
 + packaging==23.2
 - pastedeploy==3.1.0
 + pastedeploy==3.1.0
 - plaster==1.1.2
 + plaster==1.1.2
 - plaster-pastedeploy==1.0.1
 + plaster-pastedeploy==1.0.1
 - platformdirs==4.2.0
 + platformdirs==4.2.0
 - pluggy==1.4.0
 + pluggy==1.4.0
 - pydantic==2.6.1
 + pydantic==2.6.1
 - pydantic-core==2.16.2
 + pydantic-core==2.16.2
 - pyproject-api==1.6.1
 + pyproject-api==1.6.1
 - pyramid==2.0.2
 + pyramid==2.0.2
 - pytest==8.0.1
 + pytest==8.0.1
 - pytest-asyncio==0.23.5
 + pytest-asyncio==0.23.5
 - setuptools==69.1.0
 + setuptools==69.1.0
 - sniffio==1.3.0
 + sniffio==1.3.0
 - starlette==0.36.3
 + starlette==0.36.3
 - svcs==24.1.1.dev7 (from file:///Users/hynek/FOSS/svcs)
 + svcs==24.1.1.dev7 (from file:///Users/hynek/FOSS/svcs)
 - sybil==6.0.3
 + sybil==6.0.3
 - tox==4.13.0
 + tox==4.13.0
 - translationstring==1.4
 + translationstring==1.4
 - typing-extensions==4.9.0
 + typing-extensions==4.9.0
 - venusian==3.1.0
 + venusian==3.1.0
 - virtualenv==20.25.1
 + virtualenv==20.25.1
 - webob==1.8.7
 + webob==1.8.7
 - werkzeug==3.0.1
 + werkzeug==3.0.1
 - yarl==1.9.4
 + yarl==1.9.4
 - zope-deprecation==5.0
 + zope-deprecation==5.0
 - zope-interface==6.2
 + zope-interface==6.2
```

</details>

---

_Comment by @bossjones on 2024-05-04 18:44_

> I have something similar happening with `torchvision`, but only when installed from the cu118 repo. Running `uv pip install torch torchvision --verbose --index-url https://download.pytorch.org/whl/cu118` fails. It looks like it is repeatedly selecting older versions until it hits one that has a dependency it can't resolve at all, at which point it bails.
> 
> [log.txt](https://github.com/astral-sh/uv/files/14318050/log.txt)
> 
> ```
> $ uv --version
> uv 0.1.3
> $ python -m platform
> Linux-6.7.4-arch1-1-x86_64-with-glibc2.39
> ```

This just saved my life, i've been going crazy all day playing with rye + uv couldn't figure out why it was holding back all my packages! Thanks so much! I might make a pr over at `rye`, maybe in FAQ section, that describes this scenario a bit 

---

_Comment by @konstin on 2024-07-09 08:30_

I've checked https://github.com/hynek/svcs at 85827a173fd7b9952abf34a8f198c7d1fb40f43d with the latest version of uv and the original problems don't occur anymore (`mypy tests/typing/fastapi.py` passes successfully) and `pip install --force-reinstall fastapi` gives us the same versions as `uv pip install --reinstall --editable .[dev]`. I think our prioritization changes fix this.

---

_Comment by @hynek on 2024-07-09 09:24_

just for completeness, as I wrote in https://github.com/astral-sh/uv/issues/1575#issuecomment-1958816302 it already worked before, but @charliermarsh mentioned it might've just been due to FastAPI releasing something new with different version constraints or something? Not saying, it's not fixed – just saying that *svcs* isn't a useful signal anymore. :)

---

_Comment by @konstin on 2024-07-09 09:30_

I've tested with `uv pip install --reinstall --editable .[dev] --exclude-newer 2024-02-17` so i'm confident this is fixed in uv rather than by external circumstances :)

---

_Closed by @konstin on 2024-07-09 09:30_

---

_Referenced in [astral-sh/uv#8157](../../astral-sh/uv/issues/8157.md) on 2024-10-13 13:03_

---
