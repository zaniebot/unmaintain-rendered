```yaml
number: 4775
title: Use optimized versions of managed Python on Linux
type: pull_request
state: merged
author: Di-Is
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: use-opt-python-in-linux
created_at: 2024-07-03T14:41:42Z
updated_at: 2024-07-05T15:37:30Z
url: https://github.com/astral-sh/uv/pull/4775
synced_at: 2026-01-10T13:48:28Z
```

# Use optimized versions of managed Python on Linux

---

_Pull request opened by @Di-Is on 2024-07-03 14:41_

Fix #4774.

## Summary

Change the python interpreter for linux installed with `uv python` to an optimized one.

## Test Plan

I ran the following command on Linux (glibc) to confirm that an optimized (not debug built) Python is installed.

```bash
# install python
uv python install 3.12.3

# check build type
uv run python -c "import sysconfig;print(sysconfig.get_config_var('Py_DEBUG'))"
0
```

---

_Renamed from "Replace linux python downloaded by `uv python` command with optimized one" to "Replace python interpreter for linux downloaded by `uv python` command with optimized one" by @Di-Is on 2024-07-03 14:53_

---

_Comment by @zanieb on 2024-07-03 15:55_

:D of course I see this _after_ I've debugged it and found this is the problem. Glad we agree though! Thanks for opening a pull request.

---

_@zanieb approved on 2024-07-03 15:57_

---

_Renamed from "Replace python interpreter for linux downloaded by `uv python` command with optimized one" to "Use optimized versions of managed Python on Linux" by @zanieb on 2024-07-03 15:57_

---

_Label `bug` added by @zanieb on 2024-07-03 15:57_

---

_Label `preview` added by @zanieb on 2024-07-03 15:57_

---

_Merged by @zanieb on 2024-07-03 15:58_

---

_Closed by @zanieb on 2024-07-03 15:58_

---

_Branch deleted on 2024-07-03 21:41_

---

_Review comment by @helderco on `crates/uv-python/download-metadata.json`:22 on 2024-07-05 14:42_

The archive for `lto` is more than 3 times the size of `debug`.

Looking at the docs, there's an [Install Only Archive](https://gregoryszorc.com/docs/python-build-standalone/main/distributions.html#install-only-archive). Does uv need the _full_ variant?

```
cpython-3.12.3+20240415-aarch64-unknown-linux-gnu-debug-full.tar.zst	48MB
cpython-3.12.3+20240415-aarch64-unknown-linux-gnu-lto-full.tar.zst	178MB
cpython-3.12.3+20240415-aarch64-unknown-linux-gnu-install_only.tar.gz	24.4MB
```

---

_@helderco reviewed on 2024-07-05 14:42_

---

_@zanieb reviewed on 2024-07-05 15:37_

---

_Review comment by @zanieb on `crates/uv-python/download-metadata.json`:22 on 2024-07-05 15:37_

I'm not sure why Rye opted not to use these (and our download preferences are based on the implementation over there). Here's an issue for discussion https://github.com/astral-sh/uv/issues/4834

---
