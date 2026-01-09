---
number: 8093
title: Latest versions do no catch errors
type: issue
state: closed
author: mskrip
labels: []
assignees: []
created_at: 2023-10-20T15:24:37Z
updated_at: 2023-10-20T15:29:47Z
url: https://github.com/astral-sh/ruff/issues/8093
synced_at: 2026-01-07T13:12:15-06:00
---

# Latest versions do no catch errors

---

_Issue opened by @mskrip on 2023-10-20 15:24_

I tested this only on the line-length violation, but on multiple `ruff` versions (see below)

## Minimal code snippet:

`sample.file.py`:
```python
"""
This is a sample file with line that's just a bit too long and ruff should flag this as an error. Hopefully it does.
"""
```

Running `$ ruff check --isolated .` does not result in an error since version 0.1.0. This flags an error on 0.0.292.

```sh
$ ruff --version                                                                                                                                                                                                                         
ruff 0.0.292
$ ruff check --isolated .                                                                                                                                                                                                                
sample_file.py:2:89: E501 Line too long (116 > 88 characters)
Found 1 error.
$ echo $?
1
...
$ ruff --version
ruff 0.1.0
$ ruff check --isolated .
$ echo $?
0
...
$ ruff --version
ruff 0.1.1
$ ruff check --isolated .
$ echo $?
0
```


---

_Comment by @charliermarsh on 2023-10-20 15:26_

Thanks for the clear issue. `line-too-long`  (`E501`) was removed from the default rule set in [v0.1.0](https://github.com/astral-sh/ruff/blob/main/CHANGELOG.md#breaking-changes), so I believe this is expected -- but you can re-add it with `--extend-select E` or by setting `extend-select = ["E"]` in your `pyproject.toml` or `ruff.toml` file.

---

_Comment by @mskrip on 2023-10-20 15:28_

Oh, I completely missed that line in the changelog :facepalm: , thank you very much for the fast response. That solves it then.

---

_Closed by @mskrip on 2023-10-20 15:28_

---

_Comment by @charliermarsh on 2023-10-20 15:29_

No worries! Sorry for the churn.

---
