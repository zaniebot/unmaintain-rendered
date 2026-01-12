```yaml
number: 14560
title: "`uv sync`: add `--only-extra` option"
type: issue
state: closed
author: zzJinux
labels:
  - question
assignees: []
created_at: 2025-07-11T09:51:25Z
updated_at: 2025-07-12T16:45:39Z
url: https://github.com/astral-sh/uv/issues/14560
synced_at: 2026-01-12T16:01:51Z
```

# `uv sync`: add `--only-extra` option

---

_@zzJinux_

### Summary

I found that, unlike `--only-dev` or `--only-group`, `--only-extra` is missing while trying to do such an action.

### Example

_No response_

---

_Label `enhancement` added by @zzJinux on 2025-07-11 09:51_

---

_Comment by @zanieb on 2025-07-11 12:50_

What would the semantics be? Unlike dependency groups, extras are intended to be installed with the project dependencies.

---

_Comment by @zzJinux on 2025-07-11 15:09_

What is the use case of `--only-dev`? I think there is only 'for whatever reason one may want to install dev-only', and the same goes for extras. That's all.

---

_Comment by @zanieb on 2025-07-11 16:14_

Dependency groups are _designed_ to be used without the project and its dependencies. The PEP has examples and discussion https://peps.python.org/pep-0735/#dependency-groups-are-not-hidden-extras

In contrast, extras do not work that way.

---

_Label `enhancement` removed by @zanieb on 2025-07-11 16:14_

---

_Label `question` added by @zanieb on 2025-07-11 16:14_

---

_Closed by @zzJinux on 2025-07-12 16:45_

---
