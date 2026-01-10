```yaml
number: 5380
title: "UP035: Literal removed too soon"
type: issue
state: closed
author: henryiii
labels:
  - bug
assignees: []
created_at: 2023-06-26T22:32:35Z
updated_at: 2023-06-27T01:57:39Z
url: https://github.com/astral-sh/ruff/issues/5380
synced_at: 2026-01-10T11:09:47Z
```

# UP035: Literal removed too soon

---

_Issue opened by @henryiii on 2023-06-26 22:32_

If you activate UP035, Ruff removes `Literal` imports if 3.8+ is targeted. However, due to a caching bug in `Literal` fixed in CPython 3.10.1+ and 3.9.8+, `typing_extensions` provides `Literal` for <= 3.10.0 - see [the changelog for 4.6.0](https://github.com/python/typing_extensions/blob/main/CHANGELOG.md#release-460-may-22-2023).

---

_Comment by @charliermarsh on 2023-06-26 22:41_

Thanks.

---

_Label `bug` added by @charliermarsh on 2023-06-26 22:41_

---

_Comment by @charliermarsh on 2023-06-27 01:44_

Oh, we recently moved `Literal` to Python 3.10 to match pyupgrade and based on Alex Waygood's advice [here](https://github.com/astral-sh/ruff/issues/5112#issuecomment-1602800558), as part of the next release. Is that sufficient to close this?

---

_Comment by @henryiii on 2023-06-27 01:57_

Yes, I think so. Technically an issue in 3.10.0, but no one should care about a bug in an old .0 release. Thanks!

---

_Closed by @henryiii on 2023-06-27 01:57_

---
