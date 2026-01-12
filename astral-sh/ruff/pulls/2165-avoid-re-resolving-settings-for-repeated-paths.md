```yaml
number: 2165
title: Avoid re-resolving settings for repeated paths
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/skip
created_at: 2023-01-25T18:34:35Z
updated_at: 2023-01-25T18:38:35Z
url: https://github.com/astral-sh/ruff/pull/2165
synced_at: 2026-01-12T15:55:07Z
```

# Avoid re-resolving settings for repeated paths

---

_@charliermarsh_

After this change:

```shell
> time cargo run -- -n $(find ../django -type f -name '*.py')`
8.85s user 0.20s system 498% cpu 1.814 total
> time cargo run -- -n ../django
8.95s user 0.23s system 507% cpu 1.811 total
```

I also verified that we only hit the creation path once via some manual logging.

Closes #2154.

---

_Merged by @charliermarsh on 2023-01-25 18:38_

---

_Closed by @charliermarsh on 2023-01-25 18:38_

---

_Branch deleted on 2023-01-25 18:38_

---
