---
number: 9068
title: UV 0.5.1 fails to build xgboost 
type: issue
state: closed
author: blbuford
labels:
  - cache
  - external
assignees: []
created_at: 2024-11-12T18:15:55Z
updated_at: 2024-12-02T02:17:32Z
url: https://github.com/astral-sh/uv/issues/9068
synced_at: 2026-01-07T13:12:18-06:00
---

# UV 0.5.1 fails to build xgboost 

---

_Issue opened by @blbuford on 2024-11-12 18:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Here's a minimum dockerfile to reproduce the error: 
```Dockerfile
FROM python:3.11.9-slim-bookworm
RUN apt-get update \
    && apt-get install -y git build-essential cmake --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*
RUN pip install uv==0.5.1
RUN uv pip install --system --no-cache --no-binary xgboost xgboost==1.5.2
```
Changing that `uv==0.5.1` to `uv==0.5.0` makes the `docker build -f tmp/uv-fails.Dockerfile  .` pass. Also up-to-date pip works as well, so I suspect there's something with the newest version of uv. 

From what I can see about the error, somewhere in the compilation uv makes some god-awful looking path that exceeds 4096 characters and looks like: 
```
/tmp/.tmphc3YDw/sdists-v6/pypi/xgboost/1.5.2/zwaKdpTMLMT2TpKaD0bTH/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/build/temp.linux-x86_64-cpython-311/xgboost/src/xgboost/dmlc-core/src/data/basic_row_iter.h
```
Let me know if you need any more info! Thanks for taking a look.

---

_Comment by @zanieb on 2024-11-12 18:17_

Hm we changed some build behaviors in an attempt to make the file paths _shorter_, cc @charliermarsh 

---

_Label `bug` added by @zanieb on 2024-11-12 18:17_

---

_Label `cache` added by @zanieb on 2024-11-12 18:17_

---

_Comment by @charliermarsh on 2024-11-12 18:30_

It may not be related -- the path here seems to be repeated infinitely for some reason (`src/build/temp.linux-x86_64-cpython-311/xgboost`).

---

_Comment by @zanieb on 2024-11-12 18:49_

Hm I can't think of another reason it'd regress though?

---

_Comment by @charliermarsh on 2024-11-12 18:52_

I didn't see that it regressed. You're right, it's probably related.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-12 19:06_

---

_Comment by @charliermarsh on 2024-11-12 19:08_

So I don't really understand the problem yet, but if you change the directory name from `src` to `xyz` in our build cache, it works...

---

_Comment by @charliermarsh on 2024-11-12 19:10_

I think the problem is that they seem to create some sort of scratch directory at `../src`. But we build from a directory named `src`. So it's specific to XGBoost...

---

_Comment by @zanieb on 2024-11-12 19:12_

That's pretty particular. It seems naughty of them to escape their build directory like that. I think it's worth opening an upstream issue?

---

_Comment by @charliermarsh on 2024-11-12 19:13_

I might be misunderstanding, it could be a problem on our end. Need to look at the source.

---

_Referenced in [astral-sh/uv#9069](../../astral-sh/uv/pulls/9069.md) on 2024-11-12 19:18_

---

_Comment by @charliermarsh on 2024-11-12 19:23_

Ah actually, I can't replicate this with latest XGBoost:

```docker
FROM python:3.11.9-slim-bookworm
RUN apt-get update \
    && apt-get install -y git build-essential cmake --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*
RUN pip install uv==0.5.1
RUN uv pip install --system --no-cache --no-binary xgboost xgboost
```

I think this is really an issue with XGBoost that seems to have been fixed. I'm a little hesitant to make changes in light of that.

---

_Comment by @blbuford on 2024-11-12 19:50_

Bummer. Why does up-to-date pip work on that, but uv does not? E.g. this works: 
```Dockerfile
FROM python:3.11.9-slim-bookworm
RUN apt-get update \
    && apt-get install -y git build-essential cmake --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*
RUN pip install --upgrade pip && pip install --no-cache --no-binary xgboost xgboost==1.5.2
```

---

_Comment by @zanieb on 2024-11-12 19:56_

They probably do not use the same build directory naming scheme as us.

---

_Label `bug` removed by @charliermarsh on 2024-11-22 02:41_

---

_Label `upstream` added by @charliermarsh on 2024-11-22 02:41_

---

_Comment by @charliermarsh on 2024-12-02 02:17_

I'm going to close this as `wontfix`. Sorry about that -- I'd just prefer to avoid implementing custom workarounds for older versions of specific packages, and changing the directory name here does have some cost.

---

_Closed by @charliermarsh on 2024-12-02 02:17_

---
