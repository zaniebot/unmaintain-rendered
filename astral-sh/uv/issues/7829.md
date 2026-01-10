```yaml
number: 7829
title: CONDA_PREFIX not leading to conda env being used
type: issue
state: open
author: mbspng
labels:
  - question
assignees: []
created_at: 2024-10-01T07:43:51Z
updated_at: 2024-10-01T14:06:45Z
url: https://github.com/astral-sh/uv/issues/7829
synced_at: 2026-01-10T04:45:10Z
```

# CONDA_PREFIX not leading to conda env being used

---

_Issue opened by @mbspng on 2024-10-01 07:43_

The `CONDA_PREFIX` is not leading to my conda env (called `ssa`) being used with uv.

I have an activated conda env:

```
-> % echo $CONDA_PREFIX
/home/<user>/miniconda3/envs/ssa
```

I run `uv sync` and do not have a `.venv` in that directory. I am expecting it to use the conda env. It does not.

```
-> % l -a .venv/bin/python
lrwxrwxrwx 1 mbs 80 okt.   1 09:36 .venv/bin/python -> /home/<user>/.local/share/uv/python/cpython-3.10.13-linux-x86_64-gnu/bin/python3.10*
```

Whereas

```
-> % which python
/home/<user>/miniconda3/envs/ssa/bin/python

-> % l -a ~/miniconda3/envs/ssa/bin/python    
lrwxrwxrwx 1 mbs 10 okt.   1 09:16 /home/<user>/miniconda3/envs/ssa/bin/python -> python3.10*
```


Docs (https://docs.astral.sh/uv/pip/environments/#using-arbitrary-python-environments) say:

> When running a command that mutates an environment such as uv pip sync or uv pip install, uv will search for a virtual environment in the following order:
>
> - An activated virtual environment based on the VIRTUAL_ENV environment variable.
> - An activated Conda environment based on the CONDA_PREFIX environment variable.
> - A virtual environment at .venv in the current directory, or in the nearest parent directory.
>
>  If no virtual environment is found, uv will prompt the user to create one in the current directory via uv venv.

Looks like incorrect behavior to me, judging by the docs. How can I make `uv` use the conda environment?


```
uv version = 0.4.13

Linux <name> 6.8.0-45-generic #45~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Wed Sep 11 15:25:05 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
```

I tried with export `VIRTUAL_ENV`. Then I am getting

```
-> % uv sync
warning: `VIRTUAL_ENV=/home/<user>/miniconda3/envs/ssa/` does not match the project environment path `.venv` and will be ignored
```

When I set the project env var (https://docs.astral.sh/uv/concepts/projects/#configuring-the-project-environment-path), I get this:


```
-> % uv sync
error: Project virtual environment directory `/home/<user>/miniconda3/envs/ssa/` cannot be used because it is not a virtual environment and is non-empty
```

So this is also not the way to make it use the conda env.

When I do this:

```
-> % uv sync --python="$(which python)"
warning: `VIRTUAL_ENV=/home/<user>/miniconda3/envs/ssa/` does not match the project environment path `.venv` and will be ignored
Resolved 176 packages in 3ms
Audited 154 packages in 0.06ms
```

then it at least uses the right interpreter:
```
-> % l -a .venv/bin/python
lrwxrwxrwx 1 mbs 44 okt.   1 10:51 .venv/bin/python -> /home/<user>/miniconda3/envs/ssa/bin/python3.10*
```

But the dependencies still end up in `.venv`. I want them to be installed into the conda env, because that is how I manage environments for my IDE.



---

_Comment by @zanieb on 2024-10-01 12:43_

`uv sync` intentionally does not respect active environments. See https://docs.astral.sh/uv/concepts/projects/#project-environments for details.

---

_Label `question` added by @zanieb on 2024-10-01 12:43_

---

_Comment by @mbspng on 2024-10-01 13:42_

> `uv sync` intentionally does not respect active environments. See https://docs.astral.sh/uv/concepts/projects/#project-environments for details.

Okay, thank you. I think the docs should be updated at the place I linked to, because there it sounds like `uv sync` would do that, since it says 

> When running a command that mutates an environment such as uv pip sync or uv pip install, uv will search for a virtual environment in the following order [...]

How is the user supposed to know what "such as" means, and that it includes `uv pip sync` but not `uv sync`. And also after reading the section you linked to, I do not understand it. Seems contradictory to the other section to me.

---

_Comment by @zanieb on 2024-10-01 13:57_

The documentation you linked is in the "pip interface" section, it's all about `uv pip` not the commands outside of it. `uv sync` and `uv pip sync` work very differently.

---

_Comment by @mbspng on 2024-10-01 13:59_

Okay, I see, thanks. That pip-not-pip thing is also really confusing, btw.

---

_Comment by @zanieb on 2024-10-01 14:03_

Yeah sorry it's the downside of supporting both legacy interfaces and new workflows in a single tool. I'll see if I can improve the documentation on that page to make it clear we're just talking about `uv pip`.

---

_Assigned to @zanieb by @zanieb on 2024-10-01 14:03_

---

_Comment by @mbspng on 2024-10-01 14:06_

I understand. Thanks! Btw. I think it is good that it now supports the poetry / pdm style interface with lock files etc. I evaluated uv a while back before it supported that and decided not to use it at that point, because of the awkward old pip-type-of workflow. Now with the new interface, I re-evaluated.

---
