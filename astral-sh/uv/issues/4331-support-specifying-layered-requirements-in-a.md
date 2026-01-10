---
number: 4331
title: Support specifying layered requirements in a single file
type: issue
state: closed
author: jab
labels: []
assignees: []
created_at: 2024-06-14T18:43:39Z
updated_at: 2024-06-19T00:45:03Z
url: https://github.com/astral-sh/uv/issues/4331
synced_at: 2026-01-10T01:23:37Z
---

# Support specifying layered requirements in a single file

---

_Issue opened by @jab on 2024-06-14 18:43_

I recently noticed #2679 -- awesome to see all the interest in that!

As long as we're thinking about enabling combining different requirements files for different platforms into a single, platform-independent lockfile, is there any interest in enabling you to specify additional [requirements layers](https://pip-tools.readthedocs.io/en/stable/#workflow-for-layered-requirements) (e.g. for running tests, building docs, etc., which should be [constrained by the base layer](https://pip-tools.readthedocs.io/en/stable/#workflow-for-layered-requirements:~:text=constrain%20the%20dev%20requirements%20to%20packages%20already%20selected), in a single file as well?

Many projects end up with [requirements file explosion](https://github.com/pallets/flask/tree/main/requirements) due to layering even more often than due to platform differences. Enabling users to define their requirement layers in a single file would be a wonderful simplification.

---

_Comment by @zanieb on 2024-06-14 19:33_

Hi! Have you seen [PEP 735](https://peps.python.org/pep-0735/)? I think that addresses what you're talking about. Right now we're only exposing a single group for development dependencies but we're watching that PEP as a possible standard we could use for specifying multiple groups. We're working on a lock file format (#3314) that is a single file. I'm not sure we'd extend locking of multiple groups to `requirements.txt` style files.

---

_Comment by @jab on 2024-06-15 02:35_

Thanks for the heads up about PEP 735, and great you're already thinking along these lines!

Any solution enabling projects to maintain a single pair of files -- one for all the dependency group inputs, and one with the result of resolving those inputs -- would be a win. Using a standardized and more structured format than requirements.txt to accomplish that would be icing on the cake.

---

_Comment by @zanieb on 2024-06-15 02:43_

Great to hear! Yeah we're planning on using the `pyproject.toml` for the declaration (with a `tool.uv.sources` extension for things like custom indexes per package that are not supported by the standard `project.dependencies` section) and a `uv.lock` with cross-platform resolutions of all of your extras and development dependencies. You can try some of it right now if you want, we'll be announcing more and writing new documentation soon though!

---

_Comment by @zanieb on 2024-06-19 00:45_

I think https://github.com/astral-sh/uv/issues/3347 can be used to track this request.

---

_Closed by @zanieb on 2024-06-19 00:45_

---

_Referenced in [astral-sh/uv#3560](../../astral-sh/uv/issues/3560.md) on 2024-07-30 18:06_

---
