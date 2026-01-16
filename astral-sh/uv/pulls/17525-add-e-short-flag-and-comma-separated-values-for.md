```yaml
number: 17525
title: "Add `-E` short flag and comma-separated values for `--extra` option"
type: pull_request
state: open
author: terror
labels: []
assignees: []
base: main
head: extra-option-shorthand
created_at: 2026-01-16T15:42:40Z
updated_at: 2026-01-16T15:42:40Z
url: https://github.com/astral-sh/uv/pull/17525
synced_at: 2026-01-16T15:58:35Z
```

# Add `-E` short flag and comma-separated values for `--extra` option

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/17511

This diff adds `-E` as a short form for `--extra` and enables comma-separated values, allowing `uv sync -E foo,bar` as an alternative to `uv sync --extra foo --extra bar`. This is aligned with the behavior of Poetry's `-E` flag and improves ergonomics for projects with multiple extras.

---
