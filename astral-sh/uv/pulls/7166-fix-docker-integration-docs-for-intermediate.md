```yaml
number: 7166
title: Fix Docker integration docs for intermediate layers
type: pull_request
state: closed
author: NiklasRosenstein
labels: []
assignees: []
base: main
head: patch-1
created_at: 2024-09-07T12:27:59Z
updated_at: 2025-11-06T01:41:38Z
url: https://github.com/astral-sh/uv/pull/7166
synced_at: 2026-01-10T06:28:12Z
```

# Fix Docker integration docs for intermediate layers

---

_Pull request opened by @NiklasRosenstein on 2024-09-07 12:27_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The Docker integration docs were missing to mount the `uv.lock` and `pyproject.toml` which are required also for the second layer that runs `uv sync --locked`.

It also failed to mention that if your `pyproject.toml` references a readme file, that it must be mounted as well.


---

_Comment by @NiklasRosenstein on 2024-09-07 12:31_

I would also suggest that we add `--link-mode=copy` to the command to prevent this warning:

```
#11 0.340 warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
#11 0.340          If the cache and target directories are on different filesystems, hardlinking may not be supported.
#11 0.340          If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
```

Uv detects that the cache sits on a different filesystem (yay!), but it's probably a good idea to make this explicit in the Dockerfile.

It's also mandatory to use this option, even if you don't use a cache mount, if you plan on copying only your project directory from another stage (e.g. when combining multiple stages), you likely won't think about copying (and merging!) the UV cache directory. Admittedly, that is a bit of an edge case.

Adding `--compile-bytecode` might also be a good idea!

edit: I've made these changes in ea61ed1c4308a942e5773ca9ebf8adc1b3c8b7db, let me know what you think.

---

_@zanieb reviewed on 2024-09-07 14:41_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:351 on 2024-09-07 14:41_

Wouldn't these be added by the `ADD . /app` invocation?

---

_@zanieb reviewed on 2024-09-07 14:42_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:343 on 2024-09-07 14:42_

I think I have a preference for setting an environment variable for this as in https://github.com/astral-sh/uv-docker-example/pull/16

I think we cover bytecode compilation in a separate example though https://docs.astral.sh/uv/guides/integration/docker/#compiling-bytecode

---

_Comment by @zanieb on 2024-09-07 14:51_

Thanks for contributing! I have a couple questions :)

---

_@tomaszbk reviewed on 2024-09-07 17:26_

---

_Review comment by @tomaszbk on `docs/guides/integration/docker.md`:343 on 2024-09-07 17:26_

I agree that bytecode compilation shouldn't be included in this example, as it is already covered afterwards

---

_Review comment by @NiklasRosenstein on `docs/guides/integration/docker.md`:351 on 2024-09-07 18:54_

Oh my, I blame tunnel vision. 🤦 

In my case I added only the `src` directory because `ADD . /app` adds a lot of things that I don't want to add. Thinking about it, with `--link-mode=copy`, I could probably even `--mount` the source directory itself as well? Arguably, a very minor optimization as you don't usually have many hundreds of megabytes of source code. 

---

_@NiklasRosenstein reviewed on 2024-09-07 18:54_

---

_@NiklasRosenstein reviewed on 2024-09-07 18:55_

---

_Review comment by @NiklasRosenstein on `docs/guides/integration/docker.md`:343 on 2024-09-07 18:55_

Fair point, although I feel obliged to point out that it also talks about caching later but uses a cache mount in this example already. :)

---

_@zanieb reviewed on 2024-09-07 18:58_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:343 on 2024-09-07 18:58_

I think I missed that when someone added the bind mounts to the example in #6921 — we should probably drop that from here too for clarity.

---

_@zanieb reviewed on 2024-09-07 19:00_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:351 on 2024-09-07 19:00_

The source directory needs to stick around because we don't support non-editable installs of workspace members during syncs yet (#5792). Maybe it'd be best to address your case in the documentation by adding a case to the optimization examples for copying the src directory instead of the full project directory?

---

_Closed by @NiklasRosenstein on 2025-11-06 01:41_

---
