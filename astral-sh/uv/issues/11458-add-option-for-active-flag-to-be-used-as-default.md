```yaml
number: 11458
title: "Add option for `--active` flag to be used as default"
type: issue
state: closed
author: Nick-Hemenway
labels:
  - enhancement
assignees: []
created_at: 2025-02-12T18:40:47Z
updated_at: 2025-02-12T18:42:45Z
url: https://github.com/astral-sh/uv/issues/11458
synced_at: 2026-01-12T16:00:37Z
```

# Add option for `--active` flag to be used as default

---

_@Nick-Hemenway_

### Summary

There seems to be a lot of interest in enabling `uv` to reference virtual environments located in a central location on your computer (as opposed to in a `.venv` folder located in the project's directory) (See #1495). It doesn't appear that there's an immediate plan to support this functionality (at least not through an interface that resembles those of virtualenvwrapper, pyenv, or poetry). 

There's a bit of workaround, however. A user can use an external virtual environment by creating it, activating it, and then adding a `--active` flag to each invocation of `uv`. It might look something like this:

```
>>> uv venv ~/.virtualenvs/my_venv
>>> source ~/.virtualenvs/my_venv/Scripts/activate
>>> uv add --active numpy pandas jupyterlab
>>> uv remove --active pandas
>>> uv sync --active
```

This workflow is fine, but it's kind of tedious remembering to add the `--active` flag all the time. Especially because if you forget to add the flag, `uv` will automatically create a new `.venv` folder in your project directory that you have to delete. 

My request is to please add the option (perhaps through an environment variable?) to make the `--active` flag the default behavior. If enabled as the default, it would make sense to raise an error message if a user tries to call `uv add` (or similar applicable commands) without a virtual environment currently active. 

### Example

_No response_

---

_Label `enhancement` added by @Nick-Hemenway on 2025-02-12 18:40_

---

_Comment by @zanieb on 2025-02-12 18:42_

Duplicate of https://github.com/astral-sh/uv/issues/11273

---

_Closed by @zanieb on 2025-02-12 18:42_

---
