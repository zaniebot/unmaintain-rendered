```yaml
number: 7760
title: Incorporate knowledge of disjoint markers into simplification
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2024-09-28T18:07:23Z
updated_at: 2024-12-07T01:51:45Z
url: https://github.com/astral-sh/uv/issues/7760
synced_at: 2026-01-10T04:36:20Z
```

# Incorporate knowledge of disjoint markers into simplification

---

_Issue opened by @charliermarsh on 2024-09-28 18:07_

There are some markers that we "know" cannot be true at the same time, despite the fact that it's not encoded in the grammar or schema at all.

For example, this is never true: `platform_system == 'Linux' and sys_platform == 'darwin'`.

If we encode knowledge about the markers that we "know" are disjoint, we can omit more forks, wheels, etc.

---

_Label `performance` added by @charliermarsh on 2024-09-28 18:07_

---

_Comment by @charliermarsh on 2024-09-28 18:07_

\cc @ibraheemdev 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-20 18:20_

---

_Comment by @charliermarsh on 2024-11-20 18:55_

I have this working, but it does mean all expressions like `sys_platform == 'linux'` get expanded to `sys_platform == 'linux' and platform_system == 'Linux'"` in the lockfile.

---

_Comment by @charliermarsh on 2024-11-20 18:59_

I could just replace `sys_platform == 'linux'` with `platform_system == 'Linux'`, but then we wouldn't detect disjointness in cases like:

```
flask ; sys_platform == 'linux'
flask ; sys_platform == 'java1.8.0_51'
```


---

_Comment by @mitsuhiko on 2024-11-21 09:11_

> I have this working, but it does mean all expressions like `sys_platform == 'linux'` get expanded to `sys_platform == 'linux' and platform_system == 'Linux'"` in the lockfile.

Could you not keep the knowledge around what the original marker expression was and serialize that into the lockfile instead.  At read it should be rather trivial to expand it out when the lockfile is read?  Performance wise this will be slightly worse but you presumably there won't be that many cases where that is necessary.

---

_Closed by @charliermarsh on 2024-12-07 01:51_

---
