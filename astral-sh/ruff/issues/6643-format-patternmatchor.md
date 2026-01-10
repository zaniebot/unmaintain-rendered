```yaml
number: 6643
title: "Format `PatternMatchOr`"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-17T09:51:32Z
updated_at: 2023-08-28T08:09:19Z
url: https://github.com/astral-sh/ruff/issues/6643
synced_at: 2026-01-10T11:09:48Z
```

# Format `PatternMatchOr`

---

_Issue opened by @MichaReiser on 2023-08-17 09:51_

Implement formatting of `a | b` patterns

```python
match x:
    case [x] | (y):
        ...
```

---

_Label `formatter` added by @MichaReiser on 2023-08-17 09:52_

---

_Label `help wanted` added by @MichaReiser on 2023-08-17 09:52_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:14_

---

_Comment by @LaBatata101 on 2023-08-25 20:48_

I can take this

---

_Comment by @charliermarsh on 2023-08-25 21:16_

Nice, thanks @LaBatata101

---

_Assigned to @LaBatata101 by @charliermarsh on 2023-08-25 21:17_

---

_Comment by @charliermarsh on 2023-08-25 21:18_

You can maybe look at `expression/expr_bool_op.rs` as a reference, or the `BinOpLayout::Default` branch in `expression/expr_bin_op.rs` although that may have handling and special cases that we don't need here -- I'm not sure yet.

---

_Closed by @MichaReiser on 2023-08-28 08:09_

---
