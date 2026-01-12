```yaml
number: 17223
title: Find some way to give nextest a list of tests which are io bound from more sophisticated heuristics than the name
type: issue
state: open
author: EliteTK
labels:
  - enhancement
  - internal
assignees: []
created_at: 2025-12-22T21:49:18Z
updated_at: 2025-12-22T21:49:18Z
url: https://github.com/astral-sh/uv/issues/17223
synced_at: 2026-01-12T16:02:46Z
```

# Find some way to give nextest a list of tests which are io bound from more sophisticated heuristics than the name

---

_@EliteTK_

### Summary

Related to and discussed in #17177 (specifically see [this](https://github.com/astral-sh/uv/pull/17177#discussion_r2631958153_) and [this](https://github.com/astral-sh/uv/pull/17177#issuecomment-3684275691)).

Some tests are heavily IO bound and running all of them concurrently makes them individually take much longer and sometimes lead to flakiness especially on windows.

Currently we consider `python_install::` `python_update::` and `python_find::` tests to be IO bound but these are very coarse grained selectors and do not catch many of the tests which could trigger similar issues.

If tests could be tagged somehow, and potentially selected for by nextest this would help improve this.

One option could be to add each io bound test to a submodule within its test module. e.g. mod io_bound {...}; This would then allow these to be selected with `test(::io_bound::)`. Alternatively we could tag these another way and write a tool which generates a profile override for nextest.

### Example

Tests would time out less often.

---

_Label `enhancement` added by @EliteTK on 2025-12-22 21:49_

---

_Label `internal` added by @EliteTK on 2025-12-22 21:49_

---
