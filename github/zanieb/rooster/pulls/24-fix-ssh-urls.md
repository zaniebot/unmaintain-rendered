---
number: 24
title: Fix ssh urls
type: pull_request
state: closed
author: karpetrosyan
labels:
  - bug
assignees: []
base: main
head: fix-ssh-urls
created_at: 2024-03-23T16:59:33Z
updated_at: 2024-03-24T16:51:05Z
url: https://github.com/zanieb/rooster/pull/24
synced_at: 2026-01-07T13:12:14-06:00
---

# Fix ssh urls

---

_Pull request opened by @karpetrosyan on 2024-03-23 16:59_

When the remote url starts with git@..., rooster fails to find the repo with the following message:

```
RuntimeError: GraphQL server responded with error: [{'type': 'NOT_FOUND', 'path': ['repository'], 'locations': [{'line': 5, 'column': 5}], 'message': "Could not resolve to a Repository with the name 'karpetrosyan/hishel.git'."}]
```


---

_@zanieb approved on 2024-03-24 16:33_

---

_Merged by @zanieb on 2024-03-24 16:37_

---

_Closed by @zanieb on 2024-03-24 16:37_

---

_Label `bug` added by @zanieb on 2024-03-24 16:51_

---
