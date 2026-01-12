```yaml
number: 5355
title: Improve copy of console command examples
type: issue
state: closed
author: zanieb
labels:
  - documentation
assignees: []
created_at: 2024-07-23T17:42:20Z
updated_at: 2024-07-31T14:52:03Z
url: https://github.com/astral-sh/uv/issues/5355
synced_at: 2026-01-12T15:58:55Z
```

# Improve copy of console command examples

---

_@zanieb_

e.g.

```console
$ uv foo
```

cannot be copy pasted without the leading `$` in the documentation.

See https://github.com/PrefectHQ/prefect/pull/9204 for a bunch of discussion about this problem and potential solutions.

A possible solution is to use `bash` syntax instead and omit the `$` entirely, but this looses the context that a command is intended to be executed in a terminal :(

---

_Label `documentation` added by @zanieb on 2024-07-23 17:42_

---

_Closed by @zanieb on 2024-07-31 14:52_

---

_Closed by @zanieb on 2024-07-31 14:52_

---
