```yaml
number: 9697
title: "Special-case `==` Python version request ranges"
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/special-case-eq
created_at: 2024-12-06T22:54:55Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/9697
synced_at: 2026-01-10T11:10:34Z
```

# Special-case `==` Python version request ranges

---

_Pull request opened by @zanieb on 2024-12-06 22:54_

Related to https://github.com/astral-sh/uv/issues/9695

Doesn't solve the reported issue with `requires-python` because that doesn't go through the parser

https://github.com/astral-sh/uv/blob/6ed6fc108e95e742075cf1eddb09ec86c04f632e/crates/uv/src/commands/project/mod.rs#L518-L526

---
