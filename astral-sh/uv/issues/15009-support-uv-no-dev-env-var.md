```yaml
number: 15009
title: "Support `UV_NO_DEV` env var"
type: issue
state: closed
author: 2bndy5
labels:
  - enhancement
assignees: []
created_at: 2025-08-01T01:07:23Z
updated_at: 2025-08-08T14:33:45Z
url: https://github.com/astral-sh/uv/issues/15009
synced_at: 2026-01-10T03:32:46Z
```

# Support `UV_NO_DEV` env var

---

_Issue opened by @2bndy5 on 2025-08-01 01:07_

### Summary

It is tedious to keep typing `--no-dev` on a platform that is meant for production testing only. I think it would be more convenient to respect an env var `UV_NO_DEV` when present. Alternatively, I would appreciate support for `UV_DEFAULT_GROUPS`, but that seems way more complex since it is not a simple boolean value.

### Example

I want `pre-commit` installed when developing a project for embedded-Linux environments. But users that try the lib from an embedded-Linux env do not need any additional dependencies installed. Thus, the `--no-dev` option is the only way around that. I'm against removing `dev` from the default groups, because it may cause git to crash when the pre-commit binary executable is no longer in the venv for which `uv run pre-commit install` was invoked.

[env markers]: https://packaging.python.org/en/latest/specifications/dependency-specifiers/#environment-markers

I have considered adding [env markers] on all my dependencies in my dev group. Sadly, the available markers do not provide an adequate way of identifying an embedded-Linux env. For instance, the [`adafruit-blinka` package dynamically declares dependencies in it's setup.py for sdist](https://github.com/adafruit/Adafruit_Blinka/blob/main/setup.py) (which is not ideal and only really works because of the [piwheels index](https://www.piwheels.org/) added to default indexes in all RPi OS).

---

_Label `enhancement` added by @2bndy5 on 2025-08-01 01:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-01 02:10_

---

_Closed by @zanieb on 2025-08-08 14:33_

---
