```yaml
number: 10806
title: "Support for PEP-compliant `dev` extra for optional dev dependencies"
type: issue
state: closed
author: aaronsteers
labels:
  - question
assignees: []
created_at: 2025-01-21T03:44:15Z
updated_at: 2025-01-21T14:06:51Z
url: https://github.com/astral-sh/uv/issues/10806
synced_at: 2026-01-12T16:00:21Z
```

# Support for PEP-compliant `dev` extra for optional dev dependencies

---

_@aaronsteers_

There seems to be a gap in [PEP 508](https://peps.python.org/pep-0508/) and [PEP 621](https://peps.python.org/pep-0621/) related to how dev dependencies should be declared.

All the tools, Poetry, uv, etc. each have their own proprietary declaration syntaxes. One idea gaining traction (pending a new PEP that adds a uniform declaration syntax) is to standardize around an optional "`dev`" extra.

Is it acceptable to declare dependencies using an extra simply called '`dev`'? Any great alternatives or suggestions? Thanks!

---

_Label `question` added by @konstin on 2025-01-21 08:14_

---

_Comment by @konstin on 2025-01-21 08:14_

[PEP 735](https://peps.python.org/pep-0735/) has recently added dependency groups, which can be used to specify development dependencies (https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-groups).

---

_Comment by @charliermarsh on 2025-01-21 14:06_

That's right -- dependency groups are designed for this!

---

_Closed by @charliermarsh on 2025-01-21 14:06_

---
