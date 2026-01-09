---
number: 7204
title: Pylyzer not working with uv
type: issue
state: closed
author: daltunay
labels:
  - needs-mre
assignees: []
created_at: 2024-09-08T23:23:24Z
updated_at: 2024-10-11T09:54:36Z
url: https://github.com/astral-sh/uv/issues/7204
synced_at: 2026-01-07T13:12:17-06:00
---

# Pylyzer not working with uv

---

_Issue opened by @daltunay on 2024-09-08 23:23_

uv version `uv 0.4.7 (a178051e8 2024-09-07)`

---

I want to use [pylyzer](https://github.com/mtshiba/pylyzer) but it does not seem to work when using uv.

```
$ pip install pylyzer
$ pylyzer main.py
<PYLYZER OUTPUT>
```
-> WORKING

```
$ uv add pylyzer
$ uv run pylyzer main.py
Start checking: broker.py
<HANGS HERE>
```
-> NOT WORKING

```
$ uv pip install pylyzer
$ uv run pylyzer main.py
Start checking: broker.py
<HANGS HERE>
```
-> NOT WORKING

---

Thanks in advance for helping

---

_Comment by @smheidrich on 2024-09-09 10:58_

I can't reproduce this on my machine (uv 0.4.7, Debian 12, amd64):

If I just install Pylyzer by itself with either `pip` or `uv` and run it on a file, it prints errors like `[ERR] ERG_PATH not found`  (but still seems to check files fine?) until I also install `erg` manually using `cargo install erg`, which populates a `~/.erg` directory, after which these errors disappear. I never get the freezing issue you report.

Maybe it's worth reporting this on the pylyzer side as well because their devs probably know more about how their [setup script](https://github.com/mtshiba/pylyzer/blob/56e016e9159207519767d331e4a4f34eaa1c181b/setup.py) works and what the issue could be.

---

_Comment by @charliermarsh on 2024-09-16 02:53_

I also can't reproduce this:

```
❯ uv run pylyzer ../main.py
[ERR] ERG_PATH not found
[ERR] ERG_PATH not found
[ERR] ERG_PATH/lib/pystd not found
[ERR] ERG_PATH not found
[ERR] ERG_PATH/lib/core.d not found
Start checking: main.py
All checks OK: main.py
```

---

_Label `needs-mre` added by @charliermarsh on 2024-09-16 02:54_

---

_Closed by @charliermarsh on 2024-10-11 09:54_

---
