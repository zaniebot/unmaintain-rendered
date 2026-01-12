```yaml
number: 6827
title: "Don't build uv-dev by default"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/dont-uv-dev
created_at: 2024-08-29T18:27:59Z
updated_at: 2024-08-29T19:44:14Z
url: https://github.com/astral-sh/uv/pull/6827
synced_at: 2026-01-12T16:07:32Z
```

# Don't build uv-dev by default

---

_@konstin_

Most times we compile with `cargo build`, we don't actually need `uv-dev`. By making `uv-dev` dependent on a new `dev` feature, it doesn't get built by default anymore, but only when passing `--features dev`.

Hopefully a small improvement for compile times or at least system load.

---

_Label `internal` added by @konstin on 2024-08-29 18:27_

---

_Review requested from @BurntSushi by @konstin on 2024-08-29 18:27_

---

_Review requested from @charliermarsh by @konstin on 2024-08-29 18:27_

---

_Review requested from @zanieb by @konstin on 2024-08-29 18:27_

---

_@zanieb approved on 2024-08-29 18:30_

---

_@BurntSushi approved on 2024-08-29 18:35_

SGTM. I've gotten in the habit of passing `-p uv`. :P 

---

_Merged by @charliermarsh on 2024-08-29 19:44_

---

_Closed by @charliermarsh on 2024-08-29 19:44_

---

_Branch deleted on 2024-08-29 19:44_

---
