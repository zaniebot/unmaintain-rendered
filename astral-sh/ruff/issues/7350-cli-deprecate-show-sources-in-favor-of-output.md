```yaml
number: 7350
title: "CLI: Deprecate `--show-sources` in favor of `--output_format <concise|full>`"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2023-09-13T16:11:34Z
updated_at: 2024-02-01T19:35:06Z
url: https://github.com/astral-sh/ruff/issues/7350
synced_at: 2026-01-10T11:09:49Z
```

# CLI: Deprecate `--show-sources` in favor of `--output_format <concise|full>`

---

_Issue opened by @zanieb on 2023-09-13 16:11_

The `--show-sources` / `--no-show-sources` flags should be replaced with `--output_format full` / `--format concise` options. Ruff's default format should change to `full`. These formats would replace the `text` format, which should undergo a deprecation period.

The flags should also undergo a deprecation period during which
- They are hidden from the CLI help menu
- A warning is displayed if they are used
- They are ignored if `--format <..>` is provided

The "full" name is up for debate! Feel free to chime in with suggestions.

In #7349 the default behavior is changed to `--show-sources` / `--format full`.

---

_Label `cli` added by @zanieb on 2023-09-13 16:26_

---

_Assigned to @snowsignal by @snowsignal on 2024-01-29 16:31_

---

_Renamed from "CLI: Deprecate `--show-sources` in favor of `--format <concise|full>`" to "CLI: Deprecate `--show-sources` in favor of `--output_format <concise|full>`" by @snowsignal on 2024-01-29 17:11_

---

_Closed by @zanieb on 2024-02-01 19:35_

---
