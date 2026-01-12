```yaml
number: 13450
title: "Specify venv location for `uv run --script`"
type: issue
state: closed
author: ValeKnappich
labels:
  - question
assignees: []
created_at: 2025-05-14T11:51:53Z
updated_at: 2025-05-15T08:21:07Z
url: https://github.com/astral-sh/uv/issues/13450
synced_at: 2026-01-12T16:01:29Z
```

# Specify venv location for `uv run --script`

---

_@ValeKnappich_

### Question

When running a script with inline dependencies, `uv run --script` currently creates a temporary venv in the cache directory, which makes sense as a default. But it would be nice to be able to specify a fixed path for the venv, e.g. to point to in the vscode python extension config. 

Is there currently a way to achieve a static venv location for `uv run --script`?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @ValeKnappich on 2025-05-14 11:51_

---

_Comment by @konstin on 2025-05-14 12:05_

We're planning for better IDE integration which will help VS Code to find the script venv automatically without any manual action.

In the meantime, you can use `uv sync --script` to install the dependencies in your venv.

---

_Comment by @ValeKnappich on 2025-05-14 12:23_

Thanks for the quick reply @konstin! Dedicated IDE support for this would be amazing.

Can I specify a fixed location for the venv with `uv sync --script`? It apparently also creates the venv in the cache dir by default

---

_Comment by @konstin on 2025-05-14 12:33_

There's [`UV_PROJECT_ENVIRONMENT`](https://docs.astral.sh/uv/configuration/environment/#uv_project_environment), though that's more of an escape hatch for container deployment.

---

_Comment by @charliermarsh on 2025-05-15 02:35_

One thing you can do is pass `--active`, then we'll sync to whatever the _active_ virtual environment is.

So you could do: `VIRTUAL_ENV=foo uv sync --script main.py --active`

---

_Comment by @ValeKnappich on 2025-05-15 08:14_

Thanks @charliermarsh. That could be a workaround albeit with some extra steps compared to what I originally intended.

For reference, my original use case was to create a separate environment for one script in a project that requires a set of dependencies that is not compatible with the remaining project. For now, I just created a separate uv project (`uv venv` would also work but I prefer the project-based dependency definition) as shown below and activate the respective venv as required. Feel free to chip in if there is a better way to have multiple venv's in one project. Closing this for now.

```
.venv
    bin
    ...
.venv-separate
    .venv
        bin
    pyproject.toml
 ```

---

_Closed by @ValeKnappich on 2025-05-15 08:14_

---
