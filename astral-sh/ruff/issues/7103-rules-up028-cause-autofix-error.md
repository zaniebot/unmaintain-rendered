```yaml
number: 7103
title: Rules UP028 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:47:02Z
updated_at: 2023-09-03T21:20:10Z
url: https://github.com/astral-sh/ruff/issues/7103
synced_at: 2026-01-10T11:09:49Z
```

# Rules UP028 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 18:47_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select UP028 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def _serve_method(fn):
            for h in (
                TaggedText.from_file(args.input)
                .markup(highlight=args.region)
            ):
                yield h
```

error
```
/home/rafal/test/tmp_folder/4187532.py:2:13: UP028 Replace `yield` over `for` loop with `yield from`
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/4187532.py`, the rule codes UP028, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```



[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506950/python_compressed.zip)


---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Label `bug` added by @charliermarsh on 2023-09-03 21:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 21:09_

---

_Closed by @charliermarsh on 2023-09-03 21:20_

---
