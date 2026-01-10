---
number: 2520
title: "`uv pip check` gives a different result to `pip check`"
type: issue
state: closed
author: strickvl
labels: []
assignees: []
created_at: 2024-03-18T20:02:14Z
updated_at: 2024-03-19T15:06:58Z
url: https://github.com/astral-sh/uv/issues/2520
synced_at: 2026-01-10T01:23:18Z
---

# `uv pip check` gives a different result to `pip check`

---

_Issue opened by @strickvl on 2024-03-18 20:02_

Added `uv pip check` to replace `pip check` in our CI (i.e. [here](https://github.com/zenml-io/zenml/pull/2520/files#diff-674e2904ada0d1873475b631dd3b95344eb79d8dedf60c0e341c4bbf831a4049R94). The CI passed previously without issue when running `pip check`, but when switching to `uv` I get [the following error](https://github.com/zenml-io/zenml/actions/runs/8332802769/job/22802845832?pr=2520#step:7:1):

```
Run uv pip check
Checked 81 packages in 0.71ms
Found 1 incompatibility
The package `pip` has multiple installed distributions:
  - /opt/hostedtoolcache/Python/3.8.18/x64/lib/python3.8/site-packages/pip-23.0.1.dist-info
  - /opt/hostedtoolcache/Python/3.8.18/x64/lib/python3.8/site-packages/pip-24.0.dist-info
Error: Process completed with exit code 1.
```

Is there any guidance / docs on how `uv`'s `pip check` differs from normal `pip check`?

---

_Comment by @charliermarsh on 2024-03-18 20:07_

The things we check for:

- Package has no `METADATA`, or the `METADATA` can't be parsed.
- Package has a `Requires-Python` that doesn't match the current interpreter.
- Package has a dependency on a package that isn't installed.
- Package has a dependency on a package; it's installed, but not at a compatible version.
- Multiple versions of a package are installed in the virtual environment.

---

_Comment by @charliermarsh on 2024-03-18 20:08_

I'm not sure what `pip check` looks for exactly. Seems like it doesn't validate the last point.

---

_Referenced in [zenml-io/zenml#2520](../../zenml-io/zenml/pulls/2520.md) on 2024-03-19 08:34_

---

_Comment by @strickvl on 2024-03-19 08:56_

Ok so the trick there was just to use virtual environments instead of installing into the raw global system... Thanks for this explanation. I'm sure what you wrote above would be useful for an eventual docs page for `uv`, along with guidance around using uv with github actions.

---

_Closed by @strickvl on 2024-03-19 08:56_

---

_Comment by @charliermarsh on 2024-03-19 15:06_

I'll add these to the docs!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-19 15:06_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-03-19 15:06_

---

_Referenced in [astral-sh/uv#2540](../../astral-sh/uv/issues/2540.md) on 2024-03-19 15:07_

---
