---
number: 11743
title: How to specify that a distribution package is for manylinux, not musllinux?
type: issue
state: open
author: sanmai-NL
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-02-24T11:36:48Z
updated_at: 2025-02-24T13:40:38Z
url: https://github.com/astral-sh/uv/issues/11743
synced_at: 2026-01-07T13:12:18-06:00
---

# How to specify that a distribution package is for manylinux, not musllinux?

---

_Issue opened by @sanmai-NL on 2025-02-24 11:36_

### Question

I want to specify that my distribution package doesn't support `musllinux` (platform tags). Because some of my dependencies don't, and I want my fellow developers to get direct fault about this rather than a broken dependency resolution.

I didn't find a way to leverage `tool.uv.environments` for this, since the environment marker language appears unable to express constraint (?). I take as pre-condition that the required information is in the interpreter's sysconfig and that PyPA platform tags are the standard to be used.

### Platform

_No response_

### Version

uv 0.6.2

---

_Label `question` added by @sanmai-NL on 2025-02-24 11:36_

---

_Comment by @konstin on 2025-02-24 11:40_

Currently, `tool.uv.environments` can't capture manylinux vs. musllinux. The libc dependency can only be expressed in wheel filenames, but the PEP 508 markers don't have an equivalent that would allow defining a constraint here.

---

_Comment by @sanmai-NL on 2025-02-24 11:43_

@konstin 
Clear, thank you.

Is this related? https://github.com/astral-sh/uv/issues/7816

Is there some other mechanism/work-around to achieve the desired outcome?

---

_Label `question` removed by @konstin on 2025-02-24 13:25_

---

_Label `enhancement` added by @konstin on 2025-02-24 13:25_

---

_Label `needs-design` added by @konstin on 2025-02-24 13:25_

---

_Assigned to @konstin by @konstin on 2025-02-24 13:25_

---

_Unassigned @konstin by @konstin on 2025-02-24 13:25_

---

_Comment by @konstin on 2025-02-24 13:30_

Unfortunately, I don't think there's a mechanism for that yet, we'd need to introduce some new syntax, probably a new field, that can capture wheel filename tags, too. This needs design for the configuration format and how it integrates with the rest of the configuration.

---

_Comment by @sanmai-NL on 2025-02-24 13:40_

This reflects the host/targets distinction from the cross-compilation domain. Host properties can be used for uv configuration (environment markers), target properties like PyPA platform tags can't be yet.

---
