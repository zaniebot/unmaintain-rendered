```yaml
number: 22761
title: 📝 Add --exit-non-zero-on-format to formatter exit codes section
type: pull_request
state: open
author: alejsdev
labels:
  - documentation
assignees: []
base: main
head: docs/ruff-format-flag
created_at: 2026-01-20T12:37:36Z
updated_at: 2026-01-20T12:46:28Z
url: https://github.com/astral-sh/ruff/pull/22761
synced_at: 2026-01-20T13:37:58Z
```

# 📝 Add --exit-non-zero-on-format to formatter exit codes section

---

_@alejsdev_

## Summary

The formatter's exit codes documentation at https://docs.astral.sh/ruff/formatter/#exit-codes doesn't mention the `--exit-non-zero-on-format` flag, even though it exists (it was added in https://github.com/astral-sh/ruff/pull/16009) and appears in ruff format --help, and the linter flag --exit-non-zero-on-fix is properly documented in the https://docs.astral.sh/ruff/linter/#exit-codes (which was used as a reference to add the proper section in the formatter, trying to keep the consistency)

---

_Label `documentation` added by @AlexWaygood on 2026-01-20 12:46_

---
