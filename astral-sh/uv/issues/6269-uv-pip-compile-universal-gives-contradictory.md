---
number: 6269
title: uv pip compile --universal gives contradictory versions of same packages
type: issue
state: closed
author: notatallshaw-gts
labels:
  - bug
  - resolver
assignees: []
created_at: 2024-08-20T18:54:24Z
updated_at: 2024-09-03T22:41:16Z
url: https://github.com/astral-sh/uv/issues/6269
synced_at: 2026-01-10T01:23:57Z
---

# uv pip compile --universal gives contradictory versions of same packages

---

_Issue opened by @notatallshaw-gts on 2024-08-20 18:54_

Affected versions: uv 0.2.30 to uv 0.3.0+

Steps to reproduce:

1. Create `requirements.in`:

```
alembic==1.8.1
ipython>=8.4.0
pylint>=2.14.5
```

2. Create `constraints.txt`:

```
dill==0.3.1.1
exceptiongroup==1.0.0rc8
```

3. Run the uv command:

```
$ uv pip compile requirements.in -c constraints.txt --universal --python-version 3.10 --annotation-style line 2>/dev/null | grep "astroid=="
astroid==2.13.5           # via pylint
astroid==3.2.4 ; python_full_version < '3.11'  # via pylint
```

Which, if you try and install on Python 3.10 gives you:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because you require astroid==2.13.5 and astroid{python_full_version < '3.11'}==3.2.4, we can conclude that your requirements are unsatisfiable.
```

---

_Label `resolver` added by @zanieb on 2024-08-20 18:55_

---

_Comment by @charliermarsh on 2024-08-20 18:55_

I'll look now.

---

_Label `bug` added by @zanieb on 2024-08-20 18:55_

---

_Comment by @notatallshaw-gts on 2024-08-20 18:56_

Very slightly changing the `requirements.in` or `constraints.txt`, or moving the constraints into the requirements produces the correct:

```
astroid==2.13.5 ; python_full_version >= '3.11'  # via pylint
astroid==3.2.4 ; python_full_version < '3.11'  # via pylint
```

---

_Comment by @charliermarsh on 2024-08-20 18:56_

Wow very interesting!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-20 18:57_

---

_Comment by @charliermarsh on 2024-08-20 18:59_

It looks like #6268 fixes this.

---

_Comment by @BurntSushi on 2024-08-20 18:59_

@charliermarsh How are you so fast?!?! Indeed, I just confirmed it's fixed in #6268 too:

```
$ cargo r --manifest-path ~/astral/uv/Cargo.toml -p uv -- pip compile requirements.in -c constraints.txt --universal --python-version 3.10 --annotation-style line 2>/dev/null | grep "astroid=="
astroid==2.13.5 ; python_full_version >= '3.11'  # via pylint
astroid==3.2.4 ; python_full_version < '3.11'  # via pylint
```

---

_Comment by @zanieb on 2024-08-20 19:01_

He has two clones of the repo so he can do twice as much!

---

_Comment by @charliermarsh on 2024-08-20 19:01_

Correct

---

_Referenced in [astral-sh/uv#6268](../../astral-sh/uv/pulls/6268.md) on 2024-08-20 19:03_

---

_Referenced in [astral-sh/uv#6412](../../astral-sh/uv/issues/6412.md) on 2024-08-22 06:59_

---

_Closed by @BurntSushi on 2024-09-03 22:41_

---

_Closed by @BurntSushi on 2024-09-03 22:41_

---
