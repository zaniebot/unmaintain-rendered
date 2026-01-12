```yaml
number: 4354
title: "Unable to install `mamba-ssm` with uv"
type: issue
state: closed
author: kszlim
labels:
  - question
assignees: []
created_at: 2024-06-17T02:42:12Z
updated_at: 2024-06-18T07:29:23Z
url: https://github.com/astral-sh/uv/issues/4354
synced_at: 2026-01-12T15:58:49Z
```

# Unable to install `mamba-ssm` with uv

---

_@kszlim_

```shell
# in an environment with all mamba-ssm requirements installed, cuda, torch, etc.
uv pip install mamba-ssm # will fail
```

uv version:
uv 0.2.11 on linux


Error:
```
uv pip install mamba-ssm
⠙ mamba-ssm==2.0.4                                                                                                                                                                                                                                            error: Failed to download and build `mamba-ssm==2.0.4`
  Caused by: Failed to build: `mamba-ssm==2.0.4`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/path/to/home/.cache/uv/environments-v0/.tmpnxysd8/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/path/to/home/.cache/uv/environments-v0/.tmpnxysd8/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/path/to/home/.cache/uv/environments-v0/.tmpnxysd8/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/path/to/home/.cache/uv/environments-v0/.tmpnxysd8/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'packaging'
```


---

_Renamed from "Unable to install mamba-ssm with uv" to "Unable to install `mamba-ssm` with uv" by @kszlim on 2024-06-17 02:42_

---

_Comment by @kidrahahjo on 2024-06-17 03:15_

I get a similar error when I try to install with pip directly, I suspect this is unrelated to uv. But I will let the maintainers suggest

![image](https://github.com/astral-sh/uv/assets/44747868/0b4ffb32-da25-4e4e-95eb-0110ca1d5985)

[Update]

When I manually install `packaging`, `pip` works but `uv` still can't resolve packaging


---

_Comment by @kszlim on 2024-06-17 04:06_

I've managed to get it installed with pip before, it does require that you pip install packaging first though.

---

_Comment by @konstin on 2024-06-17 09:01_

For packages like this one that doesn't declare its build dependencies, you need to install `packaging` manually and use `--no-build-isolation`.

---

_Label `question` added by @konstin on 2024-06-17 09:01_

---

_Comment by @kszlim on 2024-06-17 09:14_

@konstin I believe that `packaging` is declared as a build dep [here](https://github.com/state-spaces/mamba/blob/main/setup.py#L277)? But I could be misunderstanding.

Using uv as you've directed did manage to work for me, however, I'm trying to get it to work with `pixi`, which might need some work upstream. Thanks!

---

_Comment by @konstin on 2024-06-17 09:50_

The `install_requires` are declared at the bottom of the `setup.py`, but `packaging` is imported at the top, before we get to `install_requires`. The solution is to add a `pyproject.toml` with a `[build-system]` table so uv can install the dependencies of `setup.py` before running `setup.py`. You can find more information on build backends [here](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#declaring-the-build-backend), many more details about the `build-system` vs. `setup.py` and how `pyproject.toml` solves it are described in [PEP 517](https://peps.python.org/pep-0517/).

---

_Comment by @zanieb on 2024-06-17 10:42_

See #2252 — build isolation will become the default in pip in the future too.

---

_Closed by @charliermarsh on 2024-06-18 07:29_

---
