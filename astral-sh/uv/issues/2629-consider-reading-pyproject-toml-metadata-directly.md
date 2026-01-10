---
number: 2629
title: "Consider reading `pyproject.toml` metadata directly"
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2024-03-22T23:36:24Z
updated_at: 2024-03-27T14:34:19Z
url: https://github.com/astral-sh/uv/issues/2629
synced_at: 2026-01-10T01:23:20Z
---

# Consider reading `pyproject.toml` metadata directly

---

_Issue opened by @charliermarsh on 2024-03-22 23:36_

According to the spec:

> A build back-end MUST honour statically-specified metadata (which means the metadata did not list the key in dynamic).

In other words, if a field is defined in the `[project]` section, and not declared as dynamic, the spec suggests it must be used... verbatim? I think? So in theory we could read the metadata directly without executing any PEP 517 hooks.

This would not work for Hatch projects that rely on context formatting: https://github.com/pypa/hatch/issues/1331

---

_Comment by @charliermarsh on 2024-03-22 23:37_

Yes, I believe this should work per Paul Moore's comment here: https://github.com/pypa/pip/issues/11440#issuecomment-1774064882

---

_Label `performance` added by @charliermarsh on 2024-03-22 23:37_

---

_Comment by @charliermarsh on 2024-03-22 23:38_

This could be a nice optimization for resolution.

---

_Comment by @hauntsaninja on 2024-03-22 23:47_

I've had a similar PR lying around for pip-compile for a while: https://github.com/jazzband/pip-tools/pull/1964

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-23 02:42_

---

_Comment by @bluss on 2024-03-23 10:18_

Sorry if this is too tangential, but in my reading environment variables are not part of dependency specs (only requirements files) - thus `${PROJECT_ROOT}` usage could also be a problem here, which is another case?

Example https://pdm-project.org/latest/usage/dependency/#local-dependencies

---

_Comment by @charliermarsh on 2024-03-23 13:27_

My impression is that those usages are not spec compliant. But they do exist in the wild, so we'll have to detect them and skip the optimization in that case.

---

_Referenced in [astral-sh/uv#2676](../../astral-sh/uv/pulls/2676.md) on 2024-03-26 20:23_

---

_Closed by @charliermarsh on 2024-03-27 14:34_

---
