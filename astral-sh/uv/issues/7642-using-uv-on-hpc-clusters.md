---
number: 7642
title: Using uv on HPC Clusters
type: issue
state: open
author: PhilipVinc
labels: []
assignees: []
created_at: 2024-09-23T15:44:39Z
updated_at: 2025-11-07T23:55:31Z
url: https://github.com/astral-sh/uv/issues/7642
synced_at: 2026-01-10T01:24:17Z
---

# Using uv on HPC Clusters

---

_Issue opened by @PhilipVinc on 2024-09-23 15:44_

Hi,

As I really like uv, I've recently started testing it on the academic HPC clusters I regularly use, and would like to share some issues that I found, that makes working with uv harder.
I understand those might seem somewhat exotic, but I would still like to share them because I feel that handling to some extent those issues is necessary to use uv reliably in this setting.

## Issue 1: `venv` location

In case you're not familiar with those environments, a pecularity of HPC systems that caused problems is the following: 
 - HPC clusters generally have 2+ filesystems, `$WORK` and `$SCRATCH`, where WORK is subject to various forms of quota restrictions on the file size and # of inodes (files and directories) that can be stored, while `$SCRATCH` is a system where files untouched for 30 days are automatically deleted, much faster and without any restrictions on the number of files created. 
   - In general admins are happy to increase file size quotas on WORK, but not inode quota.
   - A python environment with jax already consumes ~10% of the inode quota per user on some HPC clusters. 

Normally, we work inside the `$WORK` directory, so `uv` will naturally create a `.venv` directory in there, eating up all my precious inode quota. Moreover, I normally define `XDG_CACHE_HOME=$SCRATCH/.cache` so uv complaints that he cannot simlink things to the vent because those are different filesystems.

To work around this issue, I declare manually a per-project `UV_PROJECT_ENVIRONMENT` inside of $SCRATCH, so that the environment will be created there. This is great, because it won't eat up my quota and even if it's deleted after 30 days, I don't care, because uv can naturally regenerate it if needed.
```python
# Get the current path and remove $work
current_path=$(pwd)
relative_path=${current_path#$DIR}
relative_path=${relative_path#/}
# Replace all slashes with underscores to create a single dirname
normalized_path=$(echo "$relative_path" | tr '/' '_')
export UV_PROJECT_ENVIRONMENT="${SCRATCH}/uv-venvs/${normalized_path}"
```

However I have to export this variable every time from the correct path. 
I would love it if it was possible to set a single global environment variable like `UV_USE_VENV_DEPOT=$SCRATCH/.cache/uv-venvs/` and uv would automatically use some logic like the one above to keep all `.venv` in there.

I understand that I can manually declare `UV_PROJECT_ENVIRONMENT` every time I change project, but that is error prone and goes against the idea that uv always makes sure I am running with the 'correct' virtual environment, automatically managed for me.

## Issue 2:  multiple architectures
Another peculiarity of HPC systems is that the user might use different modules. A common issue is that an user wants to use in two different settings two different versions of MPI, which can be 'loaded' by running `module load mpich/v1` or `module load mpich/v2`.

When installing some packages with binary dependencies, such as `mpi4py`, uv will aggressively cache the compiled wheel. However the wheel will differ depending on the version of mpi I have loaded, which `ùv` does not know about.

The simplest thing that would make it easier to work with is  if there was a way to specify in the `pyproject.toml` that the compiled wheels of a package should not be cached. 
A nicer (albeit more complex and I'm not sure if it's worth it) thing would be to provide some env variable or bash command to be checked when looking into the cache.


---

_Comment by @zanieb on 2024-09-23 16:03_

Related

- #1495 

---

_Comment by @zanieb on 2024-09-24 17:14_

Regarding the second point, you could set [reinstall-package](https://docs.astral.sh/uv/reference/settings/#pip_reinstall-package) in your `pyproject.toml` and we'll refresh the cached version of that package. I wonder if we need `--no-cache-package <name>` or `--no-cache-binary`

---

_Referenced in [astral-sh/uv#7357](../../astral-sh/uv/pulls/7357.md) on 2024-10-09 12:26_

---

_Referenced in [astral-sh/uv#11420](../../astral-sh/uv/issues/11420.md) on 2025-02-11 13:47_

---

_Referenced in [astral-sh/uv#14462](../../astral-sh/uv/issues/14462.md) on 2025-07-04 19:40_

---

_Comment by @marcelroed on 2025-11-07 23:55_

@PhilipVinc Did you ever find a good solution for this? I have a semi-solution for myself where I use Bash's prompt_function to figure out which project I'm in and set UV_PROJECT_ENVIRONMENT based on this, but I'm not sure how I can have this working well for all users. The distinction between slow project file storage that's global and fast local scratch storage is the key issue for us.

---
