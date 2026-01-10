```yaml
number: 6755
title: Confusing error message when project directory is changed
type: issue
state: open
author: zanieb
labels:
  - error messages
  - projects
assignees: []
created_at: 2024-08-28T15:14:24Z
updated_at: 2024-09-06T13:25:17Z
url: https://github.com/astral-sh/uv/issues/6755
synced_at: 2026-01-10T04:45:09Z
```

# Confusing error message when project directory is changed

---

_Issue opened by @zanieb on 2024-08-28 15:14_

e.g.

```
$ uv init uv-example-flask
$ uv --directory uv-example-flask add flask
$ mv uv-example-flask uv-flask-example
$ cd uv-flask-example
$ uv run -v flask
DEBUG uv 0.3.5
DEBUG Found project root: `/Users/zb/workspace/uv-flask-example`
DEBUG No workspace root found, using project root
DEBUG Discovered project `uv-example-flask` at: /Users/zb/workspace/uv-flask-example
DEBUG The virtual environment's Python version satisfies `Python >=3.11`
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `/Users/zb/.cache/uv/built-wheels-v3/path/c88ead026f48e749`
DEBUG Found static `pyproject.toml` for: uv-example-flask @ file:///Users/zb/workspace/uv-flask-example
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 9 packages in 3ms
DEBUG Using request timeout of 30s
DEBUG Requirement already installed: blinker==1.8.2
DEBUG Requirement already installed: click==8.1.7
DEBUG Requirement already installed: flask==3.0.3
DEBUG Requirement already installed: itsdangerous==2.2.0
DEBUG Requirement already installed: jinja2==3.1.4
DEBUG Requirement already installed: markupsafe==2.1.5
DEBUG Requirement already installed: werkzeug==3.0.4
Audited 7 packages in 0.08ms
DEBUG Using Python 3.11.7 interpreter at: /Users/zb/workspace/uv-flask-example/.venv/bin/python3
DEBUG Running `flask`
error: Failed to spawn: `flask`
  Caused by: No such file or directory (os error 2)
$ /Users/zb/workspace/uv-flask-example/.venv/bin/flask
zsh: /Users/zb/workspace/uv-flask-example/.venv/bin/flask: bad interpreter: /Users/zb/workspace/uv-example-flask/.venv/bin/python: no such file or directory
```

---

_Label `error messages` added by @zanieb on 2024-08-28 15:14_

---

_Label `projects` added by @zanieb on 2024-08-28 15:14_

---

_Comment by @charliermarsh on 2024-08-28 15:16_

I think it'd be interesting to consider using relocatable venvs in the project API. It would solve this problem entirely and be a lot more intuitive for users IMO.

The main downside is that copying a script out of an environment would _not_ work, since that script would contain a relative path. But we support that with `uv tool`, or you can use `uv venv` etc.

---

_Comment by @zanieb on 2024-08-28 15:24_

Yeah basically agree we should change the default.

---

_Comment by @michaeloliverx on 2024-09-06 09:07_

I am getting this error in my CI environment, how do I fix it?
```
$ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.4.6 x86_64-unknown-linux-gnu
installing to /root/.cargo/bin
  uv
  uvx
everything's installed!
To add $HOME/.cargo/bin to your PATH, either restart your shell or run:
    source $HOME/.cargo/env (sh, bash, zsh)
    source $HOME/.cargo/env.fish (fish)
$ source $HOME/.cargo/env
$ uv --version
uv 0.4.6
$ uv sync --verbose
DEBUG uv 0.4.6
DEBUG Found project root: `/var/www/builds/biorelate/apps/galactic-api`
DEBUG No workspace root found, using project root
DEBUG Searching for Python >=3.11 in managed installations or system path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found `cpython-3.11.2-linux-x86_64-gnu` at `/usr/local/bin/python3` (search path)
Using Python 3.11.2 interpreter at: /usr/local/bin/python3
Creating virtualenv at: .venv
DEBUG Using request timeout of [30](https://gitlab.com/biorelate/apps/galactic-api/-/jobs/7761728283#L30)s
DEBUG Acquired lock for `/var/www/builds/biorelate/apps/galactic-api/.cache/uv/built-wheels-v3/editable/3be25bafd0800626`
DEBUG Found static `pyproject.toml` for: galactic-api @ file:///var/www/builds/biorelate/apps/galactic-api
DEBUG No workspace root found, using project root
DEBUG Released lock at `/var/www/builds/biorelate/apps/galactic-api/.cache/uv/built-wheels-v3/editable/3be25bafd0800626/.lock`
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 109 packages in 2ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached requirement: aiohttp==3.9.3
... <snip>
Audited 85 packages in 0.43ms
DEBUG Using Python 3.11.2 interpreter at: /var/www/builds/biorelate/apps/galactic-api/.venv/bin/python3
DEBUG Running `mypy .`
error: Failed to spawn: `mypy`
  Caused by: No such file or directory (os error 2)
```

---

_Comment by @zanieb on 2024-09-06 13:25_

@michaeloliverx please open a new issue and include the full logs.

---
