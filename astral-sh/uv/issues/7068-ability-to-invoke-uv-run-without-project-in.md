```yaml
number: 7068
title: "Ability to invoke `uv run` without `[project]` in `pyproject.toml`"
type: issue
state: closed
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2024-09-05T06:24:49Z
updated_at: 2024-09-05T17:02:14Z
url: https://github.com/astral-sh/uv/issues/7068
synced_at: 2026-01-12T15:59:10Z
```

# Ability to invoke `uv run` without `[project]` in `pyproject.toml`

---

_@jamesbraza_

I have a repo that I am slowly moving to use `uv`. For now, it doesn't have a `[project]` table in a particular `pyproject.toml`, but this seems to stop me from using `uv run`.

What I want to do is essentially this:

```bash
> curl -LsSf https://astral.sh/uv/install.sh | sh
> uv pip install -r requirements.txt
> ls
pyproject.toml requirements.txt script.py
> uv run script.py
error: No `project` table found in: `/path/to/repo/pyproject.toml`
```

I am using `uv==0.4.5`. The last command is the issue, I would like `uv run` to be able to invoke a Python script, even if the `pyproject.toml` has no `[project]` section in it.

---

_Comment by @AlexWaygood on 2024-09-05 09:49_

If all of your requirements are specified in `requirements.txt` rather than `pyproject.toml`, you should be able to do something like this:

```bash
> curl -LsSf https://astral.sh/uv/install.sh | sh
> uv run --no-project --with-requirements requirements.txt script.py
```

The `--no-project` flag tells uv to ignore the `pyproject.toml` file. Otherwise, `uv run` will try to install the project and all dependencies listed in the `pyproject.toml` file, which is impossible if the `pyproject.toml` file has no `[project]` table.

---

_Comment by @charliermarsh on 2024-09-05 13:15_

Yeah, you may be looking for `--no-project`!

---

_Label `question` added by @zanieb on 2024-09-05 13:17_

---

_Comment by @jamesbraza on 2024-09-05 17:02_

<img src="https://media2.giphy.com/media/LfGGW219qNzLP6IzTA/giphy.gif"/>

I did not know `--no-project` existed 😻. I promise I had even read the `uv run --help` manu. Sorry for false positive request, closing this out

---

_Closed by @jamesbraza on 2024-09-05 17:02_

---
