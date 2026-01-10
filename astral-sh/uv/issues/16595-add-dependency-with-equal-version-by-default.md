---
number: 16595
title: "Add dependency with \"equal\" version by default?"
type: issue
state: closed
author: mquan86
labels:
  - question
assignees: []
created_at: 2025-11-04T18:43:01Z
updated_at: 2025-11-04T19:00:45Z
url: https://github.com/astral-sh/uv/issues/16595
synced_at: 2026-01-10T01:26:07Z
---

# Add dependency with "equal" version by default?

---

_Issue opened by @mquan86 on 2025-11-04 18:43_

### Question

Right now if you add a dependency from UV, it will add with ">=" syntax of latest version.

e.g. if I type: `uv add matplotlib`
then in pyproject.toml, I will have: `matplotlib>=3.10.7`

I want by default if I type: `uv add matplotlib`
then it get latest version and add to pyproject.toml as: `matplotlib==3.10.7`

Is there anyway to achieve it by default?

Keep in mind that:
- I don't want to type `uv add matplotlib==3.10.7` (as it doesn't make sense that I need to check what is the current version number)
- I don't want to use uv.lock as pyproject.toml alonne should be able enough to lock version.

I think my question similar to this suggestion/idea: https://github.com/astral-sh/uv/issues/6781#issuecomment-3381019557

### Platform

Windows 11

### Version

uv 0.8.13 (ede75fe62 2025-08-21)

---

_Label `question` added by @mquan86 on 2025-11-04 18:43_

---

_Comment by @charliermarsh on 2025-11-04 19:00_

You can do `--bounds=exact` or:
```toml
[tool.uv]
add-bounds = "exact"
```

I generally recommend against using this since it's problematic for libraries and redundant with `uv.lock` for applications, but it does what you describe.

---

_Closed by @charliermarsh on 2025-11-04 19:00_

---
