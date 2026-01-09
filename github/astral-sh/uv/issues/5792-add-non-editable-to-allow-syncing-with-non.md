---
number: 5792
title: "Add `--non-editable` to allow syncing with non-editable workspace members"
type: issue
state: closed
author: charliermarsh
labels:
  - needs-decision
  - cli
assignees: []
created_at: 2024-08-05T12:53:55Z
updated_at: 2025-02-25T17:28:36Z
url: https://github.com/astral-sh/uv/issues/5792
synced_at: 2026-01-07T13:12:17-06:00
---

# Add `--non-editable` to allow syncing with non-editable workspace members

---

_Issue opened by @charliermarsh on 2024-08-05 12:53_

Do we want to support this?

---

_Label `needs-decision` added by @charliermarsh on 2024-08-05 12:54_

---

_Label `cli` added by @charliermarsh on 2024-08-05 12:54_

---

_Label `preview` added by @charliermarsh on 2024-08-05 12:54_

---

_Comment by @charliermarsh on 2024-08-05 13:02_

(It's very easy to do.)

---

_Comment by @charliermarsh on 2024-08-05 13:07_

If no one objects I can add it, not too hard.

---

_Comment by @zanieb on 2024-08-05 17:39_

I don't fully understand what this means. I worry how close the flag is to `--no-editable`.

---

_Comment by @charliermarsh on 2024-08-05 17:55_

It would mean: install the project and any workspace members as non-editable. So, build them to sdists, and install them into the environment, rather than adding a `.pth` file.

---

_Comment by @zanieb on 2024-08-05 17:57_

Hm. `--no-editable-project`? I'm not sure. Makes sense as something to support though. Seems easy to defer to after stabilization.

---

_Referenced in [astral-sh/uv#5958](../../astral-sh/uv/issues/5958.md) on 2024-08-09 14:20_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Comment by @eruvanos on 2024-08-23 13:19_

Hi,

I think I would need this flag, but maybe it is better to describe my use case, there might be another solution for it with uv:

I want to setup multiple AWS Lambda Functions, which all come with their own dependencies, some use flask, some don't, etc.

For local development it is convenient, to have one virtual environment, so I go with setting up a virtual workspace. I add multiple packages, one for each Lambda function.

I add one "shared" package, which I will use for code that is shared between all Lambdas.

I install the shared package into the other ones, which will create an entry like:
```
[tool.uv.sources]
shared = { workspace = true }
```

Next I create the Lambda artefact, by installing the package with `--target` 

```
cd package_a
uv pip install -c ../uv.lock --target dist .
```

`dist/` now contains everything I would have to ship, but the `shared` package is only referenced with `.pth` file, which will not work in the lambda environment.

From my point of view, `--non-editable` support in the `uv pip install` command could be used to fix this.







---

_Comment by @pawamoy on 2024-08-30 16:38_

I would use such a flag, yes, for the project itself (I don't use workspaces (yet)). Apologies if this not what this issue is about.

The initial use-case (a few years ago) was for [multi-stage Docker builds](https://github.com/pdm-project/pdm/issues/443): install every deps + project in a first stage, copy in a later stage. Editable installs of the project would not be working once the venv was copied in the final stage, since the sources were not present anymore.

The current use-case is to make CI more robust (to spot packaging issues early), with something like this:

```bash
if [ -n "${CI}" ]; then
  uv sync --no-editable-project
else
  uv sync
fi
```

---

_Referenced in [astral-sh/uv#6935](../../astral-sh/uv/issues/6935.md) on 2024-09-02 20:53_

---

_Referenced in [astral-sh/uv#7126](../../astral-sh/uv/issues/7126.md) on 2024-09-06 15:01_

---

_Referenced in [astral-sh/uv#7166](../../astral-sh/uv/pulls/7166.md) on 2024-09-07 19:01_

---

_Comment by @orf on 2024-09-11 14:33_

This would be very useful when building docker images with workspaces - you would be able to do something akin to:

```dockerfile
RUN --mount=bind,from=project,source=src/,target=src/ \
   --mount=type=bind,from=project,source=pyproject.toml,target=pyproject.toml \
   --mount=type=bind,from=project,source=uv.lock,target=uv.lock \
   uv sync --frozen
```

and avoid having to copy the `src/` directory into the image at all, and avoid multi-stage builds that in essence just try and emuate the above.

---

_Comment by @bachya on 2024-09-11 16:28_

@charliermarsh @zanieb Is there anything further the community could provide input on here? Noting that CM said this was relatively easy; this one feature is all that remains for us to move our large monorepo completely over to `uv`, so I'm excited to see it in action. I'd be happy to contribute if feedback/additional dialog would help drive a decision. No rush, of course—I'm sure many other priorities are going on.

(Unfortunately, my Rust is non-existent, and I'm not in a position to submit an actual PR. 🫠)

---

_Comment by @charliermarsh on 2024-09-11 17:28_

I think I can take this one on soon. It’s mostly about figuring the right names / UX / concepts.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-13 01:49_

---

_Referenced in [astral-sh/uv#7371](../../astral-sh/uv/pulls/7371.md) on 2024-09-13 18:06_

---

_Closed by @charliermarsh on 2024-09-17 14:50_

---
