---
number: 2182
title: Where should dev-dependancies go in pyproject.toml file?
type: issue
state: closed
author: teaxio
labels:
  - question
assignees: []
created_at: 2024-03-04T22:05:43Z
updated_at: 2024-03-05T16:39:39Z
url: https://github.com/astral-sh/uv/issues/2182
synced_at: 2026-01-07T13:12:17-06:00
---

# Where should dev-dependancies go in pyproject.toml file?

---

_Issue opened by @teaxio on 2024-03-04 22:05_

In my pyproject.toml file I added:
```
[tool.uv.dev-dependencies]
ruff = "^0.0.79"
sphinx = "^4.0.0"
```
However, I am not sure if that is correct or not. Basically, how can I ensure that someone is able to install them with a command that gets run against the toml file? For instance I run `uv pip compile pyproject.toml -o requirements.txt` to generate the requirements.txt. Is there an equivalent that looks at `tool.uv.dev-dependencies` ? And where do I find general information on the processing of the toml file?


---

_Comment by @zanieb on 2024-03-04 22:20_

Hi!

The `pyproject.toml` specification is at [packaging.python.org](https://packaging.python.org/en/latest/specifications/pyproject-toml/) and it sounds like you're looking for the [`optional-dependencies` field](https://packaging.python.org/en/latest/specifications/pyproject-toml/#dependencies-optional-dependencies) which would look roughly like

```
[project]
name = "example"

[project.optional-dependencies]
dev = [
    "ruff>=0.0.79",
    "sphinx>=4.0.0"
]
```

and `uv pip install -r pyproject.toml --extra dev`

---

_Label `question` added by @zanieb on 2024-03-04 22:20_

---

_Comment by @teaxio on 2024-03-05 16:39_

Thank you so much, this is perfect. BTW, `uv` is amazing. I am blown every-time with how fast in created a venv.

---

_Closed by @teaxio on 2024-03-05 16:39_

---
