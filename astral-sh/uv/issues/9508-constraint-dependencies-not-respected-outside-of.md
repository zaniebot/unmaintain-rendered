```yaml
number: 9508
title: "`constraint-dependencies` not respected outside of pyproject.toml"
type: issue
state: open
author: cdb39
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-11-28T17:13:29Z
updated_at: 2024-11-29T16:39:24Z
url: https://github.com/astral-sh/uv/issues/9508
synced_at: 2026-01-12T15:59:52Z
```

# `constraint-dependencies` not respected outside of pyproject.toml

---

_@cdb39_

`constraint-dependencies` specified in either a project uv.toml, or a user-level uv.toml are ignored, regardless of anything present in the pyproject.toml.  I'm using uv version 0.5.5.

The documentation on constraint dependencies is a bit light but I feel like this is surprising behaviour.

If it is expected, are there any plans to support `constraint-dependencies` specified in either the user or system-level uv configuration?

**Simple example of them working in pyproject.toml**

pyproject.toml
```toml
[project]
name = "uv-issue"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = []

[tool.uv]
constraint-dependencies = [
    "flask<3"
]
```

Running `uv add flask` does not install a version >=3
```shell
Creating virtual environment at: .venv
Resolved 9 packages in 134ms
Prepared 7 packages in 75ms
Installed 7 packages in 2ms
 + blinker==1.9.0
 + click==8.1.7
 + flask==2.3.3
 + itsdangerous==2.2.0
 + jinja2==3.1.4
 + markupsafe==3.0.2
 + werkzeug==3.1.3
```

**Simple example of them not working in uv.toml**

pyproject.toml
```toml
[project]
name = "uv-issue"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = []
```

uv.toml
```
constraint-dependencies = [
    "flask<3"
]
```

Running `uv add flask` installs version 3.1.0
```shell
Creating virtual environment at: .venv
Resolved 9 packages in 84ms
Prepared 1 package in 32ms
Installed 7 packages in 3ms
 + blinker==1.9.0
 + click==8.1.7
 + flask==3.1.0
 + itsdangerous==2.2.0
 + jinja2==3.1.4
 + markupsafe==3.0.2
 + werkzeug==3.1.3
```

---

_Comment by @charliermarsh on 2024-11-28 17:31_

Yeah these definitely aren't respected. I honestly thought that we errored here but looks like that's just for `dev-dependencies` and others.

---

_Comment by @cdb39 on 2024-11-28 18:52_

Ah ok.  Thanks for the fast response.  Is there a plan to support them in the future?

My use-case is essentially the use-case outlined in the [pypi guide on constraints file](https://pip.pypa.io/en/stable/user_guide/#constraints-files), namely:
> For instance, say that the “helloworld” package doesn’t work in your environment, so you have a local patched version. Some things you install depend on “helloworld”, and some don’t.
>
> ...
>
> Constraints files offer a better way: write a single constraints file for your organisation and use that everywhere. If the thing being installed requires “helloworld” to be installed, your fixed version specified in your constraints file will be used.

The list of constraints is quite long, and can change, so ideally we won't need to individually update every project.

I can see that there was a request for support for a similar workflow (albeit a different implementation) in https://github.com/astral-sh/uv/issues/6518.

I actually think this is also a potential solution to https://github.com/astral-sh/uv/issues/5260, if an organisation is blocking certain package versions, they can declare so in a shared constraints file (or system uv configuration); uv can then use the constraints to skip blocked versions.  As highlighted from the pip docs, this seems to be part of the original intent behind constraint files.

---

_Label `configuration` added by @charliermarsh on 2024-11-29 16:39_

---

_Label `needs-decision` added by @charliermarsh on 2024-11-29 16:39_

---
