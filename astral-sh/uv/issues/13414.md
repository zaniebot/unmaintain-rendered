```yaml
number: 13414
title: "uvx --from git+https://....git@... ignores the lockfile"
type: issue
state: closed
author: Ark-kun
labels:
  - question
assignees: []
created_at: 2025-05-12T19:39:15Z
updated_at: 2025-10-13T18:00:52Z
url: https://github.com/astral-sh/uv/issues/13414
synced_at: 2026-01-10T03:23:54Z
```

# uvx --from git+https://....git@... ignores the lockfile

---

_Issue opened by @Ark-kun on 2025-05-12 19:39_

### Summary

I have a CLI project.
My customers run it via `uv run --refresh --frozen --with git+https://github.com/Cloud-Pipelines/oasis-cli.git@5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154 oasis` or `uvx --from git+https://github.com/Cloud-Pipelines/oasis-cli.git@5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154 oasis`



It has a `uv.lock` lockfile with `click==8.1.8`.
https://github.com/Cloud-Pipelines/oasis-cli/blob/5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154/uv.lock#L77
Despite that, uv run always installs `click==8.2.0` which is a broken release.


```
uvx --from git+https://github.com/Cloud-Pipelines/oasis-cli.git@5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154 python -c "import click; print(click.__version__)"
    Updated https://github.com/Cloud-Pipelines/oasis-cli.git (5625f4ef1b4c6de65bee913c3cc31bf1cb1c3154)
8.2.0
```

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

### Platform

macOS 15

### Version

0.7.0

### Python version

Python 3.11

---

_Label `bug` added by @Ark-kun on 2025-05-12 19:39_

---

_Comment by @zanieb on 2025-05-12 19:45_

Please don't post multiple times. See my other comment at https://github.com/astral-sh/uv/issues/13410#issuecomment-2873773771

Your customers will need to add that as a dependency (`uv add`) in order to have pinned dependencies. `uv.lock` files (and, more broadly, _any_ lockfile) are not a part of the package installation standard. The _local_ `uv.lock` file in the project where `uv run` is used is what is relevant here.


---

_Label `bug` removed by @zanieb on 2025-05-12 19:45_

---

_Label `question` added by @zanieb on 2025-05-12 19:45_

---

_Comment by @zanieb on 2025-05-12 19:47_

Replying to

>  Is there any way I can let users run my app with versions pinned to prevent breakages like the ongoing clock incident?

You'll need to pin all the versions in your `pyproject.toml` so it's reflected in the distribution metadata. This is tedious, but the only standards compliant method right now. `uv export | uv add -r -` will do it.

---

_Closed by @Ark-kun on 2025-05-12 19:57_

---

_Reopened by @Ark-kun on 2025-05-12 19:58_

---

_Comment by @Ark-kun on 2025-05-12 19:58_

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

_Renamed from "uv run --refresh --locked --with git+https://....git@... ignores the lockfile" to "uvx --from git+https://....git@... ignores the lockfile" by @Ark-kun on 2025-05-12 19:58_

---

_Comment by @Ark-kun on 2025-05-12 20:00_

>Please don't post multiple times.

Sorry about that. Thank you for you patience. I posted in that thread, then thought that maybe a separate issue would have been better.

---

_Comment by @zanieb on 2025-05-12 20:01_

It's alright! Just makes it hard to tell where to respond :)

---

_Comment by @zanieb on 2025-10-13 18:00_

Let's track this in https://github.com/astral-sh/uv/issues/5815

---

_Closed by @zanieb on 2025-10-13 18:00_

---
