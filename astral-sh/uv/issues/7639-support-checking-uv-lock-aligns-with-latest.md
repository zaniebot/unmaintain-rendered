---
number: 7639
title: "Support checking `uv.lock` aligns with latest changes to `pyproject.toml`"
type: issue
state: closed
author: butterlyn
labels: []
assignees: []
created_at: 2024-09-23T13:49:30Z
updated_at: 2025-12-06T00:15:36Z
url: https://github.com/astral-sh/uv/issues/7639
synced_at: 2026-01-10T01:24:17Z
---

# Support checking `uv.lock` aligns with latest changes to `pyproject.toml`

---

_Issue opened by @butterlyn on 2024-09-23 13:49_

Request for a feature to check that no (relevant) changes to `pyproject.toml` have been made since `uv.lock` was last updated.
For reference, this is implemented in Poetry via the command `poetry check --lock` (previously under `poetry lock --check`).

---

_Comment by @BurntSushi on 2024-09-23 13:52_

Does `uv lock --locked` work for you?

---

_Comment by @zanieb on 2024-09-23 14:01_

We might want to add an alias for `--locked` in the `uv lock` command — `--locked` exists to match the rest of the commands but it's non-obvious what it means.

---

_Comment by @butterlyn on 2024-09-23 14:13_

Checked `uv lock --locked`, a few points:

- The requested feature is not about detecting if there are possible updates to `uv.lock` file due to new releases of dependencies - it's about detecting if the `pyproject.toml` was changed since the current `uv.lock` file was created.
- The use case would be for raising an error in a CICD pipeline or pre-commit hook to catch if a developer changed a dependency or `uv.sources` field in `pyproject.toml` but forgot to commit a corresponding new `uv.lock` to the repo.
- The requested feature, ideally, wouldn't involve actually resolving any dependencies and could be performed in a CICD pipeline that may not have the correct connections set up to update the `uv.lock` file

Currently using `poetry check --lock` in CICD pipelines, trying to migrate to uv but can't find an equivalent command.

I believe Poetry achieves this by hashing relevant `pyproject.toml` inputs and adding the hash to the lock file to quickly detect changes? Though I'm not entirely sure.

---

_Comment by @charliermarsh on 2024-09-23 14:15_

I think everything you've outlined there is also true of `uv lock --locked`.

---

_Comment by @charliermarsh on 2024-09-23 14:18_

For example:

```
❯ uv lock --locked --offline
Resolved 8 packages in 1ms
```

---

_Comment by @zanieb on 2024-09-23 14:19_

Yeah similarly:

```
❯ uv lock --locked --offline
Resolved 1 package in 2ms

❯ uv add anyio --frozen

❯ uv lock --locked --offline
Resolved 4 packages in 5ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

---

_Comment by @butterlyn on 2024-09-23 14:21_

Thank you 😄 will use `uv lock --locked --offline` in that case

Reading the command description ("Assert that the `uv.lock` will remain unchanged"), I didn't pick up that this was the intended usage... if it's outlined in the docs then I missed it

---

_Assigned to @zanieb by @zanieb on 2024-09-23 14:21_

---

_Comment by @zanieb on 2024-09-23 14:21_

I'll follow up with some improvements here.

---

_Comment by @danyeaw on 2024-10-06 00:12_

If I set a constraint on a dependency, like `dependencies = [ 'exceptiongroup>=1.0.0; python_version < "3.11"',]`, then running `uv lock --locked --offline` results in an error:

```
Using CPython 3.12.6 interpreter at: /opt/hostedtoolcache/Python/3.12.6/x64/bin/python
  × No solution found when resolving dependencies:
  ╰─▶ Because only exceptiongroup{python_full_version < '3.11'}<1.0.0 is
      available and your project depends on exceptiongroup{python_full_version
      < '3.11'}>=1.0.0, we can conclude that your project's requirements are
      unsatisfiable.
```

---

_Comment by @zanieb on 2024-10-06 14:31_

We presume we can't query the index to discover more versions of `exceptiongroup` — it sounds correct that we fail there?

---

_Comment by @danyeaw on 2024-10-06 18:43_

Do I need to query / build the index somehow before running the `--locked --offline` command, then?

---

_Comment by @zanieb on 2024-10-06 22:38_

@danyeaw what's your goal? If the lockfile is up to date, I don't think we'll query the index and the command should succeed. If not, it'll fail.

---

_Comment by @danyeaw on 2024-10-06 22:51_

@zanieb I have one extra dependency for my project that is only applicable for older versions of Python. I want to then check the lock file integrity in CI, and it fails with the error above. The lock file is up to date.

---

_Referenced in [astral-sh/uv#9653](../../astral-sh/uv/issues/9653.md) on 2024-12-05 08:56_

---

_Referenced in [astral-sh/uv#9662](../../astral-sh/uv/pulls/9662.md) on 2024-12-05 15:30_

---

_Referenced in [johnthagen/python-blueprint#238](../../johnthagen/python-blueprint/issues/238.md) on 2024-12-06 14:13_

---

_Closed by @zanieb on 2024-12-08 16:02_

---

_Closed by @zanieb on 2024-12-08 16:02_

---

_Referenced in [astral-sh/uv#10176](../../astral-sh/uv/issues/10176.md) on 2024-12-26 17:56_

---

_Comment by @hbelmiro on 2025-12-06 00:15_

Native uv requires uv sync --locked which attempts to sync the environment. My tool performs the static check you are looking for.

https://github.com/hbelmiro/uv-lock-check

---
