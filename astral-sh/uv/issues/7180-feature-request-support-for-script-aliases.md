```yaml
number: 7180
title: "Feature request: support for script aliases"
type: issue
state: closed
author: matejpekar
labels:
  - duplicate
assignees: []
created_at: 2024-09-07T20:53:25Z
updated_at: 2024-09-07T23:38:24Z
url: https://github.com/astral-sh/uv/issues/7180
synced_at: 2026-01-12T15:59:11Z
```

# Feature request: support for script aliases

---

_@matejpekar_

Hi! Our team currently relies heavily on **PDM** and we'd like to migrate to **uv**. However, we're missing a crucial feature: the ability to define and run [script aliases](https://pdm-project.org/latest/usage/scripts) directly from the `pyproject.toml` file.

Here’s an example of the scripts we currently use to streamline our workflow:
```pyproject.toml
[tool.pdm.scripts]
train = "python -m project mode=fit"
validate = "python -m project mode=validate"
test = "python -m project mode=test"
l = { composite = ["lint", "format"] }
lint = "ruff check --fix"
format = "ruff format"
post_install = { composite = [
    "pre-commit autoupdate",
    "pre-commit install",
    "pre-commit install --hook-type commit-msg",
] }
```

This setup allows us to:

- Easily run various modes (e.g., `pdm train <...args>`).
- Automatically install `pre-commit` hooks when running `pdm install`.
- Reference these scripts in our GitLab CI/CD templates.


### Key Features:
- **Script Definition in `pyproject.toml`**: Ability to define shell scripts that can be run directly from the `pyproject.toml` file.
- **Composite Commands**: Support for composite scripts, where a single command triggers multiple actions (e.g., `l` runs both `lint` and `format`).
- **Pre/Post Hooks**: Hooks like `post_sync` to automate tasks such as installing `pre-commit` hooks after `uv sync`.
- **Script Skipping**: A way to skip certain scripts, possibly with a `--skip` flag.


### Additional Suggestion:
It would be great if we could run scripts directly without an intermediary command like `run`. For example, instead of `uv run <script>`, we could use `uv <script>`. In case of a conflict with default commands, `uv run <script>` could be used as a fallback.

---

We’d love to see similar functionality in **uv**, inspired by PDM or npm/yarn.

---

_Comment by @gusutabopb on 2024-09-07 23:26_

Looks like a duplicate of https://github.com/astral-sh/uv/issues/5903

---

_Comment by @zanieb on 2024-09-07 23:38_

Yep I think this is covered there already. Thanks for the write-up though!

---

_Closed by @zanieb on 2024-09-07 23:38_

---

_Label `duplicate` added by @zanieb on 2024-09-07 23:38_

---
