---
number: 7126
title: "uv sync in a Docker container doesn't copy the project"
type: issue
state: closed
author: hynek
labels: []
assignees: []
created_at: 2024-09-06T13:33:05Z
updated_at: 2024-09-06T19:03:53Z
url: https://github.com/astral-sh/uv/issues/7126
synced_at: 2026-01-10T01:24:11Z
---

# uv sync in a Docker container doesn't copy the project

---

_Issue opened by @hynek on 2024-09-06 13:33_

Sorry for the opaque title, but since you mentioned on Discord you couldn't reproduce, so I'm just describing what I'm seeing.

I've created a minimal repro for you at https://github.com/hynek/uv-reproducer – you can start the build by running `just` if you've installed it.

Due to the `/app/bin/python -Ic 'import tc'` at the end, the build fails.

The ls on the line before looks like this:

```
#17 0.188 drwxr-xr-x 1 root root  192 Sep  6 13:18 .
#17 0.188 drwxr-xr-x 1 root root   26 Sep  6 13:17 ..
#17 0.188 drwxr-xr-x 1 root root   54 Sep  6 13:17 __pycache__
#17 0.188 -rw-r--r-- 1 root root    8 Sep  6 13:18 _tc.pth
#17 0.188 -rw-r--r-- 1 root root   18 Sep  6 13:17 _virtualenv.pth
#17 0.188 -rw-r--r-- 1 root root 4342 Sep  6 13:17 _virtualenv.py
#17 0.188 drwxr-xr-x 1 root root  560 Sep  6 13:17 attr
#17 0.188 drwxr-xr-x 1 root root  202 Sep  6 13:17 attrs
#17 0.188 drwxr-xr-x 1 root root   90 Sep  6 13:17 attrs-24.2.0.dist-info
#17 0.188 drwxr-xr-x 1 root root  104 Sep  6 13:18 tc-0.1.0.dist-info
```

So the deps sync works, but from the app pkg there's just dist-info.

---

If I replace:

```
cd /src
uv sync --frozen --no-dev
```

by

```
uv pip install \
     --python=$UV_PROJECT_ENVIRONMENT \
     --no-deps \
     /src
```

it works.

---

_Comment by @zanieb on 2024-09-06 13:37_

My first guess is that this is because you're doing something weird with your `pyproject.toml`? What is the output for the final sync?

---

_Comment by @hynek on 2024-09-06 13:38_

this is the default pyproject that uv created + build-system. It's all in the repo: https://github.com/hynek/uv-reproducer/blob/main/pyproject.toml


```
#15 [build 7/7] RUN --mount=type=cache,target=/root/.cache <<EOT (cd /src...)
#15 0.101 + cd /src
#15 0.101 + uv sync --frozen --no-dev
#15 0.813 Prepared 1 package in 605ms
#15 0.815 Installed 1 package in 1ms
#15 0.977 Bytecode compiled 20 files in 162ms
#15 0.978  + tc==0.1.0 (from file:///src)
#15 DONE 1.0s
```

---

_Comment by @zanieb on 2024-09-06 13:52_

By weird, I meant that you were copying it into a `_lock` directory but I guess that's actually undone at `COPY . /src`. I now suspect that hatchling is at fault, it has some peculiar behaviors around package source detection and will happily build an empty distribution. I can look into it later using the reproduction (thanks!) but have some other priorities at this moment.

---

_Comment by @hynek on 2024-09-06 14:17_

FWIW, switching `[build-system]` to

```
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```

or

```
[build-system]
requires = ["flit_core"]
build-backend = "flit_core.buildapi"
```

doesn't help either.

Interestingly, the former adds a `/app/lib/python3.12/site-packages/__editable__.tc-0.1.0.pth` containing:

```
/src/src
```

and flit creates a `/app/lib/python3.12/site-packages/tc.pth` containing the same, I think except without a trailing `\n`.

I've also just noticed that the `ls` output in my first post was truncated (Docker…) so I've updated it: Hatchling creates a `_tc.pth` too and it also contains `/src/src`.

These all look like editable installs to me.

---

To be clear: adding `/app/bin/python -Ic 'import tc'` behind `uv sync --frozen --no-dev` _does_ work.

HTH when this becomes a priority. The workaround is easy enough.

---

_Comment by @zanieb on 2024-09-06 15:01_

🤦 I didn't scroll down and see that there's a second layer. It looks like you're not copying the source code into the final image layer? That's why it doesn't work, since we're doing an editable install. The `uv pip` command you share that works doesn't use an editable install.

We're interested in adding non-editable install support (#5792) but I don't think this is intended to work as written today.

---

_Comment by @hynek on 2024-09-06 15:20_

heh yeah that’s what I’ve been trying to say in discord already but my English is still limited 🙈

given there’s https://github.com/astral-sh/uv/issues/5792 and it was all just a miscommunication, I guess we can close this?

---

_Comment by @zanieb on 2024-09-06 19:03_

Yeah haha let's track there. Sorry.

---

_Closed by @zanieb on 2024-09-06 19:03_

---
