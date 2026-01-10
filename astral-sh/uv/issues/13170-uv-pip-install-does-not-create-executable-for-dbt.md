---
number: 13170
title: uv pip install does not create executable for dbt
type: issue
state: closed
author: dnabb
labels:
  - compatibility
assignees: []
created_at: 2025-04-28T08:43:53Z
updated_at: 2025-09-22T09:47:33Z
url: https://github.com/astral-sh/uv/issues/13170
synced_at: 2026-01-10T01:25:29Z
---

# uv pip install does not create executable for dbt

---

_Issue opened by @dnabb on 2025-04-28 08:43_

### Summary

Not sure if this a bug or just me missing out some details about the [compativility with pip](https://docs.astral.sh/uv/pip/compatibility/), but I found that uv and pip behave differently when installing [dbt Cloud CLI ](https://docs.getdbt.com/docs/cloud/cloud-cli-installation?install=pip).

Starting from a clean environment and using pip to install dbt;
`uv venv --python 3.12 --seed`
`pip install dbt --no-cache-dir`
results in the creation of a `dbt.exe` executable inside .venv/Scripts, which is the expected behaviour.

If I instead I do the same thing using uv (no pip needed in that case):
`uv venv --python 3.12`
`uv pip install dbt --no-cache-dir`
dbt appears to be properly installed (shows up when doing `uv pip list`) but there is no `dbt.exe` available on the path (or anywhere else I could find).
This means that running `dbt --version` fails (the term 'dbt' is not recognized as a name of a cmdlet, function, script file, or executable program) and the dbt CLI is not usable as intended.

A (likely) consequence of this behaviour is that it's not possilbe to install dbt CLI as a tool:
`uv tool install dbt` > No executables are provided by `dbt`

I suspect this has something to do with the way dbt has configured their setup (which basically seems to just download the executable from their Github), but I couldn't find anything about this diverging behaviour in the [compatibility guide](https://docs.astral.sh/uv/pip/compatibility/#compatibility-with-pip-and-pip-tools), so I was hoping someone could explain what is really happening here and if there is something that can be done (by either the uv or dbt team) to address this.

### Platform

Windows 11

### Version

uv 0.6.17 (8414e9f3d 2025-04-25)

### Python version

Python 3.12.8

---

_Label `bug` added by @dnabb on 2025-04-28 08:43_

---

_Comment by @konstin on 2025-04-28 12:10_

I can confirm that `pip install` works and `uv pip install` fails. However, the `dbt` package has some non-standard `setup.py` assumptions about how a build is structured and from where files are included (`install_dir = Path(executable).parent` for unpacking the binary to). Even pip fails when using `pip wheel .` on the sources, so I consider this a bug in their packaging setup that relies on implementation details of `pip install`.

---

_Label `bug` removed by @konstin on 2025-04-28 12:10_

---

_Label `compatibility` added by @konstin on 2025-04-28 12:10_

---

_Comment by @jojo0094 on 2025-05-04 09:05_

Not sure if this is what you want. `uv add dbt-core` works for me. 

---

_Closed by @charliermarsh on 2025-05-04 12:41_

---

_Comment by @CrisNavasM on 2025-09-16 12:52_

@jojo0094 he's talking about [dbt](https://pypi.org/project/dbt/) **not** [dbt-core](https://pypi.org/project/dbt-core/)

Indeed I'm facing the same issues

---

_Comment by @zanieb on 2025-09-16 13:36_

The best path forward here is to get in contact with your support team at dbt. We can't fix this.

---

_Comment by @CrisNavasM on 2025-09-16 14:12_

<img width="678" height="233" alt="Image" src="https://github.com/user-attachments/assets/71916924-da63-455b-9b96-71d054e88156" />

Indeed, [Slack convo](https://getdbt.slack.com/archives/C03SAHKKG2Z/p1728342729085229?thread_ts=1728323878.624179&cid=C03SAHKKG2Z) for further search queries that land into here

---

_Comment by @zanieb on 2025-09-16 14:20_

`uv add dbt` should work still and is a reasonable way to lock the dependency in your project. It's _is_ a Python package.

---

_Comment by @konstin on 2025-09-16 15:22_

For technical context on why dbt fails to install in some cases: The dbt source distribution has a build script that downloads the dbt binary into `Path(sys.executable).parent`. This approach works with `pip install` where `sys.executable` points to the Python of the target venv, but it doesn't work with fully isolated build nor with builds that persist only the wheel.

These failure paths can also surface with pip, for example `pip install dbt` in a venv in a different environment with the same Python version produces a similarly non-functional dbt install that lacks the `dbt` executable.

---

_Comment by @notatallshaw on 2025-09-16 15:53_

> This approach works with `pip install` where `sys.executable` points to the Python of the target venv, but it doesn't work with fully isolated build nor with builds that persist only the wheel.

And pip will likely change behavior to use fully isolated builds eventually, it's a matter of an interested maintainer with enough time to make that change. 

---

_Comment by @zanieb on 2025-09-16 18:42_

I've now had an extended conversation with someone from dbt (in the linked Slack thread) and they'll raise this with their engineering team.

---
