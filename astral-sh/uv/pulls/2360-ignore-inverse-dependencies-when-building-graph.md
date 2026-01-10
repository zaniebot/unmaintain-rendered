```yaml
number: 2360
title: Ignore inverse dependencies when building graph
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/deps
created_at: 2024-03-11T17:18:21Z
updated_at: 2024-03-11T17:51:43Z
url: https://github.com/astral-sh/uv/pull/2360
synced_at: 2026-01-10T14:49:08Z
```

# Ignore inverse dependencies when building graph

---

_Pull request opened by @charliermarsh on 2024-03-11 17:18_

## Summary

It turns out that when we iterate over the incompatibilities of a package, PubGrub will _also_ show us the inverse dependencies. I suspect this was rare, because we have a version check at the bottom... So, this specifically required that you had some dependency that didn't end up appearing in the output resolution, but that matched the version constraints of the package you care about.

In this case, `langchain-community` depends on `langchain-core`. So we were seeing an incompatibility like:

```rust
FromDependencyOf(Package(PackageName("langchain-community"), None, None), Range { segments: [(Included("0.0.10"), Included("0.0.10")), (Included("0.0.11"), Included("0.0.11"))] }, Package(PackageName("langchain-core"), None, None), Range { segments: [(Included("0.1.8"), Excluded("0.2"))] })
```

Where we were iterating over `langchain-core`, and looking for version `0.0.11`... which happens to match `langchain-community`. (`langchain-community was omitted from the resolution; hence, it didn't exist in the map.)

Closes https://github.com/astral-sh/uv/issues/2358.


---

_Label `bug` added by @charliermarsh on 2024-03-11 17:18_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-11 17:19_

---

_Review requested from @konstin by @charliermarsh on 2024-03-11 17:19_

---

_Comment by @charliermarsh on 2024-03-11 17:19_

This is pretty hard to reproduce.

---

_Comment by @charliermarsh on 2024-03-11 17:24_

I can reproduce with just:

```
b2sdk==1.32.0
llama-index==0.4.40
```

But it's an expensive test.

---

_@konstin approved on 2024-03-11 17:38_

---

_Merged by @charliermarsh on 2024-03-11 17:51_

---

_Closed by @charliermarsh on 2024-03-11 17:51_

---

_Branch deleted on 2024-03-11 17:51_

---
