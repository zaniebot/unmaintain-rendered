```yaml
number: 2109
title: "uv doesn't detect pyenv-virtualenv shim environments"
type: issue
state: closed
author: jarshwah
labels: []
assignees: []
created_at: 2024-03-01T11:22:25Z
updated_at: 2025-02-11T23:27:59Z
url: https://github.com/astral-sh/uv/issues/2109
synced_at: 2026-01-10T03:50:29Z
```

# uv doesn't detect pyenv-virtualenv shim environments

---

_Issue opened by @jarshwah on 2024-03-01 11:22_

`uv` doesn't seem able to detect the virtual-environment created by [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv).

uv version and repro:

```
$ pyenv virtualenv 3.10.2 myenv
$ pyenv local myenv
$ which python
/Users/josh/.pyenv/shims/python

$ uv pip install pip==22.0.3
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.

$ uv --version
uv 0.1.13 (9ce5170e6 2024-02-29)

$ python --version
Python 3.10.12
```

But it does detect the virtualenv when activating the shell:

```
$ pyenv activate kraken-uv
$ uv pip install pip==22.0.3
Resolved 1 package in 312ms
Downloaded 1 package in 168ms
Installed 1 package in 8ms
 - pip==23.0.1
 + pip==22.0.3
```


---

_Renamed from "uv doesn't detect pyenv-virtualenv environments" to "uv doesn't detect pyenv-virtualenv shim environments" by @jarshwah on 2024-03-01 11:24_

---

_Comment by @jarshwah on 2024-03-18 00:27_

Workaround for those that are interested, using [direnv](https://direnv.net/):

```
if [ -f ".python-version" ] ; then
    envname=$(cat .python-version)
    VIRTUAL_ENV="$(pyenv root)/versions/$envname"
    export VIRTUAL_ENV
fi
```

---

_Comment by @NeilGirdhar on 2024-04-11 14:42_

How about `VIRTUAL_ENV="$(pyenv root)/versions/$(pyenv version-name)"`?

---

_Comment by @zanieb on 2024-07-01 21:55_

Would be closed by https://github.com/astral-sh/uv/pull/4032

---

_Comment by @zanieb on 2025-02-04 15:47_

per https://github.com/astral-sh/uv/issues/4009#issuecomment-2465923877 I think this was fixed, though it may have just regressed in #11214 

---

_Closed by @zanieb on 2025-02-04 15:47_

---

_Comment by @dpoznik on 2025-02-05 01:08_

> per [#4009 (comment)](https://github.com/astral-sh/uv/issues/4009#issuecomment-2465923877) I think this was fixed, though it may have just regressed in [#11214](https://github.com/astral-sh/uv/issues/11214)

Confirmed. #4009 was fixed but regressed in `uv` 0.5.27.
 

---

_Comment by @dpoznik on 2025-02-05 21:24_

> Confirmed. [#4009](https://github.com/astral-sh/uv/issues/4009) was fixed but regressed in `uv` 0.5.27.

I see that this works again in 0.5.28. Thanks again for the quick fix!


---

_Comment by @zanieb on 2025-02-05 21:26_

Yep no problem! Thanks for following up.

---

_Comment by @fjarri on 2025-02-11 23:27_

In 0.5.30 the sequence of actions from the top post works fine, and you can see that the venv was picked up:
```
> uv pip list
Using Python 3.10.10 environment at: <...>.pyenv/versions/myenv
Package    Version
---------- -------
pip        22.0.3
setuptools 65.5.0
```

But then running `uv venv` results in 
```
  × No interpreter found for executable name `myenv` in managed installations or search path
```

Is it the same issue reoccurring, or should I make a new one?

---
