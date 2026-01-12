```yaml
number: 11014
title: Use already activated venv
type: issue
state: closed
author: khamaileon
labels:
  - question
assignees: []
created_at: 2025-01-28T08:26:53Z
updated_at: 2025-01-28T10:45:24Z
url: https://github.com/astral-sh/uv/issues/11014
synced_at: 2026-01-12T16:00:26Z
```

# Use already activated venv

---

_@khamaileon_

### Question

I am in the process of switching from virtualenvwrapper + piptools to uv. The only thing stopping me from fully switching is the ability to activate a virtual environment and run some hooks (like cd ~/workspace/myproject). I've seen that's already a [popular request](https://github.com/astral-sh/uv/issues/1910)  so I decided to keep virtualenvwrapper for now.

However, when I run uv sync, I get this warning:

```
`VIRTUAL_ENV=/Users/me/.virtualenvs/myproject` does not match the project environment path `.venv` and will be ignored
```

The script then creates a .venv folder and completes the sync instead of using the already activated venv.

This behavior is odd because the documentation states the opposite: https://docs.astral.sh/uv/pip/environments/#discovery-of-python-environments

Am I missing something?



### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @khamaileon on 2025-01-28 08:26_

---

_Comment by @konstin on 2025-01-28 09:54_

`uv sync` uses `.venv` in your project independent of your system of activated environment for consistency in the project commands. If you need to specifically use a different environment, you can set `UV_PROJECT_ENVIRONMENT` to your venv location.

---

_Comment by @khamaileon on 2025-01-28 10:45_

Thanks for your help. Here's the doc if someone else missed it: https://docs.astral.sh/uv/concepts/projects/config/#project-environment-path

---

_Closed by @khamaileon on 2025-01-28 10:45_

---
