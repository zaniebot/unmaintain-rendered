```yaml
number: 8092
title: "uv init test fails if a directory ~<user>/.venv with no pyvenv.cfg is found"
type: issue
state: closed
author: pelamlio
labels:
  - question
assignees: []
created_at: 2024-10-10T13:56:42Z
updated_at: 2024-11-13T03:53:58Z
url: https://github.com/astral-sh/uv/issues/8092
synced_at: 2026-01-12T15:59:19Z
```

# uv init test fails if a directory ~<user>/.venv with no pyvenv.cfg is found

---

_@pelamlio_

uv version 0.4.20
Linux Ubuntu 24.04.1 LTS 

When transitioning (previously using venv, not virtualenv), uv does not find any pyvenv.cfg file directly under the .venv directory and generates an error:

uv init test
error: Broken virtualenv `/home/<user>/.venv`: `pyvenv.cfg` is missing

Should the directory be removed or an empty pyvenv.cfg be created at the root of ~/.venv, the command completes as expected.

---

_Comment by @charliermarsh on 2024-10-10 14:01_

What do you find to be wrong here? In other words, what behavior would you expect to see?

---

_Label `question` added by @charliermarsh on 2024-10-10 14:01_

---

_Comment by @pelamlio on 2024-10-10 15:52_

Not sure whether I can provide a clean answer.

In part, my issue was related to migrating to uv from an pre-existing development environment, and having a very limited knowledge of uv.
Should other users take the same path, they might trigger the same behavior (subdirectories per env in the .venv directory).

Per the documentation (https://docs.astral.sh/uv/guides/projects/), it was not immediately clear that uv init would need  an venv to be defined ab initio. More so, when the .venv could be created in the said project later (same url; Project structure).

My 2 cents.

---

_Comment by @jramcast on 2024-10-14 11:33_

Same problem here. I personally use the `$HOME/.venv` directory to store regular venvs for other projects.

---

_Comment by @bluss on 2024-10-14 17:30_

I was thinking, here are some steps to reproduce:

```
mkdir myroot
cd myroot
uv venv .venv/a
uv venv .venv/b
uv init testproject
```

The error from `uv init` is:

```
error: Broken virtualenv `/home/user/myroot/.venv`: `pyvenv.cfg` is missing
```

This could be a bug because: there shouldn't be a problem with `.venv` and testproject coexisting, and I don't think there is a problem if they are created in opposite order?

---

_Closed by @zanieb on 2024-11-13 03:53_

---

_Closed by @zanieb on 2024-11-13 03:53_

---
