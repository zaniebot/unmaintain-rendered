---
number: 11577
title: What is the equivalent of poetry install in uv?
type: issue
state: closed
author: karkinissan
labels:
  - question
assignees: []
created_at: 2025-02-17T13:14:33Z
updated_at: 2025-02-17T14:12:49Z
url: https://github.com/astral-sh/uv/issues/11577
synced_at: 2026-01-07T13:12:18-06:00
---

# What is the equivalent of poetry install in uv?

---

_Issue opened by @karkinissan on 2025-02-17 13:14_

### Question

I am attempting to migrate from Poetry to uv for one of my projects. A command I run in a Dockerfile is 
`RUN poetry config virtualenvs.create false --local && poetry install`
What is the equivalent for this in uv? 
From what I understand `uv sync` only installs the dependencies and doesn't run the `pip install .` command like `poetry install` does. 
What is the recommended solution here?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @karkinissan on 2025-02-17 13:14_

---

_Comment by @konstin on 2025-02-17 13:23_

Have you defined a `[build-system]` table? When that table is defined, `uv sync` installs the current project as editable (`pip install -e .`)

---

_Comment by @karkinissan on 2025-02-17 14:12_

That helped. 
I had to add the following block to the pyproject.toml
```
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Then run `uv sync` to recreate the lock file. I could then see that my project was added to the lock file. 

---

_Closed by @karkinissan on 2025-02-17 14:12_

---
