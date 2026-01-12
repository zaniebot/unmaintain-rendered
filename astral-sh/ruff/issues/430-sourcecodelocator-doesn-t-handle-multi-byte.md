```yaml
number: 430
title: "`SourceCodeLocator` doesn't handle multi-byte characters"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-10-14T17:39:21Z
updated_at: 2022-10-14T18:29:20Z
url: https://github.com/astral-sh/ruff/issues/430
synced_at: 2026-01-12T15:54:40Z
```

# `SourceCodeLocator` doesn't handle multi-byte characters

---

_@charliermarsh_

If you have a file like:

```py
class nonascii:
    'Це не латиниця'
    pass
```

Running `cargo run --select D203` throws:

```
thread 'main' panicked at 'byte index 36 is not a char boundary; it is inside 'т' (bytes 35..37) of `class nonascii:
    'Це не латиниця'
    pass
`', library/core/src/str/mod.rs:127:5
```

---

_Label `bug` added by @charliermarsh on 2022-10-14 17:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-14 17:44_

---

_Closed by @charliermarsh on 2022-10-14 18:29_

---
