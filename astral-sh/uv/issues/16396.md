```yaml
number: 16396
title: "uv sync includes packages from excluded dependency group when using `--no-group`/`--only-group`"
type: issue
state: closed
author: jiito
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-10-21T17:59:49Z
updated_at: 2025-11-06T02:28:23Z
url: https://github.com/astral-sh/uv/issues/16396
synced_at: 2026-01-10T03:23:54Z
```

# uv sync includes packages from excluded dependency group when using `--no-group`/`--only-group`

---

_Issue opened by @jiito on 2025-10-21 17:59_

### Summary

When excluding a dependency group via --no-group or selecting only another group via --only-group, uv sync still includes packages from the excluded group. The resolver reports inclusion due to the project’s gpu group even when that group is explicitly excluded.

Use case: Install a subset of dependencies on Mac machine. 

## Minimal Reproducible Example:
https://github.com/jiito/uv-groups-reproduction 

- includes `-vvv` output logs

```bash
      rm -rf .venv uv.lock
      uv sync -vvv --no-group gpu
      rm -rf .venv uv.lock
      uv sync -vvv --only-group web
      rm -rf .venv uv.lock
      uv sync -vvv --no-default-groups --no-group gpu
```


## Expected behavior
When passing --no-group gpu, no packages from the gpu group should be resolved or installed.
When passing --only-group web, only web group packages should be resolved/installed.

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.9.4 (88f519a3b 2025-10-18)

### Python version

Python 3.11.14

---

_Label `bug` added by @jiito on 2025-10-21 17:59_

---

_Comment by @artpelling on 2025-10-24 09:55_

Facing the same issue with latest `uv` (0.9.5).

Steps to reproduce:

``` bash
uv init mwe
cd mwe
uv add typer
uv add --dev numpy
uv run python -c "import typer"  # works
uv run --no-dev python -c "import numpy"  # works but should not ?
```

---

_Comment by @charliermarsh on 2025-10-29 00:07_

@artpelling -- I think your case is `uv run` does an inexact sync by default (i.e., it will add packages as needed but avoids removing extraneous packages). If you add `--exact`, you should see the expected behavior:

```
❯ uv run --exact --no-dev python -c "import numpy"
Uninstalled 1 package in 40ms
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import numpy
ModuleNotFoundError: No module named 'numpy'
```

---

_Comment by @charliermarsh on 2025-10-29 00:17_

I can't reproduce this:
```
❯ uv sync --no-group gpu
Resolved 16 packages in 318ms
Prepared 3 packages in 214ms
Installed 11 packages in 17ms
 + annotated-doc==0.0.3
 + annotated-types==0.7.0
 + anyio==4.11.0
 + fastapi==0.120.1
 + idna==3.11
 + pydantic==2.12.3
 + pydantic-core==2.41.4
 + sniffio==1.3.1
 + starlette==0.49.1
 + typing-extensions==4.15.0
 + typing-inspection==0.4.2
```

(I changed to `gpu = ["requests"]` so that I could resolve on macOS.)

Are you just seeing that it's downloading the packages in the group and trying to build them? Or that it's actually installing them? uv has to download and run those packages in order to get their dependencies, since the authors don't publish static metadata. Once the lockfile is created, though, it shouldn't be running them or similar.


---

_Label `bug` removed by @charliermarsh on 2025-10-29 00:17_

---

_Label `question` added by @charliermarsh on 2025-10-29 00:17_

---

_Label `needs-mre` added by @charliermarsh on 2025-10-29 00:17_

---

_Comment by @artpelling on 2025-10-29 12:58_

> [@artpelling](https://github.com/artpelling) -- I think your case is `uv run` does an inexact sync by default (i.e., it will add packages as needed but avoids removing extraneous packages). If you add `--exact`, you should see the expected behavior:

Works perfectly. Thanks for the quick support!

---

_Closed by @zanieb on 2025-10-29 14:15_

---

_Comment by @jiito on 2025-11-06 02:28_

> Are you just seeing that it's downloading the packages in the group and trying to build them? Or that it's actually installing them? uv has to download and run those packages in order to get their dependencies, since the authors don't publish static metadata. Once the lockfile is created, though, it shouldn't be running them or similar.

I did not understand this nuance, my apologies for the mistake. Thanks for the guidance! 

For posterity:

To remedy this, I added a `dependency-metadata` field to my `pyproject.toml` following the suggestion in https://github.com/astral-sh/uv/issues/7985.

 

---
