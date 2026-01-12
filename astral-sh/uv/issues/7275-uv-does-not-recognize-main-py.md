```yaml
number: 7275
title: uv does not recognize __main__.py?
type: issue
state: closed
author: seppzer0
labels:
  - enhancement
assignees: []
created_at: 2024-09-10T20:33:40Z
updated_at: 2025-09-30T11:25:55Z
url: https://github.com/astral-sh/uv/issues/7275
synced_at: 2026-01-12T15:59:12Z
```

# uv does not recognize __main__.py?

---

_@seppzer0_

I have a Python project that I run directly from source. Dependencies are stored within a uv-managed .venv in the project directory.

When I attempt to launch it as I normally would with `PYTHONPATH=$(pwd) python3 builder/` via my system installation, I get an error:

```txt
$ PYTHONPATH=$(pwd) uv run ./builder
error: Failed to spawn: `./builder`
  Caused by: Access is denied. (os error 5)
```

However, if I just add the `__main__.py` in my command:

```txt
$ PYTHONPATH=$(pwd) uv run ./builder/__main__.py 
usage: __main__.py [-h] [--clean] {kernel,assets,bundle} ...

A custom builder for the zero kernel.

positional arguments:
  {kernel,assets,bundle}
    kernel              build the kernel
    assets              collect assets
    bundle              build the kernel + collect assets

options:
  -h, --help            show this help message and exit
  --clean               clean the root directory
```

..it runs successfully.

Would this be an intended behaviour or is this a bug?


---

_Comment by @charliermarsh on 2024-09-10 21:30_

Does `uv run python ./builder` work as expected?

---

_Comment by @seppzer0 on 2024-09-10 21:41_

Interestingly, it does. I don't understand the difference yet, but adding `python` works.

---

_Comment by @zanieb on 2024-09-10 22:23_

In the error case, we've attempted to run a directory as though it were an executable. In the latter, you've invoked Python to run the module.

We should probably special case directories with `__main__.py` though (and support what you attempted to do)

---

_Label `enhancement` added by @zanieb on 2024-09-10 22:23_

---

_Closed by @zanieb on 2024-09-11 13:18_

---

_Comment by @ghpqans on 2025-09-30 11:24_

Same problem here. Feels uncomfortable to enter sth like `uv run src/projectname/__main__.py` . Script section in `pyproject.toml` also does not support `__main.py__`.

---

_Comment by @konstin on 2025-09-30 11:25_

@ghpqans This issue has been solved, please open a new issue following #9452.

---
