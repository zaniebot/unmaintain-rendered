---
number: 7167
title: "Allow use of `tool.uv.sources` in published build metadata"
type: issue
state: open
author: FishAlchemist
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2024-09-07T12:38:24Z
updated_at: 2024-09-07T16:25:29Z
url: https://github.com/astral-sh/uv/issues/7167
synced_at: 2026-01-10T01:24:11Z
---

# Allow use of `tool.uv.sources` in published build metadata

---

_Issue opened by @FishAlchemist on 2024-09-07 12:38_

**uv version:** uv 0.4.7 (a178051e8 2024-09-07)
If a project depends on a package that doesn't exist on PyPI, the resulting wheel from uv build will also be unusable.
![image](https://github.com/user-attachments/assets/d8fa64c2-9d2c-4c56-872e-9630d5319bbc)
Due to this problem, I am unable to depend on private packages.

```toml
[project]
name = "temp"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "python-world",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
python-world = { git = "https://github.com/ceddlyburge/python_world" }
```
**Note:** I didn't verify the contents of the Git repository. I simply found a package name that doesn't exist on PyPI.
**Note:** I'm using URL dependencies for wheels, but since it's a private package, I've found another way to reproduce the same issue.
[dist.zip](https://github.com/user-attachments/files/16918152/dist.zip)


---

_Comment by @charliermarsh on 2024-09-07 12:41_

You can use a PEP 508 requirement instead of `tool.uv.sources` if you want them to appear in the distributed metadata. This is intentional.

---

_Comment by @FishAlchemist on 2024-09-07 12:47_

> You can use a PEP 508 requirement instead of `tool.uv.sources` if you want them to appear in the distributed metadata. This is intentional.

Does uv have a way to automatically convert to PEP 508 during ``uv build``? The current format is quite readable, so I don't want to write it as PEP 508 unless necessary.

---

_Comment by @charliermarsh on 2024-09-07 12:49_

Hmm, I don't think so. We could consider adding something like that. Right now we have `--no-sources`, which ignores the sources entirely during various operations. I think we could _only_ support this if we had our own build backend though.

---

_Comment by @FishAlchemist on 2024-09-07 13:01_

> Hmm, I don't think so. We could consider adding something like that. Right now we have `--no-sources`, which ignores the sources entirely during various operations. I think we could _only_ support this if we had our own build backend though.

If there are no sources, it should error, right? After all, nothing can be found.
![image](https://github.com/user-attachments/assets/17e957c7-94d9-466f-b9c4-fa6242dfbd0e)
However, ``uv build --no-sources`` can successfully produce .tar.gz and whl files.
**Note:** Running uv within the same project.

---

_Comment by @charliermarsh on 2024-09-07 13:08_

No, it would work fine, because those aren't _build_ requirements, so they don't get pulled in at all when building the wheel or source distribution.

---

_Comment by @FishAlchemist on 2024-09-07 13:21_

Have you considered stopping the build if there are dependencies originating from ``uv source``? Since uv isn't using UV's build backend now, its dependency behavior will differ from normal UV usage. 
Could this lead to confusion regarding package dependencies? For instance, if we expect to install packages from a specific source but end up installing from PyPI.
Issuing a warning is also an option; it's not necessary to halt the build. Perhaps users expect such an outcome.

---

_Renamed from "The result package dependencies of  ``uv build`` cannot be Git." to "Allow use of `tool.uv.sources` in published build metadata" by @charliermarsh on 2024-09-07 14:45_

---

_Label `enhancement` added by @charliermarsh on 2024-09-07 14:45_

---

_Label `needs-decision` added by @charliermarsh on 2024-09-07 14:45_

---

_Comment by @zanieb on 2024-09-07 15:14_

I don't think it's improper to define alternative package sources for development then publish using public versions. We probably should have some sort of safe guards or warnings here though — maybe an opt-in mechanism.

---

_Comment by @charliermarsh on 2024-09-07 15:50_

So that's roughly what we do today -- if you run `uv build` or `python -m build` or whatever, any information in `tool.uv.sources` is ignored (i.e., that Git reference would be absent from the published metadata). That _is_ by design, since we consider `project.dependencies` to be the declared project metadata, and `tool.uv.sources` to be for local development. The ask here is that `tool.uv.sources` find its way into the declared project metadata, so that `uv build` would retain that Git reference above by translating it to PEP 508.

---

_Comment by @zanieb on 2024-09-07 16:25_

Yep I think we're on the same page about the current behavior. I think a possible idea is that during publish we could require opt-in when local sources are defined so our behavior does not surprise people. I'm not a huge fan of that, but it's a possibility. My comment was roughly in response to:

> Have you considered stopping the build if there are dependencies originating from uv source?

An alternative is that we display a warning which requires opt-in to silence. I don't love that either, as I don't think the happy path should include warnings. I'm not certain of the best way for the CLI to teach that sources are not included in the built and published package.

> The ask here is that tool.uv.sources find its way into the declared project metadata, so that uv build would retain that Git reference above by translating it to PEP 508.

This might be kind of hard to teach as well, but it does seem like reasonable behavior to opt-in to. I think we could consider exposing this with a flag like `--convert-sources`. What source kinds could we support here?

---

_Referenced in [astral-sh/uv#7355](../../astral-sh/uv/issues/7355.md) on 2024-09-13 12:53_

---
