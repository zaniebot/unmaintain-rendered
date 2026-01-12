```yaml
number: 10483
title: Does uv support backward compatibility for its cache?
type: issue
state: closed
author: cc636489
labels:
  - question
assignees: []
created_at: 2025-01-10T22:59:41Z
updated_at: 2025-01-10T23:26:17Z
url: https://github.com/astral-sh/uv/issues/10483
synced_at: 2026-01-12T16:00:14Z
```

# Does uv support backward compatibility for its cache?

---

_@cc636489_

Hi everyone,

I'm exploring the uv package manager and came across the **caching mechanism** it employs to improve performance and dependency management. While caching is undoubtedly useful, I have a few questions regarding its **backward compatibility**:

- If I upgrade uv to a newer version, will the cache created by the older version still be usable?
- If not usable, how will uv handle the old-dated cache? Does uv throw, or delete/update quietly?
- What would be the best practice to do with uv-cache when upgrading the uv?

Any insights, official documentation links, or experiences with managing the uv cache would be greatly appreciated!

Thanks in advance.

---

_Comment by @zanieb on 2025-01-10 23:25_

> If I upgrade uv to a newer version, will the cache created by the older version still be usable?

Generally, yes.

> If not usable, how will uv handle the old-dated cache? Does uv throw, or delete/update quietly?

The cache buckets are versioned. If we change the cache in a backwards incompatible way, a new bucket will be used. `uv cache clean` should be used to remove old cache buckets.

> What would be the best practice to do with uv-cache when upgrading the uv?

Nothing :)


---

_Comment by @zanieb on 2025-01-10 23:25_

See https://docs.astral.sh/uv/concepts/cache/#cache-versioning

---

_Label `question` added by @zanieb on 2025-01-10 23:26_

---

_Closed by @zanieb on 2025-01-10 23:26_

---
