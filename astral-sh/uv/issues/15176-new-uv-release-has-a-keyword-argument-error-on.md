```yaml
number: 15176
title: new uv release has a keyword argument error  on python 3.9
type: issue
state: closed
author: gibsondan
labels:
  - bug
assignees: []
created_at: 2025-08-08T21:16:35Z
updated_at: 2025-08-08T23:45:15Z
url: https://github.com/astral-sh/uv/issues/15176
synced_at: 2026-01-12T16:02:05Z
```

# new uv release has a keyword argument error  on python 3.9

---

_@gibsondan_

### Summary

Hi uv team, working on an MRE, but maybe this stack trace is sufficient. python 3.10+ works, python 3.9 does not. This started happening today presumably due to the latest release:

```

[2025-08-08T21:04:57Z] #9 [5/7] RUN python -m uv pip install     boto3     -e ./dagster     -e ./dagster-pipes     -e ./dagster-shared     -e ./dagster-cloud     -e ./dagster-cloud-cli     py-spy
--
  | [2025-08-08T21:04:57Z] #9 0.341 Traceback (most recent call last):
  | [2025-08-08T21:04:57Z] #9 0.341   File "/usr/local/lib/python3.9/runpy.py", line 197, in _run_module_as_main
  | [2025-08-08T21:04:57Z] #9 0.341     return _run_code(code, main_globals, None,
  | [2025-08-08T21:04:57Z] #9 0.341   File "/usr/local/lib/python3.9/runpy.py", line 87, in _run_code
  | [2025-08-08T21:04:57Z] #9 0.341     exec(code, run_globals)
  | [2025-08-08T21:04:57Z] #9 0.341   File "/usr/local/lib/python3.9/site-packages/uv/__main__.py", line 52, in <module>
  | [2025-08-08T21:04:57Z] #9 0.341     _run()
  | [2025-08-08T21:04:57Z] #9 0.341   File "/usr/local/lib/python3.9/site-packages/uv/__main__.py", line 27, in _run
  | [2025-08-08T21:04:57Z] #9 0.341     uv = os.fsdecode(find_uv_bin())
  | [2025-08-08T21:04:57Z] #9 0.341   File "/usr/local/lib/python3.9/site-packages/uv/_find_uv.py", line 28, in find_uv_bin
  | [2025-08-08T21:04:57Z] #9 0.341     _matching_parents(_module_path(), "lib/python*/site-packages/uv"), "bin"
  | [2025-08-08T21:04:57Z] #9 0.341   File "/usr/local/lib/python3.9/site-packages/uv/_find_uv.py", line 79, in _matching_parents
  | [2025-08-08T21:04:57Z] #9 0.341     for part, match_part in zip(
  | [2025-08-08T21:04:57Z] #9 0.341 TypeError: zip() takes no keyword arguments

```

### Platform

python:3.9-slim docker image

### Version

0.8.7

### Python version

python 3.9

---

_Label `bug` added by @gibsondan on 2025-08-08 21:16_

---

_Comment by @gibsondan on 2025-08-08 21:17_

likely blamerev https://github.com/astral-sh/uv/commit/8968d783de58b0c475f65433f8d8668243811210

---

_Comment by @zanieb on 2025-08-08 23:36_

Thanks!

---

_Closed by @zanieb on 2025-08-08 23:45_

---
