---
number: 16650
title: "allow configuring `prerelease` per-package"
type: issue
state: closed
author: DetachHead
labels:
  - enhancement
assignees: []
created_at: 2025-11-09T06:20:42Z
updated_at: 2025-11-10T09:44:41Z
url: https://github.com/astral-sh/uv/issues/16650
synced_at: 2026-01-07T13:12:19-06:00
---

# allow configuring `prerelease` per-package

---

_Issue opened by @DetachHead on 2025-11-09 06:20_

### Summary

in my project, i want certain dependencies to always allow prereleases, but not others. the [`prerelease`](https://docs.astral.sh/uv/reference/settings/#prerelease) setting only seems to allow configuring this for every dependency.

i know i can just run `uv sync --upgrade-package packagename --prerelease allow`, but it would be more convenient if i could just configure this in my `pyproject.toml` since it's always the same packages that i want to allow pre-releases for.

this way, `uv sync --upgrade` will always resolve prerelease versions for the packages where they are allowed, without me having to manually update each package individually.

(similar issue: #15780)

### Example

[my project](https://github.com/DetachHead/pytest-robotframework/) is a pytest plugin that relies on the internals of both pytest and robotframework, so i like to always test it against prerelease versions of these two packages whenever they are available to ensure that i always stay on top of any changes that may break my plugin. but i want all my other dependencies to stick with the release versions.

maybe the config could look something like this:

```toml
[tool.uv.prerelease]
robotframework = "allow"
pytest = "allow"
```

---

_Label `enhancement` added by @DetachHead on 2025-11-09 06:20_

---

_Renamed from "allow pre-releases per-package" to "allow configuring `prerelease` per-package" by @DetachHead on 2025-11-09 06:20_

---

_Comment by @zanieb on 2025-11-09 16:52_

You can also just include a pre-release segment in your requirement for those packages, e.g., `foo>=1.0a0`

---

_Comment by @DetachHead on 2025-11-10 09:44_

oh i didn't realize that makes it resolve any prerelease, not just prereleases of that specific version. thanks!

---

_Closed by @DetachHead on 2025-11-10 09:44_

---
