```yaml
number: 15357
title: Stabilize conflicting modules warning
type: issue
state: open
author: konstin
labels:
  - preview
  - tracking
assignees: []
created_at: 2025-08-18T16:36:55Z
updated_at: 2025-09-17T10:21:27Z
url: https://github.com/astral-sh/uv/issues/15357
synced_at: 2026-01-12T16:02:09Z
```

# Stabilize conflicting modules warning

---

_@konstin_

We added a check to warn when packages write to the same modules (https://github.com/astral-sh/uv/pull/13437), causing broken installations and uninstallations. Specifically, this can cause non-deterministically broken installation that are inscrutable to users. This was reverted to preview (https://github.com/astral-sh/uv/pull/15253).
 
* https://github.com/astral-sh/uv/issues/10708
* https://github.com/astral-sh/uv/issues/11806
* https://github.com/astral-sh/uv/issues/11659
* https://github.com/astral-sh/uv/issues/13435
* https://github.com/astral-sh/uv/issues/13550
* https://github.com/astral-sh/uv/issues/14030
* https://github.com/astral-sh/uv/issues/15238
* https://github.com/astral-sh/uv/issues/15813

Note: If you are affected and need to remove one of two conflicting packages from your resolution, you can use an [override](https://docs.astral.sh/uv/reference/settings/#override-dependencies) with:

```
foo-wrong; sys_platform == "never"
```

Problems that need to be addressed before stabilization. Most of them can be solved by ignoring exact duplicates.

- [ ] https://github.com/NVIDIA/NeMo-Agent-Toolkit/issues/611: This is a packaging bug and should be fixed upstream, neither of those packages should provide a `tests` module. Nevertheless, it's a useless warning to show.
- [ ] https://github.com/astral-sh/uv/issues/15239: Filed upstream as https://bugreports.qt.io/browse/PYSIDE-3161. The files are identical, so we can skip warning them. We'll still create a broken uninstallation when remove `pyside6` alone, but it's not broken on installation.
- [ ] Older `nvida-*-cu12` package have a redundant `__init__.py` (https://github.com/astral-sh/uv/issues/15239#issuecomment-3182788175)
- [ ] https://github.com/mlflow/mlflow/issues/17267

---

_Label `enhancement` added by @konstin on 2025-08-18 16:36_

---

_Label `preview` added by @konstin on 2025-08-18 16:36_

---

_Comment by @zanieb on 2025-08-18 16:40_

Thank you for writing up a tracking issue!

---

_Label `enhancement` removed by @konstin on 2025-08-18 16:42_

---

_Label `tracking` added by @konstin on 2025-08-18 16:42_

---
