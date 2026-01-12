```yaml
number: 7142
title: "\"uv sync -U\" update also pyproject.toml package version"
type: issue
state: closed
author: muk-as
labels:
  - question
assignees: []
created_at: 2024-09-06T22:12:30Z
updated_at: 2024-10-21T21:43:28Z
url: https://github.com/astral-sh/uv/issues/7142
synced_at: 2026-01-12T15:59:10Z
```

# "uv sync -U" update also pyproject.toml package version

---

_@muk-as_

in pyproject.toml was fastapi>=0.113
uv sync -U will update fastapi to 0.114 in .venv, uv.lock
but in pyproject.toml still fastapi>=0.113
all ok, but i think
will be better change on fastapi>=0.114

---

_Comment by @zanieb on 2024-09-07 15:32_

This is the intended behavior — upgrading changes the locked version within the constraints you provide in your pyproject.toml: https://docs.astral.sh/uv/concepts/projects/#upgrading-locked-package-versions

It'd be weird for us to change the specification of the dependency itself on sync — it'd be best implemented as some other dedicated command. I think there are some other issues tracking support for that but having a hard time tracking them down right now.

---

_Label `question` added by @zanieb on 2024-09-07 15:32_

---

_Closed by @zanieb on 2024-10-21 21:43_

---
