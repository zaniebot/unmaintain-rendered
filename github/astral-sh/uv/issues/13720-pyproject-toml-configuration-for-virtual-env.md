---
number: 13720
title: "`pyproject.toml` configuration for `VIRTUAL_ENV`"
type: issue
state: closed
author: namanshenoy
labels:
  - enhancement
assignees: []
created_at: 2025-05-29T19:42:16Z
updated_at: 2025-06-09T19:55:33Z
url: https://github.com/astral-sh/uv/issues/13720
synced_at: 2026-01-07T13:12:18-06:00
---

# `pyproject.toml` configuration for `VIRTUAL_ENV`

---

_Issue opened by @namanshenoy on 2025-05-29 19:42_

### Summary

Although it is possible to configure a `VIRTUAL_ENV` configuration, the virtualenv directory that is preferred on my team is `env`.

We have build scripts and `Makefile`s that fully configure our project, so that a new developer can get up and running quickly, i.e, have all dependencies installed, all docker images built, the right python version installed, pre-commit installed, etc.

We could also use `--active`, but that feels a little clunky for my use-case, since if I read correctly, `--active` needs to be used for every `uv` command.

I'd like to be able to configure the virtualenv that `uv` uses via the `pyproject.toml`, so that all developers can use the same virtualenv.

My proposal is to ad a `virtual_env` setting in `pyproject.toml` so that this can be configured on a project basis.

The precedence, in decreasing order I'd suggest would be:
1. The `VIRTUAL_ENV` environment variable
2. `virtual_env` configuration in `pyproject.toml`
3. The default `.venv` directory.

Thanks for the consideration, love the project!

### Example

_No response_

---

_Label `enhancement` added by @namanshenoy on 2025-05-29 19:42_

---

_Comment by @oconnor663 on 2025-05-29 20:57_

I'm in the middle of adding a new env var that you could use instead of passing `--active` explicitly to every command. Any chance this would work for your use case? https://github.com/astral-sh/uv/pull/13714

---

_Comment by @namanshenoy on 2025-06-02 22:44_

That would be helpful as well, but my use case is slightly different. I want to configure the virtual environment location via configuration, rather than relying on collaborators to create a virtualenv manually and use the --active flag, which I believe is how your PR works—if I understood it correctly 🙂.

Including the environment directory in the configuration code is more explicit than setting it through an environment variable.

---

_Comment by @zanieb on 2025-06-02 22:57_

It sounds like you're asking for the same thing as https://github.com/astral-sh/uv/issues/12587 but I don't think we'll add support in the `pyproject.toml` as described at https://github.com/astral-sh/uv/issues/12587#issuecomment-2770642225

---

_Closed by @zanieb on 2025-06-09 19:55_

---
