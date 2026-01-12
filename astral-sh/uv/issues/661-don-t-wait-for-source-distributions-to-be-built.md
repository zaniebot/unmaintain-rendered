```yaml
number: 661
title: "Don't wait for source distributions to be built"
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2023-12-15T12:55:59Z
updated_at: 2024-01-03T21:50:41Z
url: https://github.com/astral-sh/uv/issues/661
synced_at: 2026-01-12T15:58:24Z
```

# Don't wait for source distributions to be built

---

_@konstin_

With the `scripts/requirements/transformers-extras.in` example on python 3.10 on linux, the resolution is often waiting for a specific source distribution build to continue, even if we could start other builds in parallel already. We should investigate ways to do as much as possible before a source distribution build finishes.

```shell
cargo run --bin puffin --profile profiling -- pip-compile --no-cache scripts/requirements/transformers-extras.in
```

---

_Comment by @charliermarsh on 2023-12-15 13:04_

Do you mind including the command you’re running? It’s not clear if this is resolving or installing.

---

_Comment by @charliermarsh on 2024-01-03 21:35_

Do you think we can close this now?

---

_Closed by @konstin on 2024-01-03 21:50_

---
