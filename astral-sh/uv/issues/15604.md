```yaml
number: 15604
title: "`uv pip list` shows system python packages instead of `.venv` python packages"
type: issue
state: closed
author: taoari
labels:
  - bug
assignees: []
created_at: 2025-08-31T06:17:17Z
updated_at: 2025-09-17T11:04:30Z
url: https://github.com/astral-sh/uv/issues/15604
synced_at: 2026-01-10T03:23:54Z
```

# `uv pip list` shows system python packages instead of `.venv` python packages

---

_Issue opened by @taoari on 2025-08-31 06:17_

### Summary

`uv pip list` shows the wrong (system python) environment:

```
> uv pip list
Using Python 3.11.5 environment at: D:\scoop\apps\anaconda3\current\App
Package                       Version
----------------------------- ---------------
aiobotocore                   2.5.0
```

The following one should be the correct one:

```
> .venv\Scripts\python -m pip list
Package                      Version   Editable project location
---------------------------- --------- ------------------------------------------------------
annotated-types              0.7.0
```

### Platform

windows 

### Version

uv 0.8.14 (af856fb88 2025-08-28)

### Python version

Python 3.12.11

---

_Label `bug` added by @taoari on 2025-08-31 06:17_

---

_Comment by @charliermarsh on 2025-08-31 14:31_

It looks like you have an active Anaconda environment? What do the `CONDA_PREFIX` and `VIRTUAL_ENV` vars show?

---

_Comment by @taoari on 2025-08-31 20:36_

@charliermarsh Here are the environment variables. I do have the base Anaconda environment activated by default, but shouldn't uv pip list prioritize the .venv folder over the base environment?

```
> $Env:CONDA_PREFIX
D:\scoop\apps\anaconda3\current\App

> $Env:VIRTUAL_ENV
```

---

_Comment by @gigberg on 2025-09-01 09:10_

the same issue in the same time, uv pip's logic is a little wired. releated to https://github.com/astral-sh/uv/issues/7124

see also:
#

> An activated virtual environment based on the VIRTUAL_ENV environment variable.
An activated Conda environment based on the CONDA_PREFIX environment variable.
A virtual environment at .venv in the current directory, or in the nearest parent directory.
If no virtual environment is found, uv will prompt the user to create one in the current directory via uv venv.
If the --system flag is included, uv will skip virtual environments search for an installed Python version. 

https://docs.astral.sh/uv/pip/environments/#discovery-of-python-environments

However, you can use `uv pip install --python .venv/bin/python` to let pip use your venv.

---

_Comment by @zanieb on 2025-09-02 20:31_

@taoari what is `CONDA_DEFAULT_ENV` set to?

We do special-case base Conda environments, but we don't recognize the name "App" today. https://github.com/astral-sh/uv/blob/3f83390e342dcb6fccc449335c15d1255885ad20/crates/uv-python/src/virtualenv.rs#L79-L106

Why is your base environment called "App"?

---

_Comment by @taoari on 2025-09-02 21:01_

@zanieb $Env:CONDA_DEFAULT_ENV
base

Anaconda `base` environemnt is located at `anaconda3\current\App` by default I think.

---

_Comment by @zanieb on 2025-09-02 21:21_

Really? I thought the path usually matched the `CONDA_DEFAULT_ENV` name? What other directories are in that folder?

---

_Comment by @taoari on 2025-09-02 21:57_

@zanieb Conda environments you create yourself usually match the directory name, like `envs/<name>`. The base environment is a special case.

```
> conda env list
# conda environments:
#
base                  *  D:\scoop\apps\anaconda3\current\App
python312                D:\scoop\apps\anaconda3\current\App\envs\python312
```

This is a bit off-topic — the actual issue is that uv pip seems to get the priority order wrong.

---

_Comment by @zanieb on 2025-09-02 22:03_

It's not really off topic, because we use a different order if we can detect it's the base environment

---

_Comment by @zanieb on 2025-09-02 22:07_

e.g., see https://github.com/astral-sh/uv/pull/7691

and prior discussions in

- https://github.com/astral-sh/uv/issues/8940
- https://github.com/astral-sh/uv/issues/7124
- https://github.com/astral-sh/uv/issues/7137

---

_Comment by @zanieb on 2025-09-02 22:26_

It seems like there is a mistake in the heuristic we're using.

---

_Comment by @gigberg on 2025-09-04 12:04_

> It seems like there is a mistake in the heuristic we're using.

Not really, the  path format `\"$dir\\App\\envs\"`  is caused by `scoop`, a windows package manager. I guess the op install anaconda by command `scoop install extras/anaconda3`. see as 
```powershell
"Start-Process \"$dir\\$fname\" -ArgumentList @('/S', '/InstallationType=JustMe', '/RegisterPython=1', '/AddToPath=0', '/NoRegistry=1', \"/D=$dir\\App\") -Wait
```
https://github.com/ScoopInstaller/Extras/blob/master/bucket/anaconda3.json#L23-L24

---

_Comment by @gigberg on 2025-09-04 12:54_

Yes, this seems to be wrong, my ubuntu miniconda base path  is `$CONDA_PREFIX=~/miniconda` not `~/miniconda/base` or `$CONDA_PREFIX=~/miniconda3/envs/labelyolo` like special common envs.

https://github.com/astral-sh/uv/blob/3f83390e342dcb6fccc449335c15d1255885ad20/crates/uv-python/src/virtualenv.rs#L96-L98

Ubuntu 22.04, miniconda3:
env_name `base`: 
$CONDA_PREFIX=~/miniconda
$echoCONDA_DEFAULT_ENV=base

env_name `xxx`
$CONDA_PREFIX=~/miniconda/envs/xxx
$echoCONDA_DEFAULT_ENV=xxx

So, maybe use
```rust
fn from_prefix_path(path: &Path) -> CondaEnvironmentKind {
    // Check whether the path contains a directory named "envs"
    if path.components().any(|comp| comp.as_os_str() == "envs") {
        // If "envs" is part of the path, it's a child environment
        CondaEnvironmentKind::Child
    } else {
        // Otherwise, it's the base environment
        CondaEnvironmentKind::Base
    }
}
```



---

_Closed by @zanieb on 2025-09-17 11:04_

---
