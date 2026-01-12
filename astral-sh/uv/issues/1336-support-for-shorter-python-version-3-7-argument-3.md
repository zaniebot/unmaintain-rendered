```yaml
number: 1336
title: "Support for shorter --python-version=3.7 argument: --3.7 or --python=3.7"
type: issue
state: closed
author: 2-5
labels:
  - enhancement
assignees: []
created_at: 2024-02-15T20:55:53Z
updated_at: 2024-06-26T01:07:37Z
url: https://github.com/astral-sh/uv/issues/1336
synced_at: 2026-01-12T15:58:26Z
```

# Support for shorter --python-version=3.7 argument: --3.7 or --python=3.7

---

_@2-5_

I often create venvs for multiple Python versions, `--python-version=3.7` is just too long, consider (additional) shorter options:

```shell
--3.7
--py=3.7
--python=3.7
```

---

_Label `enhancement` added by @MichaReiser on 2024-02-15 22:18_

---

_Comment by @zanieb on 2024-02-18 20:40_

I don't think we'll support `--3.7` as that's ambiguous, but we could add a shorter flag for the version.

---

_Comment by @dhruvmanila on 2024-02-19 01:28_

`--python` seems reasonable, it's also being used by [`pipenv`](https://github.com/pypa/pipenv#usage).

---

_Comment by @zanieb on 2024-02-19 02:26_

We'll need to be careful that the flag makes sense with #1396 

---

_Comment by @charliermarsh on 2024-06-26 01:07_

Most commands support `-p` which I think is fine.

---

_Closed by @charliermarsh on 2024-06-26 01:07_

---
