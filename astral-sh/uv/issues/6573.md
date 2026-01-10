```yaml
number: 6573
title: "Allow `uv sync --no-install-project` without relying on `pyproject.toml`"
type: issue
state: open
author: albertferras-vrf
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-08-24T07:45:59Z
updated_at: 2024-09-14T22:16:19Z
url: https://github.com/astral-sh/uv/issues/6573
synced_at: 2026-01-10T04:45:09Z
```

# Allow `uv sync --no-install-project` without relying on `pyproject.toml`

---

_Issue opened by @albertferras-vrf on 2024-08-24 07:45_

I'm trying the new feature in today's `0.3.3` release: `uv sync --no-install-project` but I could not make it work since it keeps failing because `pyproject.toml` file is not found

The full `Dockerfile` example provided in the documentation here https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers does not work and raises this error too

---

_Comment by @charliermarsh on 2024-08-24 11:30_

Apologies, does it work if you copy over the `pyproject.toml`? It might be relying on _discovering_ it but not actually reading from it. Will fix.

---

_Label `bug` added by @charliermarsh on 2024-08-24 11:30_

---

_Comment by @albertferras-vrf on 2024-08-24 12:37_

Yes, it works after copying `pyproject.toml`.
However, it seems like it tries to read it because after creating an empty one (`touch pyproject.toml`) then `uv sync` fails with `0.092 error: No `project` table found in: `/opt/app/pyproject.toml``

---

_Comment by @charliermarsh on 2024-08-24 12:41_

Yes it reads it to discover the project name, so it can pick out the right dependency tree in the lockfile.

---

_Comment by @charliermarsh on 2024-08-24 12:42_

But it does not require the project _contents_ which is the critical piece that would ruin caching.

---

_Comment by @charliermarsh on 2024-08-24 12:42_

(The docs are incorrect though.)

---

_Comment by @albertferras-vrf on 2024-08-24 12:51_

What happens if the project has multiple packages (using workspaces), do I need to copy all other pyproject.toml files too?

---

_Comment by @charliermarsh on 2024-08-24 13:05_

Let me test it before I try to answer confidently.

---

_Comment by @charliermarsh on 2024-08-24 13:34_

Yes, but I think we can fix that part.

---

_Comment by @charliermarsh on 2024-08-24 14:01_

I updated the documentation (and added test coverage) for now. We'll follow-up with some other changes.

---

_Comment by @albertferras-vrf on 2024-08-24 17:07_

thank you @charliermarsh !

One suggestion: Maybe we can consider storing the root (?) project name in uv.lock so that pyproject.toml is not needed. 
 it would make it slightly easier to have a working Dockerfile

---

_Comment by @charliermarsh on 2024-08-24 17:46_

I think we probably can. Just trying to decide if we _should_. It creates some weirdness if we don't use the `pyproject.toml` to discover the root directory, etc.

---

_Label `bug` removed by @charliermarsh on 2024-08-25 02:27_

---

_Label `enhancement` added by @charliermarsh on 2024-08-25 02:27_

---

_Renamed from "Error when running `uv sync --no-install-project`: pyproject.toml file not found" to "Allow `uv sync --no-install-project` with relying on `pyproject.toml`" by @charliermarsh on 2024-08-25 02:27_

---

_Comment by @jaklan on 2024-08-25 11:02_

@charliermarsh I don't know low-level differences between `uv` and `pdm`, just to mention the latter is able to update venv without `pyproject.toml` with the below command:
> pdm sync --no-self

Btw, what is a difference between these two flags?
```
      --locked                                   Assert that the `uv.lock` will remain unchanged
      --frozen                                   Sync without updating the `uv.lock` file
```

---

_Comment by @zanieb on 2024-08-26 16:40_

@jaklan you can see the extended documentation at https://docs.astral.sh/uv/reference/cli/#uv-sync or `uv help sync`.

---

_Comment by @jaklan on 2024-08-26 17:22_

@zanieb thanks, I haven't noticed docs contain extended version of options' help messages (I used `uv sync --help`)

---

_Comment by @zanieb on 2024-08-26 17:50_

The end of `uv sync --help` suggests the longer option and we cover this at https://docs.astral.sh/uv/getting-started/help/ — let me know if there's anything more we can do to emphasize that it's available though!

---

_Comment by @jaklan on 2024-08-26 18:00_

@zanieb ach lol, haven't noticed the footer at all 😄 Maybe you can make it bold:

> Use **`uv help sync`** for more details.

or put at the top:

> ❯ uv sync --help
> Update the project's environment
>
> Note: Use **`uv help sync`** for more detailed help.
>
> Usage: uv sync [OPTIONS]
> ...

but generally - it was simply my bad, I was just quickly testing `uv` and haven't got familiar with the CLI and docs yet

---

_Renamed from "Allow `uv sync --no-install-project` with relying on `pyproject.toml`" to "Allow `uv sync --no-install-project` without relying on `pyproject.toml`" by @konstin on 2024-08-28 09:58_

---

_Comment by @charliermarsh on 2024-08-28 15:06_

The biggest hangup to doing this is that you lose any configuration specified in the `pyproject.toml`, which feels like a footgun.

---

_Label `wish` added by @charliermarsh on 2024-09-14 22:16_

---
