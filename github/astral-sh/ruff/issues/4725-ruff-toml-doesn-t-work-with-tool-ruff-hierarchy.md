---
number: 4725
title: "ruff.toml doesn't work with [tool.ruff] hierarchy"
type: issue
state: closed
author: turnerm
labels: []
assignees: []
created_at: 2023-05-30T12:56:01Z
updated_at: 2025-04-29T13:28:58Z
url: https://github.com/astral-sh/ruff/issues/4725
synced_at: 2026-01-07T13:12:14-06:00
---

# ruff.toml doesn't work with [tool.ruff] hierarchy

---

_Issue opened by @turnerm on 2023-05-30 12:56_

The `ruff` [documentation](https://beta.ruff.rs/docs/configuration/#using-rufftoml) states the following: 

> As an alternative to pyproject.toml, Ruff will also respect a ruff.toml (or .ruff.toml) file, which implements an equivalent schema (though the [tool.ruff] hierarchy can be omitted).

In particular, stating that the `[tool.ruff]` hierarchy _can_ be omitted implies that it should be possible to use the hierarchy in `ruff.toml`.  

However, I do not find this to be the case. In particular, I've created the following `ruff.toml`:

```toml
[tool.ruff]
line-length = 79
```

Then with Python 3.11 and `ruff` 0.0.270 I tried running the command `ruff check *`, and get the following error:
```shell
error: Failed to parse `/home/turnerm/scratch/ruff_test/ruff.toml`: TOML parse error at line 1, column 2
  |
1 | [tool.ruff]
  |  ^^^^
unknown field `tool`, expected one of `allowed-confusables`, `builtins`, `cache-dir`, `dummy-variable-rgx`, `exclude`, `extend`, `extend-exclude`, `extend-include`, `extend-ignore`, `extend-select`, `extend-fixable`, `extend-unfixable`, `external`, `fix`, `fix-only`, `fixable`, `format`, `force-exclude`, `ignore`, `ignore-init-module-imports`, `include`, `line-length`, `tab-size`, `required-version`, `respect-gitignore`, `select`, `show-source`, `show-fixes`, `src`, `namespace-packages`, `target-version`, `task-tags`, `typing-modules`, `unfixable`, `flake8-annotations`, `flake8-bandit`, `flake8-bugbear`, `flake8-builtins`, `flake8-comprehensions`, `flake8-errmsg`, `flake8-quotes`, `flake8-self`, `flake8-tidy-imports`, `flake8-type-checking`, `flake8-gettext`, `flake8-implicit-str-concat`, `flake8-import-conventions`, `flake8-pytest-style`, `flake8-unused-arguments`, `isort`, `mccabe`, `pep8-naming`, `pycodestyle`, `pydocstyle`, `pylint`, `per-file-ignores`, `extend-per-file-ignores`
```

If I change the name of `ruff.toml` to `pyproject.toml`, or remove the `[ruff.toml]` heading, then `ruff` runs without error. 

---

_Referenced in [OCHA-DAP/hdx-python-utilities#16](../../OCHA-DAP/hdx-python-utilities/pulls/16.md) on 2023-05-30 12:56_

---

_Comment by @charliermarsh on 2023-05-30 13:19_

The documentation should be clearer on this point: `[tool.ruff]` _has_ to be omitted for `ruff.toml` (it's not optional).

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-30 17:25_

---

_Referenced in [astral-sh/ruff#4732](../../astral-sh/ruff/pulls/4732.md) on 2023-05-30 17:27_

---

_Closed by @charliermarsh on 2023-05-30 17:35_

---

_Comment by @cartok on 2023-10-31 13:58_

People will still land here cause the preset is using it. https://github.com/astral-sh/ruff#configuration.


---

_Comment by @phillipjohnston on 2024-03-13 15:06_

> People will still land here cause the preset is using it. https://github.com/astral-sh/ruff#configuration.

 I did this just now while working through the readme.

---

_Comment by @charliermarsh on 2024-03-13 15:07_

I don't think we can put tabs in the README (unlike in the docs). I'll try to clarify it.

---

_Referenced in [astral-sh/ruff#10393](../../astral-sh/ruff/pulls/10393.md) on 2024-03-13 18:07_

---

_Comment by @AlexWaygood on 2024-03-13 18:12_

> I don't think we can put tabs in the README (unlike in the docs).

Yeah that seems correct :/ https://github.com/github/markup/issues/1552#issuecomment-1554743574

---

_Comment by @michealroberts on 2025-04-29 13:20_

I'm here in 2025:

```
error: Failed to parse `/workspaces/celerity/ruff.toml`: TOML parse error at line 33, column 1
   |
33 | indent-width = 4
```

Seems like I have ruff 0.0.* installed, and needed to bump minor versions. If anyone encounters this error.

---
