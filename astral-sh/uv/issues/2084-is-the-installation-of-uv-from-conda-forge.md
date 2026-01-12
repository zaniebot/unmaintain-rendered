```yaml
number: 2084
title: "Is the installation of `uv` from `conda-forge` officially supported?"
type: issue
state: closed
author: Yura52
labels:
  - question
assignees: []
created_at: 2024-02-29T15:44:19Z
updated_at: 2024-02-29T23:51:11Z
url: https://github.com/astral-sh/uv/issues/2084
synced_at: 2026-01-12T15:58:35Z
```

# Is the installation of `uv` from `conda-forge` officially supported?

---

_@Yura52_

Hi! I noticed that I could install `uv` via `conda` from `conda-forge`, here is the feedstock: https://github.com/conda-forge/uv-feedstock

However, this installation method is not mentioned in README. Is it officially supported or is it a community effort?

---

_Comment by @zanieb on 2024-02-29 16:34_

That's a community effort, it's not owned by us. I wouldn't recommend managing `uv` with `conda` (as I wouldn't recommend managing it with `pip`) but it's fine.

@bollwyvl feel free to add me as a feedstock maintainer.

---

_Label `question` added by @zanieb on 2024-02-29 16:34_

---

_Comment by @Yura52 on 2024-02-29 16:37_

Thank you for the quick reply!

---

_Closed by @Yura52 on 2024-02-29 16:37_

---

_Comment by @notatallshaw-gts on 2024-02-29 18:54_

> I wouldn't recommend managing `uv` with `conda` (as I wouldn't recommend managing it with `pip`) but it's fine.

btw, is there some reason beyond uv has less control over how it's installed?

We have the use case of having multiple Python development environments, and we bootstrap the binaries (CPython, git, a few others) via conda and the install the Python packages via pip (qa and prod use the same pip process but don't need to bootstrap the binaries with conda).

I was looking at how feasible it is to replace pip with uv, and in this scenario it would be simplest to add uv to the binary requirements that conda bootstraps. But if there's some significant reason not to do this it would be good to know.

---

_Comment by @Yura52 on 2024-02-29 19:02_

My situation is technically different, but it is somewhat similar in its spirit, and I was considering using micromamba just to fetch uv. Perhaps I should not have closed the issue, so reopening it just in case, though feel free to close it, since my initial question is resolved. Would be nice to know if `conda-forge` becomes officially supported! Conda is a convenient way to bootstrap Python environments.

---

_Reopened by @Yura52 on 2024-02-29 19:02_

---

_Comment by @charliermarsh on 2024-02-29 23:49_

> But if there's some significant reason not to do this it would be good to know.

I don't think there's any _significant_ reason not to do it. The one reason I _can_ think of not to do it is that we're likely to make `uv` self-updateable in the future (e.g., `uv update` or similar), and `uv` installations that come from a package manager (instead of through the standalone binary) won't support that.

---

_Comment by @charliermarsh on 2024-02-29 23:50_

I'm fine to maintain it though as long as we're added as feedstock maintainers (\cc @bollwyvl) similar to our relationship to https://github.com/conda-forge/ruff-feedstock.

---

_Closed by @charliermarsh on 2024-02-29 23:50_

---

_Comment by @charliermarsh on 2024-02-29 23:51_

(So yes, I consider it fine and I'm happy to support it, but we would need to be added as feedstock maintainers before I would consider us "supporting it".)

---
