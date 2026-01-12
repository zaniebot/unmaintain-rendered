```yaml
number: 7291
title: "Can't install `chumpy`"
type: issue
state: closed
author: YouJiacheng
labels:
  - question
assignees: []
created_at: 2024-09-11T14:05:38Z
updated_at: 2024-09-14T12:24:50Z
url: https://github.com/astral-sh/uv/issues/7291
synced_at: 2026-01-12T15:59:12Z
```

# Can't install `chumpy`

---

_@YouJiacheng_

uv version
```
uv 0.4.7 (a178051e8 2024-09-07)
```

Reproduce:
```
uv add chumpy
```

![image](https://github.com/user-attachments/assets/39974291-bb11-46f0-9d97-a27baefbeae8)

It's similar to #1551 but the solution don't work:
1. Use `uv init --lib` to create a project that allows `[build-system]`
2. Modify the `[build-system]` to
![image](https://github.com/user-attachments/assets/526a64ae-6560-45c1-967b-89803f551666)
3. Use `uv add chumpy`, and get an error
![image](https://github.com/user-attachments/assets/19d75c21-95e3-4e76-950f-9d0a48dcafa8)

`uv add pip`/`uv pip install pip` then `uv add chumpy`/`uv pip install chumpy` don't work.


`uv add pip`/`uv pip install pip` then `uv run pip install chumpy`/`uv run python -m pip install chumpy` don't work. (NOTE: they work when there is a cached wheel.)

![image](https://github.com/user-attachments/assets/fa0e709a-7f82-4810-859f-a1bd411486a6)


But pixi can handle this.
```
pixi add python pip
pixi run pip install chumpy
```
![image](https://github.com/user-attachments/assets/ce437983-71fc-481f-84a4-295fc8bdc973)



---

_Comment by @charliermarsh on 2024-09-11 15:39_

Sadly this is an issue with `chumpy` itself. The package is not properly structured -- it's documented as such here: https://github.com/astral-sh/uv/issues/2252#issuecomment-1981978204. For example, it also won't work if you run `pip install chumpy --use-pep517`, which will eventually be the default in pip:

```console
❯ pip install chumpy --use-pep517
Collecting chumpy
  Downloading chumpy-0.70.tar.gz (50 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [26 lines of output]
      Traceback (most recent call last):
        File "<string>", line 9, in <module>
      ModuleNotFoundError: No module named 'pip'

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.3/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.3/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
       ...
```

(In the pixi example you included, that's just pixi running `pip` within an environment, there's no special handling there that differs from `pip install`.)

You can either use `uv pip install chumpy --no-build-isolation`, or see the docs on using [build isolation](https://docs.astral.sh/uv/concepts/projects/#build-isolation) with `uv add` and related APIs.


---

_Label `question` added by @charliermarsh on 2024-09-11 15:40_

---

_Comment by @YouJiacheng on 2024-09-11 17:52_

Thanks! I should google "uv can't install chumpy" instead of the error message! Hope this pointer can help others who google the error message!

Here are verified solutions following [build isolation](https://docs.astral.sh/uv/concepts/projects/#build-isolation):
Solution 1:
```toml
[project]
name = "smpl-preprocess"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10,<3.11"
dependencies = [
    "numpy<1.24",
    "scipy<2",
    "chumpy",
]

[tool.uv]
no-build-isolation-package = ["chumpy"]

```
Run
```pwsh
uv venv
uv pip install pip setuptools
uv sync

```
![image](https://github.com/user-attachments/assets/e507a36d-f0a3-43e2-9b96-e820b9754a03)

Solution 2:
```toml
[project]
name = "smpl-preprocess"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10,<3.11"
dependencies = [
    "numpy<1.24",
    "scipy<2",
]

[project.optional-dependencies]
build = ["setuptools", "pip"]
run = ["chumpy"]

[tool.uv]
no-build-isolation-package = ["chumpy"]

```
It won't work if we follow [build isolation](https://docs.astral.sh/uv/concepts/projects/#build-isolation)
```pwsh
uv sync --extra build
uv sync --extra run

```
![image](https://github.com/user-attachments/assets/73d7cd5f-271b-4a30-98bc-7142137ea9c3)
But I found that it will work if we run `uv lock` first: (@charliermarsh do you know why?)
```
uv lock
uv sync --extra build
uv sync --extra run
```
![image](https://github.com/user-attachments/assets/d6fcd127-ddaf-4129-a6d6-f7a29f00b1e5)


---

_Closed by @YouJiacheng on 2024-09-11 17:52_

---

_Reopened by @YouJiacheng on 2024-09-11 17:52_

---

_Comment by @charliermarsh on 2024-09-12 01:31_

Does it work if you use `uv sync --extra run --extra build` for the second command, instead of `uv sync --extra run`?

---

_Comment by @YouJiacheng on 2024-09-13 01:38_

the screen shot showed it failed at the first command, i.e. `uv sync --extra build`.

---

_Comment by @charliermarsh on 2024-09-13 01:46_

It looks like `chumpy` doesn't declare any static metadata so you _have_ to build it even just to resolve the project dependencies. In that case, you can't use the strategy you described above (with separate optional groups). You have to pre-install the package with `uv pip install` unfortunately.

---

_Comment by @YouJiacheng on 2024-09-13 01:49_

but separate optional groups do work, if I run `uv lock` first, as shown in another screenshot.
that's why I asked you🧐.

---

_Comment by @charliermarsh on 2024-09-13 01:55_

My best guess is that the package is cached at some point, and so the build isn't recurring. If you run `uv cache clean` before each set of commands, do you see more consistent behavior? For example, I still see this locally:
```
❯ uv lock
⠙ chumpy==0.70                                                                                                            error: Failed to download and build `chumpy==0.70`
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'setuptools'
---
```

But if I run `uv venv && uv pip install pip setuptools && uv sync` first, the package is now cached, so running `uv lock` works.

---

_Comment by @YouJiacheng on 2024-09-13 02:03_

I suspected the cache too.
but I did run `uv cache clean` before(sorry I should include it in the screenshot), such that `uv lock` took 2.15s.

---

_Comment by @YouJiacheng on 2024-09-13 02:06_

I would try to reproduce it in a "fresher" environment…

---

_Comment by @charliermarsh on 2024-09-13 15:52_

Yeah I think to explore further I'd need a consistent reproduction (I'd suggest running `uv cache clean` and `rm -f uv.lock` before any sequence of commands).

---

_Comment by @YouJiacheng on 2024-09-13 19:39_

I have written a GitHub Action to create a consistent reproduction.
https://github.com/YouJiacheng/chumpy-uv-test/actions/runs/10855047631/job/30126795524
![image](https://github.com/user-attachments/assets/42af8e1f-c16c-47b8-bcc3-2389ea67e6d5)

Without `uv lock`:
https://github.com/YouJiacheng/chumpy-uv-test/actions/runs/10855149370/job/30127113769
![image](https://github.com/user-attachments/assets/317f908b-9af0-4a47-8bb9-d6afa888383b)
- - -
Local Tests:
Windows:
![image](https://github.com/user-attachments/assets/c3f5e01c-1667-4d46-afbe-3fb00a1659b8)
![image](https://github.com/user-attachments/assets/2a4f9022-7759-4740-bc2e-e6822b55aed4)
Ubuntu:
![image](https://github.com/user-attachments/assets/4ba74aa5-1ec8-4c5a-ab01-752c6611e017)


---

_Comment by @charliermarsh on 2024-09-13 19:48_

Thank you, I'll take a look.

---

_Comment by @charliermarsh on 2024-09-13 20:07_

Can you rerun the Actions with `--verbose` on every command?

---

_Comment by @YouJiacheng on 2024-09-13 21:02_

Done.
https://github.com/YouJiacheng/chumpy-uv-test/actions/runs/10856035081/job/30129895137
https://github.com/YouJiacheng/chumpy-uv-test/actions/runs/10856035837/job/30129897646

---

_Comment by @charliermarsh on 2024-09-13 22:13_

I think the difference is that `uv lock` runs in the system Python interpreter (since we're not doing any install operations), which happens to have `setuptools` installed in the environment (this isn't true of later versions of Python, like Python 3.12), whereas with `uv sync`, we run in the project virtual environment, which _doesn't_ include `setuptools` by default. If the project had a build-time dependency other than `setuptools` and `pip`, both versions would fail. So it kind of works by accident.

You can probably get the `uv lock` version to fail (I assume) by running `uv venv` first?

---

_Comment by @YouJiacheng on 2024-09-14 07:20_

Run `uv venv` first -- fail as expected:
https://github.com/YouJiacheng/chumpy-uv-test/actions/runs/10860593781/job/30141381752
Run `uv venv --python 3.12` first -- succeed (`uv lock` still runs in the system Python interpreter?):
https://github.com/YouJiacheng/chumpy-uv-test/actions/runs/10860520657/job/30141224028
Run `uv venv --python 3.12` first and use `uv lock --python-preference only-managed` instead of `uv lock` -- succeed:
https://github.com/YouJiacheng/chumpy-uv-test/actions/runs/10860542823/job/30141271654
Run `uv python install 3.10` first and then `uv lock --python-preference only-managed` -- succeed:
https://github.com/YouJiacheng/chumpy-uv-test/actions/runs/10860657431/job/30141522450

It kind of works by accident. But I think it is also kind of feature: `chumpy`'s `setup.py` doesn't directly depend on `setuptools`, instead, it uses `from distutils.core import setup`. Strictly speaking, some system Python interpreters are even not shipped with `pip`, so it won't work because its `setup.py` depends on `pip`.
`flash-attn`, on the other hand, doesn't have a `pyproject.toml` thus `setup.py` must be run to determine the metadata, and its `setup.py` directly depends on `packaging`, `setuptools` etc.

---
Specify `python==3.9.*` to avoid the system Python interpreter -- succeed.
The behavior is similar to `uv lock --python-preference only-managed`.
https://github.com/YouJiacheng/chumpy-uv-test/actions/runs/10861485699/job/30143436427

---

_Comment by @YouJiacheng on 2024-09-14 10:05_

A more consistent behavior is desirable.
Even managed Python Interpreters can have an environment inconsistent with the default virtual environment created by `uv venv`.
Let `uv lock` be "polluted" by the system Python Interpreter with an arbitrary environment sounds even worse.

---

_Comment by @YouJiacheng on 2024-09-14 10:26_

Moreover, maybe we can add an extra option to skip "Preparing metadata" with a user provided metadata.
IIUC, only lines like
```
Requires-Dist: scipy >=0.13.0
Requires-Dist: six >=1.11.0
```
in the `METADATA` is required by `uv lock`?

---

_Comment by @charliermarsh on 2024-09-14 12:24_

Closing in favor of the other issue.

---

_Closed by @charliermarsh on 2024-09-14 12:24_

---
