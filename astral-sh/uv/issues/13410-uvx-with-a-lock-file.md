```yaml
number: 13410
title: uvx with a lock file
type: issue
state: open
author: dimaqq
labels:
  - enhancement
assignees: []
created_at: 2025-05-12T13:43:24Z
updated_at: 2025-05-12T19:48:39Z
url: https://github.com/astral-sh/uv/issues/13410
synced_at: 2026-01-10T03:41:47Z
```

# uvx with a lock file

---

_Issue opened by @dimaqq on 2025-05-12 13:43_

### Summary

I've just ran into a situation where `uvx juju-doctor --help` fails, but `checkout; uv sync; juju-doctor --help` works, because (I'm taking some guesses here):

- `uv sync` uses the lock file
   - some-entrypoint runs with specific dependencies
- `uvx some-proj` loads the wheel
   - wheels don't contain lock files
      - some-entrypoint runs with newest allowed dependencies

There is a solution for direct deps.

There isn't really a solution for transient deps.

Writing out all the dependencies explicitly in `dist-info/METADATA` would probably be counter-productive, as that may hurt folks who use this package as a library.

### Example

https://github.com/pallets/click/issues/2908

---

_Label `enhancement` added by @dimaqq on 2025-05-12 13:43_

---

_Comment by @zanieb on 2025-05-12 16:19_

We're interested in something like `uvx --locked`, but it's complicated because (as you said) the wheel doesn't include the lockfile. I expect we'll design something here eventually, but it's not a high priority at this time.

---

_Comment by @Ark-kun on 2025-05-12 19:33_

I think I'm trying to do the same thing (please tell me if that's not the case), but in my case I use repo link (so uv.lock is present):

I have a project with `uv.lock` file. It has `click==8.1.8`.
https://github.com/Cloud-Pipelines/oasis-cli/blob/5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154/uv.lock#L77
Despite that, uv run always installs `click==8.2.0` which is a broken release.


```
% uv run --refresh --exact --with git+https://github.com/Cloud-Pipelines/oasis-cli.git@5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154 python -c "import click; print(click.__version__)" 
    Updated https://github.com/Cloud-Pipelines/oasis-cli.git (5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154)
Installed 29 packages in 16ms
8.2.0
```
```
% uv run --refresh --frozen --with git+https://github.com/Cloud-Pipelines/oasis-cli.git@5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154 python -c "import click; print(click.__version__)" 
    Updated https://github.com/Cloud-Pipelines/oasis-cli.git (5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154)
Installed 29 packages in 18ms
8.2.0
```

```
% uv run --refresh --locked --with git+https://github.com/Cloud-Pipelines/oasis-cli.git@5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154 python -c "import click; print(click.__version__)" 
    Updated https://github.com/Cloud-Pipelines/oasis-cli.git (5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154)
Installed 29 packages in 18ms
8.2.0
```

This seems problematic. I've pinned everything. I'm using exact pinned SHA commit and uv.lock. And yet the package got broken over the weekend.

---

_Comment by @zanieb on 2025-05-12 19:37_

Yeah, we don't support that yet. Installations from remote source trees (like Git) still currently go through the fully "standardized" package build and install path — which does not allow for tool-specific metadata to have an effect. In that context, `uv run --locked` refers to your local lockfile. If you want to pin the dependencies of `oasis-cli`, then add it as a dependency to your project instead of using `--with`.

---

_Comment by @dimaqq on 2025-05-12 19:39_


My 2c: Uv run runs a dependency. The dependencies’ lock files are not used, only the dependency specifications.

For now, you can specify click in your pyproject.toml 

---

_Comment by @Ark-kun on 2025-05-12 19:45_

I guess I was just using `uv run --with` instead of `uvx --from`.
Anyways,
```
uvx --from git+https://github.com/Cloud-Pipelines/oasis-cli.git@5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154 python -c "import click; print(click.__version__)"
    Updated https://github.com/Cloud-Pipelines/oasis-cli.git (5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154)
8.2.0
```

>If you want to pin the dependencies of oasis-cli, then add it as a dependency to your project instead of using --with.

> For now, you can specify click in your pyproject.toml

unfortunately, there is no project. I'm basically doing `uvx`.

>Yeah, we don't support that yet. Installations from remote source trees (like Git) still currently go through the fully "standardized" package build and install path — which does not allow for tool-specific metadata to have an effect.

I see. Just brainstorming. Is there any way I can let users run my app with versions pinned to prevent breakages like the ongoing clock incident?

https://github.com/pallets/click/issues/2908
https://github.com/fastapi/typer/discussions/1215
https://github.com/modelcontextprotocol/python-sdk/issues/688
https://github.com/ai-dynamo/dynamo/issues/1039


---

_Comment by @zanieb on 2025-05-12 19:48_

Moving to https://github.com/astral-sh/uv/issues/13414

---
