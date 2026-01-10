```yaml
number: 14732
title: "docs: small improve"
type: pull_request
state: closed
author: chirizxc
labels: []
assignees: []
base: main
head: small-docs-improve
created_at: 2025-07-18T16:29:08Z
updated_at: 2025-07-18T17:42:24Z
url: https://github.com/astral-sh/uv/pull/14732
synced_at: 2026-01-10T06:53:02Z
```

# docs: small improve

---

_Pull request opened by @chirizxc on 2025-07-18 16:29_

## Summary

<img width="1173" height="529" alt="изображение" src="https://github.com/user-attachments/assets/e1bb44d4-053c-4747-a859-5d034704cea4" />
Now if we copy this segment, this is the name pyproject.toml

## Test Plan

`uvx --with-requirements docs/requirements.txt -- mkdocs serve -f mkdocs.public.yml`  


---

_Comment by @chirizxc on 2025-07-18 16:33_

Although I don't think copying this section with tree makes much sense

---

_Comment by @zanieb on 2025-07-18 17:42_

Yeah sorry I don't think this change makes much sense to me. We want that to be a part of the file tree, not a title.

---

_Closed by @zanieb on 2025-07-18 17:42_

---
