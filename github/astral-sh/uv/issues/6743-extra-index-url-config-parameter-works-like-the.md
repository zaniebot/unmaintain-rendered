---
number: 6743
title: "`extra-index-url` config parameter works like the `index-url`"
type: issue
state: closed
author: frague59
labels:
  - question
assignees: []
created_at: 2024-08-28T10:36:35Z
updated_at: 2024-08-29T10:09:09Z
url: https://github.com/astral-sh/uv/issues/6743
synced_at: 2026-01-07T13:12:17-06:00
---

# `extra-index-url` config parameter works like the `index-url`

---

_Issue opened by @frague59 on 2024-08-28 10:36_

# `pyproject.toml`

```sh
$ cat pyproject.toml
[tool.uv.pip]
extra-index-url = [
    "https://example.com/simple/"
]
```

# `uv venv --seed`

... an error about unable to find / download `pip`,`setuptool` and `wheel`

# Version
```sh
$ uv --version 
uv 0.3.5
```


---

_Comment by @charliermarsh on 2024-08-28 12:55_

What is the error you're seeing? Do you need `index-strategy = "unsafe-first-match"`?

---

_Label `question` added by @charliermarsh on 2024-08-28 12:55_

---

_Comment by @zanieb on 2024-08-28 13:33_

See https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes for more context.

---

_Comment by @frague59 on 2024-08-29 10:09_

Hi, 

I've read the doc, OK...

---

_Closed by @frague59 on 2024-08-29 10:09_

---
