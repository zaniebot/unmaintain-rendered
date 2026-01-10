```yaml
number: 8815
title: Add reference to correct issue for conflicting dependency groups 
type: pull_request
state: closed
author: ReinforcedKnowledge
labels: []
assignees: []
base: main
head: fix-deps-groups-issue-mention-in-dependencies-md
created_at: 2024-11-04T18:47:24Z
updated_at: 2024-11-04T19:57:03Z
url: https://github.com/astral-sh/uv/pull/8815
synced_at: 2026-01-10T12:00:00Z
```

# Add reference to correct issue for conflicting dependency groups 

---

_Pull request opened by @ReinforcedKnowledge on 2024-11-04 18:47_

In the [Dependency Groups section of the Dependencies documentation](https://docs.astral.sh/uv/concepts/dependencies/#dependency-groups), the issue mentioned concerns optional dependencies and not dependency groups.

I think it'd help newcomers know what is currently allowed (maybe conflicting optional dependencies will be allowed before conflicting dependency groups or vice-versa etc.). 

I have opened an issue for the conflicting dependency groups here https://github.com/astral-sh/uv/issues/8814

PS:
I haven't found another issue that mentions that but please do edit or even remove this PR / the issue if you have a better way.

---

_Closed by @zanieb on 2024-11-04 19:57_

---
