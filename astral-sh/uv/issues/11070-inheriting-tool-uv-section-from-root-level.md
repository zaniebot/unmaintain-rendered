---
number: 11070
title: "Inheriting [tool.uv] section from root-level `pyproject.toml` when desired"
type: issue
state: open
author: uwu-420
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2025-01-29T17:02:17Z
updated_at: 2025-11-03T09:30:13Z
url: https://github.com/astral-sh/uv/issues/11070
synced_at: 2026-01-10T01:25:01Z
---

# Inheriting [tool.uv] section from root-level `pyproject.toml` when desired

---

_Issue opened by @uwu-420 on 2025-01-29 17:02_

### Summary

Hi all, thanks for making uv!

I'm trying to make a monorepo with uv and I'd love to do some base uv configuration in the root level `pyproject.toml`. Things that are most likely to be the same for all packages, e.g. package indices. But I'm not using workspaces because the packages will have conflicting dependencies. This is also listed as a reason in the docs, see https://docs.astral.sh/uv/concepts/projects/workspaces/#when-not-to-use-workspaces But some packages will have some custom uv config in `[tool.uv]` in `pyproject.toml` but I noticed that this will cause the package to ignore everything set in `[tool.uv]` by the root `pyproject.toml`. I suppose this is intentional, but I could imagine my use case being relevant as well.

After all, I saw that ruff offers some explicit opt-in to inherit from some other config, see https://docs.astral.sh/ruff/settings/#extend which is really good for my use case! (I wish pyright and pytest would make it as easy... edit: pyright actually has an `extends` option, see [here](https://microsoft.github.io/pyright/#/configuration?id=environment-options). pytest does not prioritize that as of now, see [here](https://github.com/pytest-dev/pytest/issues/13173))

Relatedly, there already seems to be some config merging logic https://docs.astral.sh/uv/configuration/files/ but it's only for `uv.toml` user/system-level files.

### Example

Not exactly sure about the exact design of it but having this

```toml
# ./pyproject.toml
[tool.uv]
required-version = ">=0.5.25"
```
and
```toml
# ./sub/pyproject.toml
[tool.uv]
environments = ["sys_platform == 'linux'"]
```
it would be cool to have some way to let the `./sub/pyproject.toml` inherit the `required-version` from `./pyproject.toml`.

Maybe like ruff does it
```toml
# ./sub/pyproject.toml
[tool.uv]
extend = "../pyproject.toml"
environments = ["sys_platform == 'linux'"]

---

_Label `enhancement` added by @uwu-420 on 2025-01-29 17:02_

---

_Comment by @matthewadams on 2025-01-30 13:50_

You might be interested in https://github.com/astral-sh/uv/issues/10960

---

_Label `configuration` added by @charliermarsh on 2025-01-30 14:11_

---

_Comment by @charliermarsh on 2025-01-30 14:19_

I think we're somewhat unlikely to do this, but I understand the problem you're facing. I don't _think_ we'd do the same thing in Ruff if we were to re-implement it today. The inheritance causes a lot of complexity and confusion for users. We _do_ support inheritance of user- and system-level settings (e.g., `~/.config/uv/uv.toml` as in https://docs.astral.sh/uv/configuration/files/ -- that gets combined with your `pyproject.toml`), so that's one option, but it doesn't help with sharing across a team.


---

_Comment by @jvacek on 2025-02-06 11:59_

Smells related to my issue https://github.com/astral-sh/uv/issues/5596

---

_Comment by @tibbe on 2025-07-09 14:51_

We're facing this as well as our monorepo gets larger. Each package probably has a 100 lines worth of cut-n-pasted `pyproject.toml` to configure ruff, mypy, and pytest.

---

_Comment by @tjhunter on 2025-11-03 09:30_

We "solve" this issue with a script that compares shared sections of pyproject.toml and triggers an error if they deviate:
https://github.com/ecmwf/WeatherGenerator/blob/develop/scripts/check_tomls.py

---

_Referenced in [ecmwf/WeatherGenerator#1177](../../ecmwf/WeatherGenerator/pulls/1177.md) on 2025-11-03 09:30_

---
