```yaml
number: 3786
title: "feat: expose inner marker_env"
type: pull_request
state: closed
author: tdejager
labels: []
assignees: []
base: main
head: feat/expose-inner-marker-env
created_at: 2024-05-23T09:24:53Z
updated_at: 2024-05-23T10:39:14Z
url: https://github.com/astral-sh/uv/pull/3786
synced_at: 2026-01-12T16:05:51Z
```

# feat: expose inner marker_env

---

_@tdejager_

Made the inner public, we used to be able to construct a `MarkerEnvironment` programatically. However, this functionality has seemed to be removed. As there is now public `new` method as far as I can see, we cannot use the `with_*` methods.

We are using this in https://github.com/prefix-dev/pixi/blob/main/src/pypi_marker_env.rs to construct an environment from locked data.

I could also create a `new` method instead, WDYT?


---

_Comment by @konstin on 2024-05-23 09:37_

Would `MarkerEnvironment::try_from(MarkerEnvironmentBuilder { ... })` work for you?

---

_Comment by @tdejager on 2024-05-23 10:39_

Seems to work! Totally missed that sorry!

---

_Closed by @tdejager on 2024-05-23 10:39_

---
