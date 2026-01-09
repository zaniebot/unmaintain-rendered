---
number: 11352
title: Pin/Lock Package to existing version in venv
type: issue
state: closed
author: ChromoX
labels:
  - question
assignees: []
created_at: 2025-02-09T10:50:46Z
updated_at: 2025-02-11T03:32:10Z
url: https://github.com/astral-sh/uv/issues/11352
synced_at: 2026-01-07T13:12:18-06:00
---

# Pin/Lock Package to existing version in venv

---

_Issue opened by @ChromoX on 2025-02-09 10:50_

### Question

I create a `venv` with `uv` and I custom compile a few packages(numpy/scipy/etc...). In my `pyproject.toml` I have the versions for those packages pinned to the version I custom compile.

When I run `uv sync` it still "upgrades" to the same version using some downloaded version of the package.

Is there any way to prevent this from happening? How can I pin or lock a package from being touched?

### Platform

Debian 12

### Version

uv 0.5.20

---

_Label `question` added by @ChromoX on 2025-02-09 10:50_

---

_Comment by @FishAlchemist on 2025-02-09 17:17_

I'm guessing your pyproject.toml isn't pinned to your own source? If you can, could you provide your pyproject.toml and more info?

``[tool.uv.sources]``
https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-sources

---

_Comment by @konstin on 2025-02-10 09:31_

[`tool.uv.constraint-dependencies`](https://docs.astral.sh/uv/reference/settings/#constraint-dependencies) are an option.

---

_Comment by @ChromoX on 2025-02-11 03:32_

I've been changing a lot of stuff in my `pyproject.toml`, but the `constraint-dependencies` seem to do the trick. I also added the `package = false` which I feel like may helped even before the `constraint-dependencies`.

Thanks to you both!

---

_Closed by @ChromoX on 2025-02-11 03:32_

---
