```yaml
number: 9067
title: "`ENV UV_SYSTEM_PYTHON=1` still creates `.venv` and dependencies aren't able to be found"
type: issue
state: closed
author: theelderbeever
labels:
  - question
assignees: []
created_at: 2024-11-12T18:06:25Z
updated_at: 2025-11-06T15:12:05Z
url: https://github.com/astral-sh/uv/issues/9067
synced_at: 2026-01-10T03:23:53Z
```

# `ENV UV_SYSTEM_PYTHON=1` still creates `.venv` and dependencies aren't able to be found

---

_Issue opened by @theelderbeever on 2024-11-12 18:06_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I am building inside of a docker container and am setting `ENV UV_SYSTEM_PYTHON=1` prior to ever running `uv sync`. However, despite setting this per the documentation a `.venv` directory is still created and packages are installed there. This result in none of my python dependencies being found by the environment. How can I force `uv` to not create a `venv`? 

```
uv 0.5.1
ubuntu 18.04 devcontainer
```


---

_Comment by @charliermarsh on 2024-11-12 18:10_

`UV_SYSTEM_PYTHON` doesn't affect the project interface (`uv sync`, `uv lock`, `uv add`, etc.).

See: https://docs.astral.sh/uv/concepts/projects/#configuring-the-project-environment-path.

---

_Label `question` added by @charliermarsh on 2024-11-12 18:10_

---

_Comment by @theelderbeever on 2024-11-12 18:24_

> `uv sync` will remove extraneous packages from the environment by default and, as such, may leave the system in a broken state.

Yikes.... So when using `ENV UV_SYSTEM_PYTHON=1` is the "correct"/"only" way to install to the system packages without removing whats there to use the following? I know its bad practice to not use a venv but this project has some strange limitations unfortunately.

```
COPY pyproject.toml .
RUN uv pip install -r pyproject.toml
COPY . .
RUN uv pip install -e .
```

---

_Comment by @zanieb on 2024-11-12 18:57_

I would use `uv export` and `uv pip install` so you get the locked versions.

---

_Comment by @theelderbeever on 2024-11-12 19:08_

Thank you @zanieb 

---

_Closed by @theelderbeever on 2024-11-12 19:08_

---

_Comment by @thclark on 2025-11-06 15:12_

To use the system python environment in a `devcontainer`, I use:
```
# Use system python and dependencies
ENV UV_PROJECT_ENVIRONMENT="/usr/local/"
```


---
