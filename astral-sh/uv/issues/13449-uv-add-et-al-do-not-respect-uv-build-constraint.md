```yaml
number: 13449
title: "`uv add` et al do not respect UV_BUILD_CONSTRAINT"
type: issue
state: closed
author: tmct
labels:
  - bug
assignees: []
created_at: 2025-05-14T11:00:39Z
updated_at: 2025-06-28T13:26:31Z
url: https://github.com/astral-sh/uv/issues/13449
synced_at: 2026-01-12T16:01:29Z
```

# `uv add` et al do not respect UV_BUILD_CONSTRAINT

---

_@tmct_

### Summary

Hi,

In the last week we encountered an issue with a not-actively-maintained package that we have a dependency on. [Here](https://github.com/escherba/python-metrohash/issues/29) are the details if you're interested. We became unable to install the dependency, even on builds installing an identical version as before, because: no py312+ wheels have ever been published for the dep, its build from source was incompatible with the newly-released Cython 3.1.0, and the latter is automatically brought in for new isolated builds when one installs packages.

Fortunately, we were using uv pip install, so we could use UV_BUILD_CONSTRAINT to avoid the new Cython until we remove that package:

constraints.txt:
```
Cython < 3.1.0
```

```
UV_BUILD_CONSTRAINT=constraints.txt uv pip install my-package-that-depends-on-metrohash
```

This worked successfully for uv pip install, including our usages of uv pip install through Hatch to build and test packages.

Now - a user has come to me with the same problem with `uv sync` - it seems like uv add, sync et all do not respect the UV_BUILD_CONSTRAINT parameter. IT picks latest Cython and you still see the build error.

I am fairly sure that these uv commands _should_ respect UV_BUILD_CONSTRAINT: otherwise there is no realmechanism to exert control over the isolated builds that occur for one's dependencies that lack binary distributions.

(The user found an excellent workaround - uv pip install the troublesome package _in any other environment_, using UV_BUILD_CONSTRAINT. This pops the built package into uv's cache, and it starts working everywhere else! But clearly this is not a fully satisfying solution.)

Please could this behaviour be added to all relevant uv commands?

Thanks,
Tom

### Platform

Ubuntu 24.04

### Version

latest (0.7.x)

### Python version

Python 3.12

---

_Label `bug` added by @tmct on 2025-05-14 11:00_

---

_Comment by @charliermarsh on 2025-05-14 18:39_

I believe those APIs expect to read build constraints from `[tool.uv.build-constraint-dependencies]` -- this is actually documented in the note here: https://docs.astral.sh/uv/reference/settings/#build-constraint-dependencies

---

_Closed by @charliermarsh on 2025-06-28 13:26_

---
