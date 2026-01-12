```yaml
number: 13714
title: "define `UV_USE_ACTIVE_ENVIRONMENT` as an alternative to `--active`"
type: pull_request
state: closed
author: oconnor663
labels: []
assignees: []
base: main
head: jack/uv_active
created_at: 2025-05-29T00:31:08Z
updated_at: 2025-09-12T12:19:42Z
url: https://github.com/astral-sh/uv/pull/13714
synced_at: 2026-01-12T16:10:49Z
```

# define `UV_USE_ACTIVE_ENVIRONMENT` as an alternative to `--active`

---

_@oconnor663_

We considered `UV_PROJECT_ENVIRONMENT=active`, but that env var doesn't affect script environments, and we probably want this to work the same way for both.

Closes https://github.com/astral-sh/uv/issues/11273.

---

_Review requested from @zanieb by @oconnor663 on 2025-05-29 00:31_

---

_Comment by @zanieb on 2025-05-29 12:11_

I worry this name misses that it's scoped to projects. We _do_ use the active environment elsewhere.

> We considered UV_PROJECT_ENVIRONMENT=active, but that env var doesn't affect script environments, and we probably want this to work the same way for both.

This is.. fair. Hm..

---

_Assigned to @zanieb by @zanieb on 2025-05-29 12:11_

---

_Comment by @zanieb on 2025-05-29 12:31_

Let's see if others have opinions, e.g., @konstin 

---

_@zanieb reviewed on 2025-05-29 12:32_

---

_Review comment by @zanieb on `docs/configuration/environment.md`:435 on 2025-05-29 12:32_

Should we explain a bit more of the nuance here? Like... that this is the default behavior outside of projects and scripts with inline metadata?

---

_Comment by @oconnor663 on 2025-05-29 15:26_

> We _do_ use the active environment elsewhere.

Ah, I was missing that. Now I see https://docs.astral.sh/uv/pip/environments/#using-arbitrary-python-environments re: `uv install` and `uv pip install`. Are there other commands that use the active environment without `--active`?

---

_Comment by @zanieb on 2025-05-29 15:32_

(`uv install` doesn't exist)

`uv run` outside of a project
`uv run foo.py` if it's not a script with inline metadata and you're outside of a project
`uv pip *` all of the commands here
`uv python find` outside of a project

---

_Comment by @oconnor663 on 2025-05-29 20:54_

Here's some improved wording. How does this sound?

>Equivalent to the `--active` command line argument for commands that support it. For
example, when running inside a project or when running a script with inline metadata,
`uv run` manages its own virtual environment and ignores the caller's active environment
(i.e. the `VIRTUAL_ENV` environment variable). Similarly, `uv sync` and `uv add` expect to
run in a project, and they use the managed virtual environment by default. The active
option causes these commands to use the caller's active virtual environment instead, if
there is one.



---

_Comment by @konstin on 2025-05-30 07:56_

To add an option to the bikeshed: `UV_SYNC_ACTIVE_ENVIRONMENT`, to clarify the scoping to project commands.

---

_Comment by @oconnor663 on 2025-06-02 16:24_

I like `UV_SYNC_ACTIVE_ENVIRONMENT`, as long as you think it's sufficiently obvious that in addition to _sync'ing_ the environment we actually _execute_ stuff inside it?

---

_Comment by @zanieb on 2025-06-02 16:46_

I don't think it's obvious that wouldn't be scoped to the `uv sync` command. 

---

_Comment by @zanieb on 2025-06-02 16:47_

(but maybe it makes sense as a step in the direction of `UV_SYNC` being configuration of the general "sync" step, we just don't have any `UV_SYNC` variables yet)

---

_Comment by @samypr100 on 2025-06-03 03:11_

Some more bikeshedding
* `UV_ACTIVE_VENV` --> more specific to an activated venv
* `UV_ACTIVE_ENV` --> alt
* `UV_DEFAULT_ENV` --> probably would want an explicit `active` here

---

_Comment by @zanieb on 2025-06-04 15:33_

A few concerns here

- An environment variable here would apply equally to projects and scripts with inline metadata, is that what people would want? Or would you still expect a script to be run in a separate environment? Does that imply we want separate variables? Do we just want to tackle the `UV_PROJECT_ENVIRONMENT` case? 
- How would this behave if `VIRTUAL_ENV` is not set? Would it error?
- Do we really want to make this easier to turn on in a persistent way? It's off the happy path and adds complexity?


Another option

- `UV_PROJECT_ENVIRONMENT=VIRTUAL_ENV`

---

_Comment by @oconnor663 on 2025-06-05 16:47_

I have some more follow-up questions about the intended behavior on the issue: https://github.com/astral-sh/uv/issues/11273#issuecomment-2945223978

---

_Comment by @zanieb on 2025-09-12 12:19_

Let's defer this for now.

---

_Closed by @zanieb on 2025-09-12 12:19_

---
