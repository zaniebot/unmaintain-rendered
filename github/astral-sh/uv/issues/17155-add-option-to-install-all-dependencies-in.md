---
number: 17155
title: Add option to install all dependencies in editable mode
type: issue
state: closed
author: danijar
labels:
  - enhancement
assignees: []
created_at: 2025-12-16T22:34:50Z
updated_at: 2025-12-21T21:43:09Z
url: https://github.com/astral-sh/uv/issues/17155
synced_at: 2026-01-07T13:12:19-06:00
---

# Add option to install all dependencies in editable mode

---

_Issue opened by @danijar on 2025-12-16 22:34_

### Summary

We have a monorepo with many packages and are considering the workspace feature, but also have concerns about requiring globally consistent versions for the whole code base (e.g. one obscure package can hold back upgrading a dependency for the whole organization).

Using path sources for the dependencies between packages in our monorepo would solve that problem. However, it makes it hard to install dependencies in editable mode. The `editable=true` needs to be specified for each source in every single pyproject.toml. Moreover, we only want editable installs for development, not deployment, which would require us to search/replace the flag across the whole repo.

Would it make sense to add a global config (or command-line flag) to enable/disable editable dependency installs for the whole monorepo?

### Example

_No response_

---

_Label `enhancement` added by @danijar on 2025-12-16 22:34_

---

_Referenced in [astral-sh/uv#17154](../../astral-sh/uv/issues/17154.md) on 2025-12-16 23:37_

---

_Comment by @konstin on 2025-12-17 09:23_

`uv sync` has a `--no-editable` flag for this use case: https://docs.astral.sh/uv/reference/cli/#uv-run--no-editable.

---

_Comment by @danijar on 2025-12-17 20:19_

Great, thanks! What I'd really like is the opposite, a `--editable` flag or `UV_EDITABLE` env variable that affects all local dependencies.

But it seems like a workspace is the only way to get that right now (which then would restrict us too much by enforcing a single lock file for the whole monorepo)?

---

_Comment by @pstrzelczak on 2025-12-18 07:39_

Can `--no-editable` be also supported in pip interface `uv pip sync`?

---

_Comment by @konstin on 2025-12-18 09:40_

> But it seems like a workspace is the only way to get that right now (which then would restrict us too much by enforcing a single lock file for the whole monorepo)?

Yes, currently the default for path dependencies is a non-editable install.

---

_Comment by @pstrzelczak on 2025-12-19 13:03_

> Can `--no-editable` be also supported in pip interface `uv pip sync`?

Submitted separate enhancement request [here](https://github.com/astral-sh/uv/issues/17189).

---

_Comment by @danijar on 2025-12-19 20:57_

> Yes, currently the default for path dependencies is a non-editable install.

@konstin Do you think making this default configurable would be reasonable?

For context, we're in a monorepo with a lot of projects. We can't use uv workspace because we need to allow different version locks for different projects in the repo. So right now, we have to specify `editable=true` many hundred times in all our pyproject.toml files. Configuring it with one line per pyproject.toml would be really helpful (specifying it once for the whole monorepo would be even better but I can't see a way to support that currently).

---

_Comment by @zanieb on 2025-12-19 23:19_

We already support `--editable`, I think, it's just hidden?

---

_Comment by @danijar on 2025-12-21 21:43_

Nice, thanks!

---

_Closed by @danijar on 2025-12-21 21:43_

---

_Referenced in [astral-sh/uv#17189](../../astral-sh/uv/issues/17189.md) on 2025-12-23 16:01_

---
