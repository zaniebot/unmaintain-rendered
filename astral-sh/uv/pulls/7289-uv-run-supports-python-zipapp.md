```yaml
number: 7289
title: "`uv run` supports python zipapp"
type: pull_request
state: merged
author: j178
labels:
  - enhancement
assignees: []
merged: true
base: main
head: run-zipapp
created_at: 2024-09-11T13:30:26Z
updated_at: 2024-09-28T05:41:35Z
url: https://github.com/astral-sh/uv/pull/7289
synced_at: 2026-01-12T16:07:46Z
```

# `uv run` supports python zipapp

---

_@j178_

## Summary

`python` supports running a zipfile containing a `__main__.py` file, for example `python ./pre-commit-3.8.0.pyz`.

See https://docs.python.org/3/using/cmdline.html#interface-options:

> <script> Execute the Python code contained in script, which must be a filesystem path (absolute or relative) referring to either a Python file, a directory containing a __main__.py file, or a zipfile containing a __main__.py file.

and https://docs.python.org/3/library/zipapp.html.

Similar to #7281, this PR allows `uv run ./pre-commit-3.8.0.pyz` to work.

## Test Plan

```console
$ curl -O https://github.com/pre-commit/pre-commit/releases/download/v3.8.0/pre-commit-3.8.0.pyz
$ cargo run -- run ./pre-commit-3.8.0.pyz
```


---

_Converted to draft by @j178 on 2024-09-11 13:38_

---

_Comment by @zanieb on 2024-09-11 13:42_

Now we're getting kind of niche. Makes sense though, I think?

---

_Marked ready for review by @j178 on 2024-09-11 13:59_

---

_@zanieb approved on 2024-09-11 19:34_

---

_Merged by @zanieb on 2024-09-11 19:34_

---

_Closed by @zanieb on 2024-09-11 19:34_

---

_Label `enhancement` added by @zanieb on 2024-09-11 19:34_

---

_Branch deleted on 2024-09-28 05:41_

---
