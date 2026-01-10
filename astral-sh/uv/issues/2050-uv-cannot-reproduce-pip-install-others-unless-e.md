---
number: 2050
title: "`uv` cannot reproduce `pip install \".[others]\"` (unless `-e` is specified)"
type: issue
state: closed
author: kelvindecosta
labels:
  - duplicate
  - great writeup
assignees: []
created_at: 2024-02-28T18:32:40Z
updated_at: 2024-03-11T14:52:16Z
url: https://github.com/astral-sh/uv/issues/2050
synced_at: 2026-01-10T01:23:11Z
---

# `uv` cannot reproduce `pip install ".[others]"` (unless `-e` is specified)

---

_Issue opened by @kelvindecosta on 2024-02-28 18:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Overview

`uv` currently doesn't account for `pip install ".[dev]"`, a command used to install a local package/project with additional, in this case `dev`, dependencies defined in `pyproject.toml`.

Unless this type of installation is a bad practice, I think `uv` should be able to emulate this `pip` command.

## Actual Behavior

When running:

```
uv pip install ".[dev]"
```

the following output is shown:

```
error: Failed to parse `.[dev]`
  Caused by: Expected package name starting with an alphanumeric character, found '.'
.[dev]
^
```

With `--verbose`:

```
 uv::requirements::from_source source=.[dev]
error: Failed to parse `.[dev]`
  Caused by: Expected package name starting with an alphanumeric character, found '.'
.[dev]
^
```

It works flawlessly when using the `-e` flag:

```
 uv::requirements::from_source source=-e .[dev]
    0.005651s DEBUG uv_interpreter::virtual_env Found a virtualenv through CONDA_PREFIX at: /home/kelvin/.miniconda/envs/dotlas
    0.005755s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.9.16, skipping probing: /home/kelvin/.miniconda/envs/dotlas/bin/python
    0.005773s DEBUG uv::commands::pip_install Using Python 3.9.16 environment at /home/kelvin/.miniconda/envs/dotlas/bin/python
Audited 1 package in 16ms
```

## Expected Behavior

Running `pip install ".[dev]"` on its own works, so I'm assuming that `uv` should handle this too.

## System Info

```
# uname -a
Linux vision 5.10.179-1-MANJARO #1 SMP PREEMPT Wed Apr 26 19:18:39 UTC 2023 x86_64 GNU/Linux
```

```
# uv --version
uv 0.1.11
```

---

_Comment by @zanieb on 2024-02-28 18:35_

Thanks for the great writeup! This is a duplicate of #313 

---

_Closed by @zanieb on 2024-02-28 18:35_

---

_Label `duplicate` added by @zanieb on 2024-02-28 18:35_

---

_Label `great writeup` added by @zanieb on 2024-02-28 18:35_

---

_Comment by @kelvindecosta on 2024-02-28 20:06_

Ah sorry about that, I couldn't find it via the search. Thank you!

---

_Comment by @eirnym on 2024-03-10 13:16_

@zanieb it's not the duplicate of #313 as it's installing package `.` (which is supported) but specific section of dependencies. Please, reopen the issue

---

_Comment by @eirnym on 2024-03-10 13:18_

Also `-e` is not a mandatory key and it's not very useful if a person doesn't actively develop a package.

---

_Comment by @charliermarsh on 2024-03-10 13:28_

@eirnym -- I'm not sure what you're referring to exactly but yes, extras are supported just file with local paths:

```
pip install "black[d] @ scripts/editable-installs/black_editable"
```

---

_Comment by @eirnym on 2024-03-10 13:55_

if extras are supported and installing from local repository is supported, why `.[others]` is not supported?

---

_Comment by @charliermarsh on 2024-03-10 14:04_

Take a look at #313. We do not support `uv pip install .` right now -- if it's a non-editable install, it needs to include the package name syntax, like `uv pip install "my-project @ .`".

So, similarly if we don't support `uv pip install .`, then `uv pip install .[others]` wouldn't work either.

`uv pip install "my-project @ ."` and `uv pip install "my-project[others] @ ."` work.

---

_Comment by @eirnym on 2024-03-10 20:18_

Package name can be taken from pyproject/seruptools

---

_Comment by @eirnym on 2024-03-10 20:24_

By the way, package name is required as well as for `-e .` installation. 

---

_Comment by @zanieb on 2024-03-11 14:52_

We do not require the package name for `-e .` installs (we infer it there). We'll infer package names everywhere in #313.

---
