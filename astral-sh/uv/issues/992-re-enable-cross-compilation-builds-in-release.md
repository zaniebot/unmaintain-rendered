---
number: 992
title: Re-enable cross-compilation builds in release pipeline
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-01-19T02:01:06Z
updated_at: 2024-01-19T20:13:09Z
url: https://github.com/astral-sh/uv/issues/992
synced_at: 2026-01-10T01:23:05Z
---

# Re-enable cross-compilation builds in release pipeline

---

_Issue opened by @charliermarsh on 2024-01-19 02:01_

Right now, the `linux-cross` builds are all failing, so they're disabled: https://github.com/astral-sh/puffin/actions/runs/7575202511/job/20631293066.

It seems related to this: https://github.com/briansmith/ring/issues/1728#issuecomment-1758180655. But changing the manylinux didn't resolve it for me.

---

_Label `bug` added by @charliermarsh on 2024-01-19 02:01_

---

_Comment by @charliermarsh on 2024-01-19 02:01_

\cc @konstin who will be the most knowledgeable.

---

_Referenced in [astral-sh/uv#1006](../../astral-sh/uv/pulls/1006.md) on 2024-01-19 10:33_

---

_Comment by @konstin on 2024-01-19 10:34_

Should be fixed by https://github.com/astral-sh/puffin/pull/1006.

For me locally,
```
cargo zigbuild --target aarch64-unknown-linux-gnu -p puffin
```
works nicely.

---

_Referenced in [astral-sh/uv#1012](../../astral-sh/uv/pulls/1012.md) on 2024-01-19 20:13_

---

_Closed by @charliermarsh on 2024-01-19 20:13_

---
