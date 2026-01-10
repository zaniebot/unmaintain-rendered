---
number: 7259
title: "error: Expected a dependency at index 6"
type: issue
state: closed
author: kdheepak
labels:
  - bug
assignees: []
created_at: 2024-09-10T15:57:10Z
updated_at: 2024-09-10T17:44:00Z
url: https://github.com/astral-sh/uv/issues/7259
synced_at: 2026-01-10T01:24:12Z
---

# error: Expected a dependency at index 6

---

_Issue opened by @kdheepak on 2024-09-10 15:57_

I created a fresh project and ran the following:

```
$ history
 2233  which uv
 2234  uv self update
 2235  uv init --app
 2237  rm hello.py
 2238  vim .python-version
 2239  uv sync
 2241  uv add ipykernel --dev
 2242  ls
 2243  uv add "ibis-framework[duckdb]" pandas
 2244  uv add panel param
 2246  uv remove panel
 2247  uv add matplotlib panel param hvplot "ibis-framework[duckdb]" pandas # error occurs here
 2248  ls
 2249  rm uv.lock
 2251  uv add matplotlib panel param hvplot
 2252  uv add matplotlib panel param hvplot "ibis-framework[duckdb]" pandas
 2253  uv --version
```

And I was getting this error after removing `panel` as a dependency:

<img width="500" alt="image" src="https://github.com/user-attachments/assets/ab4a0ae9-e5a6-4169-aa06-fecab2e2c0bf">

In the place I've marked as `error occurs here` in the history above.

After deleting `uv.lock` and adding the dependencies one by one I don't get the error again. I thought I'd report in case it was apparent to someone as to what is going on here

```
uv --version
uv 0.4.8 (956cadd1a 2024-09-09)
```



---

_Comment by @kdheepak on 2024-09-10 15:59_

It looks like the first `uv add panel param` added a version of `panel` that is incorrect? I'm not sure where it got version 3.9.2 from:

<img width="1110" alt="image" src="https://github.com/user-attachments/assets/4b391464-f843-406d-89df-2829c604bcc8">


---

_Comment by @kdheepak on 2024-09-10 16:01_

Weirdly I'm not able to reproduce this again from scratch. Feel free to close if you think is a user error of some sort.

---

_Label `bug` added by @charliermarsh on 2024-09-10 17:13_

---

_Comment by @charliermarsh on 2024-09-10 17:14_

Definitely a bug! I reproduced once.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-10 17:14_

---

_Referenced in [astral-sh/uv#7262](../../astral-sh/uv/pulls/7262.md) on 2024-09-10 17:28_

---

_Closed by @charliermarsh on 2024-09-10 17:44_

---

_Closed by @charliermarsh on 2024-09-10 17:44_

---
