---
number: 3076
title: "Add `--prefix` flag for `uv pip install`"
type: issue
state: closed
author: jorng
labels:
  - compatibility
assignees: []
created_at: 2024-04-16T22:35:21Z
updated_at: 2024-06-06T20:15:30Z
url: https://github.com/astral-sh/uv/issues/3076
synced_at: 2026-01-10T01:23:24Z
---

# Add `--prefix` flag for `uv pip install`

---

_Issue opened by @jorng on 2024-04-16 22:35_

When performing multi-stage Docker builds, the `--prefix` flag is very useful to create an isolated install of dependencies that can then be copied to the final stage of a build.

Example with `pip`:
```Dockerfile
FROM python AS builder
# Possibly install prerequisites, gcc, etc, that aren't needed in runtime
COPY requirements.txt ./
RUN pip install -r requirements.txt --prefix /install
# Do other installs / builds...

FROM python:slim
COPY --from=builder /install /usr/local
# Other stuff as needed
```

Related:
- #1517 
- #2077 

Notably, this may need to be used in association with `--system` or `--python` to specify which python executables are pointed to.

---

_Label `compatibility` added by @zanieb on 2024-04-16 23:04_

---

_Comment by @ericmarkmartin on 2024-06-05 21:18_

We have a use for this at Jane Street. I have a first draft patch that seems to work.

Still need to write tests and think about windows though

---

_Comment by @zanieb on 2024-06-05 21:38_

Is `--target` insufficient?

---

_Comment by @charliermarsh on 2024-06-05 21:44_

What’s the use-case?

---

_Comment by @ericmarkmartin on 2024-06-06 00:20_

> Is `--target` insufficient?

Target dumps everything into the specified dir. Prefix implies a [certain `Scheme`](https://docs.python.org/3/library/sysconfig.html). Concretely, jupyterlab, for instance, includes plugins and such that should go into `share`, but `--target` will dump that stuff alongside the source.

From the linked doc:

> The “prefix scheme” is useful when you wish to use one Python installation to perform the build/install ..., but install modules into the third-party module directory of a different Python installation (or something that looks like a different Python installation).

---

_Comment by @charliermarsh on 2024-06-06 00:21_

What is the workflow though that demands `--prefix` as opposed to installing into an arbitrary Python environment via `--python`? `--prefix` has its own problems -- IIUC, scripts etc. will reference the parent Python interpreter, not the `--prefix` Python interpreter.

---

_Comment by @charliermarsh on 2024-06-06 00:28_

(By the way, I don't think `--prefix` is too hard to support, and I'm not really against it -- I just want to make sure that it's justified.)

---

_Comment by @charliermarsh on 2024-06-06 00:32_

Do you want advice on your draft? My hope is that you can just change `Interpreter::layout` to return paths joined against `self.virtualenv` rather than `self.scheme`.

---

_Referenced in [astral-sh/uv#4085](../../astral-sh/uv/pulls/4085.md) on 2024-06-06 02:26_

---

_Comment by @charliermarsh on 2024-06-06 02:31_

I put up a PR here with basic support: https://github.com/astral-sh/uv/pull/4085.

---

_Comment by @ericmarkmartin on 2024-06-06 03:39_

Oh wow that was quick. I was about to put mine up but will take a look at this first

---

_Comment by @charliermarsh on 2024-06-06 03:51_

Was hoping to save you some trouble, sorry if I stepped on your toes.

---

_Comment by @ericmarkmartin on 2024-06-06 04:12_

Haha no worries--thanks for the help!

---

_Closed by @charliermarsh on 2024-06-06 20:15_

---

_Referenced in [astral-sh/uv#9591](../../astral-sh/uv/issues/9591.md) on 2024-12-03 23:49_

---
