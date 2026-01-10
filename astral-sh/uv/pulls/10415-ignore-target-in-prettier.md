```yaml
number: 10415
title: Ignore target in Prettier
type: pull_request
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
base: main
head: charlie/target
created_at: 2025-01-08T22:47:40Z
updated_at: 2025-01-08T22:52:02Z
url: https://github.com/astral-sh/uv/pull/10415
synced_at: 2026-01-10T11:44:47Z
```

# Ignore target in Prettier

---

_Pull request opened by @charliermarsh on 2025-01-08 22:47_

The [Prettier docs](https://prettier.io/docs/en/ignore.html) say that it should respect `.gitignore`, but in my experience it's... not? So `npx prettier --prose-wrap always --write "**/*.md"` keeps reformatting a bunch of files in `target`, slowing things down.

---

_Label `internal` added by @charliermarsh on 2025-01-08 22:47_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-08 22:49_

---

_Marked ready for review by @charliermarsh on 2025-01-08 22:49_

---

_Comment by @charliermarsh on 2025-01-08 22:52_

Oh, I just have an old version...

---

_Closed by @charliermarsh on 2025-01-08 22:52_

---
