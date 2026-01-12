```yaml
number: 9326
title: "`~` Tildes in environment variables not expanded before use"
type: issue
state: closed
author: fynnsu
labels: []
assignees: []
created_at: 2024-11-21T15:58:13Z
updated_at: 2024-11-21T17:32:17Z
url: https://github.com/astral-sh/uv/issues/9326
synced_at: 2026-01-12T15:59:47Z
```

# `~` Tildes in environment variables not expanded before use

---

_@fynnsu_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```bash
export UV_CACHE_DIR="~/uv_cache"
# Use uv
uv venv
uv pip install numpy
```
Will produce a subfolder `'~'` with `'uv_cache'` in it instead of expanding to `/home/user/uv_cache` when used. 

Note: Since shells will often expand the tildes themselves this may not often be a problem. In the above example, using `export UV_CACHE_DIR=~/uv_cache` (without the ""), works as my shell expands the `~` before setting the env variable. 

I ran into this issue because I was trying to use `astral-sh/setup-uv` with `cache-local-path: ~/.uv_cache` which then is used to set `UV_CACHE_DIR`. Unfortunately here, whether I include the "" or not, the env variable gets set with the `~` giving me the wrong cache dir.

I think it would be useful to support `~` at the start of paths for `UV_CACHE_DIR` and other similar path environment variables (untested but I'm guessing they have the same limitation). 



---

_Comment by @fynnsu on 2024-11-21 16:31_

This is potentially related to #9327. 
Doing something similar with env variables
```bash
export UV_CACHE_DIR=/home/user/path/../path/cache
uv venv
```
results in
```
error: failed to create directory `/home/user/path/../path/cache`
  Caused by: Operation not supported (os error 45)
```
Ideally there would be path resolution on all paths provided by the user (env variables, configuration files, cli args)

---

_Comment by @zanieb on 2024-11-21 16:58_

I don't think I agree that we should expand this `~` in inputs, I don't think that's standard for most tools? As a simple example,`gh` clones into a `~` directory in the current folder:

```
$ gh repo clone zanieb/uv "~/test"
$ tree . -L 2
.
└── ~
    └── test
```

---

_Comment by @zanieb on 2024-11-21 16:58_

I do think `astral-sh/setup-uv` should expand the `~` though.

---

_Comment by @charliermarsh on 2024-11-21 16:58_

Yeah I don't think Cargo does that either. Maybe the GitHub Action should be expanding it though.

---

_Comment by @fynnsu on 2024-11-21 17:32_

Okay I will open an issue in `astral-sh` for this. Thank you

---

_Closed by @fynnsu on 2024-11-21 17:32_

---
