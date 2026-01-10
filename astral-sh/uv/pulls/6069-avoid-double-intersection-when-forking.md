```yaml
number: 6069
title: Avoid double-intersection when forking
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
  - preview
assignees: []
base: main
head: charlie/negate
created_at: 2024-08-13T21:31:11Z
updated_at: 2024-08-14T11:53:56Z
url: https://github.com/astral-sh/uv/pull/6069
synced_at: 2026-01-10T13:09:50Z
```

# Avoid double-intersection when forking

---

_Pull request opened by @charliermarsh on 2024-08-13 21:31_

## Summary

In the forking code, I noticed that we were checking `is_disjoint`, and then calling `.and`. But my understanding is that _exactly_ equivalent to checking if `.and` yields `false`. This PR modifies the operations to instead call `.and` and then check `.is_false` directly.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-13 21:31_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-13 21:31_

---

_Label `performance` added by @charliermarsh on 2024-08-13 21:31_

---

_Label `preview` added by @charliermarsh on 2024-08-13 21:31_

---

_Comment by @ibraheemdev on 2024-08-13 23:37_

I'm actually working on making `is_disjoint` much cheaper than calling `and`, so I think we should prefer the code as is (this was also likely @BurntSushi's intent in `!is_disjoint` being a relatively cheap fast path).

---

_Comment by @charliermarsh on 2024-08-13 23:48_

Ok no problem.

---

_Closed by @charliermarsh on 2024-08-13 23:48_

---

_Comment by @BurntSushi on 2024-08-14 11:53_

I might have just gotten lucky here. The first version of this code was written against the markers of yore, before ADD. In that context, the pattern of doing an `and` followed by an `is_false` didn't really exist, or would be likely less reliable than doing a disjoint check up-front.

---
