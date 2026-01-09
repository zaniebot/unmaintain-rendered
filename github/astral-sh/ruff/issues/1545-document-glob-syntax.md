---
number: 1545
title: Document glob syntax
type: issue
state: closed
author: not-my-profile
labels:
  - documentation
assignees: []
created_at: 2023-01-02T05:07:28Z
updated_at: 2023-01-12T04:41:57Z
url: https://github.com/astral-sh/ruff/issues/1545
synced_at: 2026-01-07T13:12:14-06:00
---

# Document glob syntax

---

_Issue opened by @not-my-profile on 2023-01-02 05:07_

The glob syntax is described in the [globset documentation](https://docs.rs/globset/latest/globset/#syntax). However that documentation mentions some settings (`literal_separator` and `backslash_escape`) and users won't know if ruff has these enabled or disabled.

I think it would make sense to:

1. set the `value_name = "FILE_PATTERN"` clap option for all arguments that take file patterns, so that `--help` shows e.g.
   ```
    --exclude <FILE_PATTERN>
    --extend-exclude <FILE_PATTERN>
    ```
2. document the syntax for `<FILE_PATTERN>` via the `after_help` clap option of the `Cli` struct

---

_Label `core` added by @charliermarsh on 2023-01-02 18:40_

---

_Label `core` removed by @charliermarsh on 2023-01-02 18:41_

---

_Label `documentation` added by @charliermarsh on 2023-01-02 18:41_

---

_Comment by @marscher on 2023-01-03 11:54_

That seems like a good location to reference the upstream docs.

---

_Referenced in [astral-sh/ruff#1808](../../astral-sh/ruff/pulls/1808.md) on 2023-01-12 04:41_

---

_Closed by @charliermarsh on 2023-01-12 04:41_

---

_Referenced in [astral-sh/ruff#1956](../../astral-sh/ruff/issues/1956.md) on 2023-01-18 18:27_

---
