```yaml
number: 6476
title: "formatter: still fails to format certain f-string comments"
type: issue
state: closed
author: davidszotten
labels:
  - formatter
assignees: []
created_at: 2023-08-10T10:31:22Z
updated_at: 2023-08-16T07:11:22Z
url: https://github.com/astral-sh/ruff/issues/6476
synced_at: 2026-01-10T11:09:48Z
```

# formatter: still fails to format certain f-string comments

---

_Issue opened by @davidszotten on 2023-08-10 10:31_

scratch.py
```
result_f = (
    f'{1}'
    # comment
    ''
)
```

```
thread 'main' panicked at 'The following comments have not been formatted.
SourceComment {
    text: "# comment",
    position: OwnLine,
    formatted: false,
}
', crates/ruff_python_formatter/src/comments/mod.rs:399:9
```

---

_Label `formatter` added by @MichaReiser on 2023-08-10 12:40_

---

_Closed by @MichaReiser on 2023-08-11 07:22_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:11_

---
