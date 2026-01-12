```yaml
number: 14028
title: Ignore non-existing dependencies when using --extra flag
type: issue
state: open
author: elephaint
labels:
  - question
assignees: []
created_at: 2025-06-13T14:46:12Z
updated_at: 2025-06-27T13:07:27Z
url: https://github.com/astral-sh/uv/issues/14028
synced_at: 2026-01-12T16:01:42Z
```

# Ignore non-existing dependencies when using --extra flag

---

_@elephaint_

### Question

As of #11426, uv gives an error if an extra dependency is not available in pyproject.toml when running `uv sync --extra non-existing`. However, I'd like to have the old behavior back.

Is there any way of sort of pre-checking the `uv.lock` (or `pyproject.toml`)if an extra is available, and if not, ignore the `--extra` command, and if it is available, use the `--extra` command? 

### Platform

Linux 6.8.0-1027-azure x86_64 GNU/Linux

### Version

0.7.2

---

_Label `question` added by @elephaint on 2025-06-13 14:46_

---

_Comment by @konstin on 2025-06-13 16:24_

Can you emulate this with have an optional dependency or a dependency group that activates the extra? We allow non-existent extras in dependencies themselves, since we don't know to which version they point yet.

What is the reason the extra is only sometimes available?

---

_Comment by @elephaint on 2025-06-14 08:50_

Thanks!

How would that solution work? 

In this case, I'm exploring to build wheels sometimes including a set of extra optional dependencies (e.g. based on a customer id, so the customer id would be the name of the extra) but generally without. The extras are added on a need-to-have basis, so if an extra flag is given for a customer id that's not in pyproject.toml, it should just continue without raising an error, whereas if the id is in pyproject.toml, it includes the optional dependency.

The workflow is that the repo is first synced with optional extra dep based on a uv.lock file and then a wheel is build.

Maybe there's a better / easier alternative, and also maybe building the wheel completely ignores optional deps anyways, as it builds with refs to all deps, idk....

---

_Comment by @konstin on 2025-06-27 13:07_

The way extras usually work, they assume a static set of extras; they aren't suited for storing information such as customer in there.

---
