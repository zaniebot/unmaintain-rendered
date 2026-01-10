---
number: 11990
title: "`uv python find` does not respect `--project`"
type: issue
state: open
author: zanieb
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-03-05T21:47:32Z
updated_at: 2025-03-13T05:03:16Z
url: https://github.com/astral-sh/uv/issues/11990
synced_at: 2026-01-10T01:25:13Z
---

# `uv python find` does not respect `--project`

---

_Issue opened by @zanieb on 2025-03-05 21:47_

### Summary

```
❯ uv init example
❯ cd example
❯ uv sync
❯ uv python find
/Users/zb/workspace/uv/example/.venv/bin/python3
❯ cd ..
❯ uv python find
/Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
❯ uv python find --project example
/Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
```

This should discover the project environment

### Platform

n/a

### Version

0.6.4 (04db70662 2025-03-03)

### Python version

n/a

---

_Label `bug` added by @zanieb on 2025-03-05 21:47_

---

_Label `good first issue` added by @zanieb on 2025-03-05 21:47_

---

_Comment by @thejchap on 2025-03-06 01:21_

@zanieb i'll take a look at this one

---

_Comment by @zanieb on 2025-03-06 04:47_

Cool! Let me know if you need help.

---

_Comment by @thejchap on 2025-03-06 05:52_

@zanieb looks like this affects the `pip` commands as well

```
> cd example
> uv pip show example
Name: example
Version: 0.1.0
...
> cd ..
> uv pip show example --project example
warning: Package(s) not found for: example
```

appears venvs are [always discovered starting at the current directory](https://github.com/astral-sh/uv/blob/0fb52912393b78deb8c0612ecda6fd3a947df777/crates/uv-python/src/virtualenv.rs#L125)

idea to fix for both `uv pip` and `uv python` is refactor [find_python_installation](https://github.com/astral-sh/uv/blob/0fb52912393b78deb8c0612ecda6fd3a947df777/crates/uv-python/src/discovery.rs#L1032)/[PythonInstallation::find](https://github.com/astral-sh/uv/blob/0fb52912393b78deb8c0612ecda6fd3a947df777/crates/uv-python/src/installation.rs#L52) to accept a "root" or "directory" argument and pass down to [virtualenv_from_working_dir](https://github.com/astral-sh/uv/blob/0fb52912393b78deb8c0612ecda6fd3a947df777/crates/uv-python/src/virtualenv.rs#L124) (probably with a rename and/or adding a new function that takes a path argument)

lmk if this is on the right track!

---

_Comment by @zanieb on 2025-03-06 23:41_

Oh interesting. I'm... not sure if I'd expect `uv pip` to respect `--project` when determining the path to virtual environments. I'll need to ponder that. Regardless, I think you're right that we need to thread through a directory to that function.

---

_Comment by @zanieb on 2025-03-06 23:44_

(as an aside, I wonder if accepting the root directory there will let us drop the `current_dir` testing hack we have)

---

_Referenced in [astral-sh/uv#12049](../../astral-sh/uv/pulls/12049.md) on 2025-03-10 14:01_

---

_Comment by @thejchap on 2025-03-13 05:03_

@zanieb i think #12049 is ready for a look - looked a little bit into removing the `current_dir` hack for testing - when removing [this](https://github.com/thejchap/uv/blob/6490c0993e920dc3843386135905509c400ad738/crates/uv-python/src/lib.rs#L58), a few tests fail even with passing the `TestContext.workdir` through. didn't dig too deep but wondering if it has something to do with `PythonRequest::parse` also depending on [current_dir](https://github.com/thejchap/uv/blob/6490c0993e920dc3843386135905509c400ad738/crates/uv-python/src/discovery.rs#L1487) - decided to not mess with changing the api for that unless you think that's also worth doing as part of this pr

---
