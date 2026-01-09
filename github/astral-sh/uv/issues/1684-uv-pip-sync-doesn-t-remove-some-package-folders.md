---
number: 1684
title: "`uv pip sync` doesn't remove some package folders"
type: issue
state: closed
author: jfcherng
labels: []
assignees: []
created_at: 2024-02-19T10:24:49Z
updated_at: 2024-02-19T10:41:39Z
url: https://github.com/astral-sh/uv/issues/1684
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv pip sync` doesn't remove some package folders

---

_Issue opened by @jfcherng on 2024-02-19 10:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Description

`uv pip sync` doesn't remove some package folders. Maybe it's due to `-`/`_` in package name.

## Reproducer

### `more.txt`

```
pandas==2.2.0
pandas-stubs==2.2.0.240218
```

### `less.txt`

```
pandas==2.2.0
```

1. `$ uv v`
2. `$ uv pip sync more.txt`
3. `ls` packages

   ```
   $ ls -al .venv/lib/python3.12/site-packages/
   total 12
   drwxr-xr-x 1 jfcherng soc  230 Feb 19 18:20 .
   drwxr-xr-x 1 jfcherng soc   26 Feb 19 18:19 ..
   drwxr-xr-x 1 jfcherng soc  310 Feb 19 18:20 pandas
   drwxr-xr-x 1 jfcherng soc  120 Feb 19 18:20 pandas-2.2.0.dist-info
   drwxr-xr-x 1 jfcherng soc    0 Feb 19 18:20 pandas.libs
   drwxr-xr-x 1 jfcherng soc  228 Feb 19 18:20 pandas-stubs
   drwxr-xr-x 1 jfcherng soc   88 Feb 19 18:20 pandas_stubs-2.2.0.240218.dist-info
   -rw-r--r-- 1 jfcherng soc   18 Feb 19 18:19 _virtualenv.pth
   -rw-r--r-- 1 jfcherng soc 4375 Feb 19 18:19 _virtualenv.py
   ```

   Not sure whether it's important but notice that in package name `pandas-stubs` uses `-` and `pandas_stubs-2.2.0.240218.dist-info` uses `_`.

4. `$ uv pip sync less.txt`

   The package `pandas-stubs` should be removed.

5. `ls` packages

   ```
   $ ls -al .venv/lib/python3.12/site-packages/
   total 12
   drwxr-xr-x 1 jfcherng soc  206 Feb 19 18:22 .
   drwxr-xr-x 1 jfcherng soc   26 Feb 19 18:19 ..
   drwxr-xr-x 1 jfcherng soc  310 Feb 19 18:20 pandas
   drwxr-xr-x 1 jfcherng soc  120 Feb 19 18:20 pandas-2.2.0.dist-info
   drwxr-xr-x 1 jfcherng soc    0 Feb 19 18:20 pandas.libs
   drwxr-xr-x 1 jfcherng soc    0 Feb 19 18:22 pandas_stubs-2.2.0.240218.dist-info      <--- still there
   -rw-r--r-- 1 jfcherng soc   18 Feb 19 18:19 _virtualenv.pth
   -rw-r--r-- 1 jfcherng soc 4375 Feb 19 18:19 _virtualenv.py
   ```

   `pandas_stubs-2.2.0.240218.dist-info` is still there. P.s. it's an empty folder.

6. Now, `$ uv pip sync more.txt` does nothing because `uv` seems to consider `pandas-stubs` has been installed.
7. Now, `$ uv pip sync less.txt` will fail because `.venv/lib/python3.12/site-packages/pandas_stubs-2.2.0.240218.dist-info/RECORD` is not found.

## Env

- `uv --version`: uv 0.1.5
- uv platform: Linux x86_64

---

_Comment by @jfcherng on 2024-02-19 10:37_

Oops... It turns out that this is because I am using Network File System. I can't reproduce this on a local disk. Not sure whether there is an easy fix in `uv` side.

---

_Closed by @jfcherng on 2024-02-19 10:37_

---
