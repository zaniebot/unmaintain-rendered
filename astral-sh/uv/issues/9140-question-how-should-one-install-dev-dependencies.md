```yaml
number: 9140
title: "[Question] How should one install dev dependencies declared in pyproject.toml [dependency-groups]"
type: issue
state: closed
author: TonyYanOnFire
labels:
  - question
assignees: []
created_at: 2024-11-15T03:33:41Z
updated_at: 2024-11-15T21:31:56Z
url: https://github.com/astral-sh/uv/issues/9140
synced_at: 2026-01-12T15:59:43Z
```

# [Question] How should one install dev dependencies declared in pyproject.toml [dependency-groups]

---

_@TonyYanOnFire_

Hi.

I'm trying to introduce uv into my project. I did the following things to simulate the scenario of installing development dependencies when other members switch to using uv:
1. uv add --dev pycowsay
2. uv pip uninstall pycowsay
At this point, `pyproject.toml` has:
```
[dependency-groups]
dev = [
"pycowsay>=0.0.0.1",
]
```
But `pycowsay` is not installed in the environment, just like what other members may encounter.
At this time, if you run `uv pip install -r pyproject.toml`, `pycowsay` will not be installed.
Trying to add `--extra dev` results in an error:
```
$ uv pip install -r pyproject.toml --extra dev
error: Requested extra not found: dev
```

How can I achieve my goal?  Or am I trying to solve the wrong problem?

---

_Comment by @charliermarsh on 2024-11-15 21:31_

We don't support dependency groups in the `uv pip` interface right now. We've been waiting on pip itself so that we can align on a similar CLI. Dependency groups are only supported in `uv sync`, `uv lock`, etc.

In your case, you want `uv sync --dev`.

---

_Label `question` added by @charliermarsh on 2024-11-15 21:31_

---

_Closed by @charliermarsh on 2024-11-15 21:31_

---

_Comment by @charliermarsh on 2024-11-15 21:31_

See also: https://github.com/astral-sh/uv/issues/8590

---
