```yaml
number: 17626
title: Drop PPC64 (big endian) builds
type: pull_request
state: open
author: konstin
labels:
  - breaking
  - "build:skip-release"
assignees: []
base: main
head: konsti/drop-ppc64
created_at: 2026-01-20T15:08:46Z
updated_at: 2026-01-21T11:48:07Z
url: https://github.com/astral-sh/uv/pull/17626
synced_at: 2026-01-21T11:58:56Z
```

# Drop PPC64 (big endian) builds

---

_@konstin_

PPC64 (big endian) seems dead, and it's only supported on one exact manylinux version (https://github.com/pypa/auditwheel/issues/669), so we should drop it.

This is a low risk change, but we can wait to 0.10 also.


---

_Added to milestone `v0.10.0` by @konstin on 2026-01-20 15:08_

---

_Label `breaking` added by @konstin on 2026-01-20 15:08_

---

_Comment by @zanieb on 2026-01-20 15:11_

Do we need to update https://docs.astral.sh/uv/reference/policies/platforms/

---

_Label `build:skip-release` added by @zanieb on 2026-01-20 15:15_

---

_Comment by @tiran on 2026-01-21 11:42_

It's not dead. All major Linux distributions (Alma, CentOS Stream, Debian, Fedora, RHEL, Ubuntu) have full support for ppc64le. PEP 11 lists Power as a Tier 3 platform, too.

---

_Comment by @konstin on 2026-01-21 11:44_

This is specifically about PPC64 (big endian), the PPC64LE support you mentioned remains as is. The terminology of wheel tags is confusing here, as PPC64 (no extension) refers to big endian. I've adjusted the title.

---

_Renamed from "Drop PPC64 builds" to "Drop PPC64 (big endian) builds" by @konstin on 2026-01-21 11:44_

---

_Comment by @konstin on 2026-01-21 11:46_

Could you also have a look at https://discuss.python.org/t/what-platforms-should-wheels-be-provided-for-by-default/105822? The perspective of linux distros on this would be valuable, as it's the basis for the support we have for Linux architectures in Python.

---
