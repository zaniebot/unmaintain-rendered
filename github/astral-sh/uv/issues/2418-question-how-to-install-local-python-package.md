---
number: 2418
title: "Question: How to install local python package?"
type: issue
state: closed
author: mkleinbort-ic
labels:
  - question
assignees: []
created_at: 2024-03-13T15:14:23Z
updated_at: 2025-10-12T16:19:03Z
url: https://github.com/astral-sh/uv/issues/2418
synced_at: 2026-01-07T13:12:17-06:00
---

# Question: How to install local python package?

---

_Issue opened by @mkleinbort-ic on 2024-03-13 15:14_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hi, just struggling with the uv pip install syntax...

I have a folder called `src/` with a sub folder `projectlib` which itself contains a bunch of .py files and folders of .py files

The directory structure is more or less:

```
PROJECT-SOME-RANDOM-NAME/
   |-> pyproject.toml
   |-> notebooks/...
   └-> src/
         |-> projectlib/
         |    |-> __init__.py
         |    |-> data_loading.py
         |    |-> models/...
         |    |-> features.py
         |    └-> analysis/...
         |
         └> otherlib/...
```

Then in a bunch of jupyter notebooks and scripts I simply import things as 

```python
from projectlib.data_loading import get_users_from_db
```

So far I've been installing this via 

`python -m pip install --user .`

ran on the top level folder `PROJECT-SOME-RANDOM-NAME` (outside `src`)

But looking to move to `uv` (for the obvious reasons)

I see I can install all the dependencies described in `pyproject.toml` using 

`python -m uv pip install --system --prerelease=allow -r pyproject.toml`

But not clear on how I can install the `projectlib/` folder (and maybe also `otherlib/`)


---

_Comment by @zanieb on 2024-03-13 15:16_

Hi! You're looking for `uv pip install -e .` or `uv pip install "your-project@."`

See #313 for some more details on the second syntax.

---

_Label `question` added by @zanieb on 2024-03-13 15:16_

---

_Comment by @mkleinbort-ic on 2024-03-13 15:25_

Yes, that worked.

---

_Closed by @mkleinbort-ic on 2024-03-13 15:25_

---

_Comment by @mkleinbort-ic on 2024-03-15 02:23_

Ok, just in case someone comes across this issue... 

I had the problem that when I modified `projectlib` some changes would not be applied. Meaning that after running 

`python -m uv pip install --system -e . `

I was still loading the previous version installed with `python -m pip install . `

The issue was that at some point pip had installed `projectlib` in  `~/AppData/Roaming/Python/Python310/site-packages/`

Deleting that folder fixed my issue - it had nothing to do with `uv`

---

_Comment by @charliermarsh on 2024-03-22 21:28_

This is supported as of `v0.1.24`. You can `uv pip install .` directly, without including a package name.

---

_Comment by @kazemihabib on 2025-03-12 18:08_

Is it possible to use a local python package when running a script using `uv run --isolated --no-project  --with` command?

---

_Comment by @bityob on 2025-05-04 18:33_

> Is it possible to use a local python package when running a script using `uv run --isolated --no-project --with` command?

Yes, you can like this - 
```
uv run --isolated --no-project --with . python

uv run --isolated --no-project --with /path/to/package/directory python

uv run --isolated --no-project --with /path/to/package/directory/foo-0.1.0-py3-none-any.whl python
```

Source:
https://docs.astral.sh/uv/concepts/projects/dependencies/#path

---

_Comment by @cpita-mutt on 2025-10-12 16:19_

> You're looking for uv pip install -e . or uv pip install "your-project@."

Is is possible to achieve the same thing but listing the package directory under dependencies as for everything else, so as to avoid manually calling uv pip install -e . ? 

---
