```yaml
number: 2388
title: Fail if maturin produces a PyPI incompatible wheel
type: pull_request
state: open
author: zanieb
labels:
  - ci
assignees: []
draft: true
base: main
head: zb/compat-validate
created_at: 2026-01-07T23:17:16Z
updated_at: 2026-01-08T19:17:16Z
url: https://github.com/astral-sh/ty/pull/2388
synced_at: 2026-01-12T15:54:28Z
```

# Fail if maturin produces a PyPI incompatible wheel

---

_@zanieb_

When we use automatic manylinux tag determination, we need to ensure that the wheel is uploadable to PyPI or the job will fail downstream and cause a partial release. 

---

_@zanieb reviewed on 2026-01-08 02:26_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:300 on 2026-01-08 02:26_

```suggestion
            # This target produces a `linux_armv6l` wheel which PyPI accepts,
            # but maturin's `--compatibility pypi` flag rejects.
```

---

_Marked ready for review by @zanieb on 2026-01-08 02:26_

---

_Label `ci` added by @MichaReiser on 2026-01-08 08:34_

---

_@MichaReiser approved on 2026-01-08 08:34_

Thank you, we should add the same to ruff's release pipeline

---

_Comment by @zanieb on 2026-01-08 13:06_

I'll let @konstin follow-up here before merging and porting elsewhere

---

_Converted to draft by @konstin on 2026-01-08 19:11_

---

_Assigned to @konstin by @konstin on 2026-01-08 19:17_

---
