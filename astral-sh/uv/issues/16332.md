```yaml
number: 16332
title: option to not create .gitignore in dist/ folder when using uv build
type: issue
state: closed
author: CangyuanLi
labels:
  - enhancement
assignees: []
created_at: 2025-10-16T23:53:42Z
updated_at: 2025-10-28T12:25:32Z
url: https://github.com/astral-sh/uv/issues/16332
synced_at: 2026-01-10T03:23:54Z
```

# option to not create .gitignore in dist/ folder when using uv build

---

_Issue opened by @CangyuanLi on 2025-10-16 23:53_

### Summary

While in general it is best practice to not commit build artifacts, unfortunately I am in a situation where I do actually want (or rather have to) commit my dist/ folder. However, when running uv build, a .gitignore is automatically created in the dist/ folder--it would be great to have a flag to disable this behavior.

### Example

Maybe something like

```
uv build --no-gitignore
```

---

_Label `enhancement` added by @CangyuanLi on 2025-10-16 23:53_

---

_Assigned to @konstin by @konstin on 2025-10-20 09:24_

---

_Closed by @zanieb on 2025-10-28 12:25_

---
