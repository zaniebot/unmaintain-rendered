```yaml
number: 1401
title: "Add `uv pip list`"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - help wanted
  - compatibility
assignees: []
created_at: 2024-02-16T00:21:50Z
updated_at: 2024-02-26T11:33:09Z
url: https://github.com/astral-sh/uv/issues/1401
synced_at: 2026-01-10T05:40:31Z
```

# Add `uv pip list`

---

_Issue opened by @zanieb on 2024-02-16 00:21_

This is a really helpful command :)

---

_Label `enhancement` added by @zanieb on 2024-02-16 00:21_

---

_Label `compatibility` added by @zanieb on 2024-02-16 00:21_

---

_Comment by @matthewfeickert on 2024-02-16 04:04_

Yes please! :)

---

_Comment by @charliermarsh on 2024-02-16 04:06_

Wow I'm so used to `pip freeze`, no idea why!

Is it important we follow the exact same format? Do people read this programmatically?

---

_Comment by @Tarliton on 2024-02-16 04:23_

Maybe this can help?
https://github.com/pypa/pip/pull/752

---

_Comment by @matthewfeickert on 2024-02-16 04:23_

> Is it important we follow the exact same format? Do people read this programmatically?

Hm, good question. Problably not required to have the _exact_ same output, but I think plenty of people are doing `grep`/`awk`/`sed` stuff on the output of `pip list` (I know I am).

@charliermarsh as to why `pip list` when `pip freeze` exists, I would say the spacing choices are more (quickly) human readable and having the two column structure allows for parsing of _package_ (not version) easier.

```console
$ docker run --rm -ti python:3.12 /bin/bash
root@4106918bf8c2:/# python -m venv venv && . venv/bin/activate
(venv) root@4106918bf8c2:/# python -m pip --quiet install awkward
(venv) root@4106918bf8c2:/# python -m pip list
Package     Version
----------- --------
awkward     2.6.1
awkward-cpp 29
fsspec      2024.2.0
numpy       1.26.4
packaging   23.2
pip         24.0
(venv) root@4106918bf8c2:/# python -m pip freeze
awkward==2.6.1
awkward-cpp==29
fsspec==2024.2.0
numpy==1.26.4
packaging==23.2
(venv) root@4106918bf8c2:/#
```

For me, `pip list` is the goto and as an example of how I use it also programatically, here's how I clean up my virtual environment so that I can have clean installs when I'm updaing my GPU support enabled JAX environment

```bash
#!/bin/bash

python -m pip list | grep 'jax\|nvidia\|ml-dtypes' | awk '{print $1}' | xargs python -m pip uninstall --yes
python -m pip list

JAX_VERSION=0.4.24

# CUDA_VERSION=11
CUDA_VERSION=12

CUDA_DISTRIBUTION="pip"
# CUDA_DISTRIBUTION="local"

python -m pip install --upgrade "jax[cuda${CUDA_VERSION}_${CUDA_DISTRIBUTION}]==${JAX_VERSION}" --find-links https://storage.googleapis.com/jax-releases/jax_cuda_releases.html

python -m pip list | grep jax

echo ""
python jax_detect_GPU.py
```

---

_Comment by @zanieb on 2024-02-16 05:02_

Funny, I didn't know we had a `freeze` command 😶‍🌫️ 

---

_Comment by @ismail on 2024-02-16 07:45_

`pip list --outdated` is also a useful feature.

---

_Label `help wanted` added by @zanieb on 2024-02-17 05:40_

---

_Comment by @jab on 2024-02-17 15:56_

pip list also shows editable install locations (if any) as compared to pip freeze. 

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-19 16:12_

---

_Comment by @ghost on 2024-02-26 02:08_

There is also the `--format` option you might want to support.

>  --format <list_format>      Select the output format among: columns
                              (default), freeze, or json. The 'freeze' format
                              cannot be used with the --outdated option.

Then you could do `uv pip list --format=freeze`. The `json` format could be good for people who read this programmatically.

---

_Comment by @zanieb on 2024-02-26 04:38_

Is `--format=freeze` different than `pip freeze`? 

This was added in https://github.com/astral-sh/uv/pull/1662 so I'm going to close this now. We can track output formats in a new issue.

---

_Closed by @zanieb on 2024-02-26 04:38_

---

_Comment by @T-256 on 2024-02-26 11:33_

> Is `--format=freeze` different than `pip freeze`?
```shell
(uvtest) C:\Users\User\Desktop\uvtest> python -m pip list --format freeze
pip==24.0
setuptools==68.0.0
uvtest==0.1.0
wheel==0.42.0

(uvtest) C:\Users\User\Desktop\uvtest> python -m pip freeze
# Editable Git install with no remote (uvtest==0.1.0)
-e c:\users\user\desktop\uvtest
```
Yes, they are different, but I'd like to `pip list --format freeze` supersede `pip freeze`. tracked in #1970.

---
