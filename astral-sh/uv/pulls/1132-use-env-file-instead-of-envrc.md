```yaml
number: 1132
title: "Use `.env` file instead of `.envrc`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/env
created_at: 2024-01-26T19:37:35Z
updated_at: 2024-01-26T20:00:07Z
url: https://github.com/astral-sh/uv/pull/1132
synced_at: 2026-01-12T16:04:27Z
```

# Use `.env` file instead of `.envrc`

---

_@zanieb_

#1131 shows that `direnv` installation is _most_ of the CI overhead introduced by #1105.

Instead of using `direnv`, let's just use a simple `.env` file that can be loaded with `source` or [`direnv`'s `dotenv` directive](https://direnv.net/man/direnv-stdlib.1.html#codedotenv-ltdotenvpathgtcode).

---

_Review requested from @charliermarsh by @zanieb on 2024-01-26 19:39_

---

_Review requested from @BurntSushi by @zanieb on 2024-01-26 19:39_

---

_Label `internal` added by @zanieb on 2024-01-26 19:42_

---

_@BurntSushi approved on 2024-01-26 19:59_

Wow.

---

_Merged by @zanieb on 2024-01-26 20:00_

---

_Closed by @zanieb on 2024-01-26 20:00_

---

_Branch deleted on 2024-01-26 20:00_

---
