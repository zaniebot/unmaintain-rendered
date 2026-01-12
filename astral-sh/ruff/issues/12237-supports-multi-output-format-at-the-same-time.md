```yaml
number: 12237
title: "Supports multi `--output-format` at the same time"
type: issue
state: closed
author: Zxilly
labels: []
assignees: []
created_at: 2024-07-08T06:29:01Z
updated_at: 2024-07-08T07:50:15Z
url: https://github.com/astral-sh/ruff/issues/12237
synced_at: 2026-01-12T15:54:51Z
```

# Supports multi `--output-format` at the same time

---

_@Zxilly_

For example, while running in the `GitHub Actions`, I want to use the `github` format to see details on the web ui, while also outputting the `SARIF` format to upload the result to the CodeQL.

---

_Comment by @MichaReiser on 2024-07-08 07:50_

Thanks for opening the issue. Yeah, this is a use case that ruff doesn't support well today. I wonder if we could just write a cheap conversion between the two formats. Anyway, this has come up before, see https://github.com/astral-sh/ruff/issues/2388. I'll close this issue in favor of the other one.

---

_Closed by @MichaReiser on 2024-07-08 07:50_

---
