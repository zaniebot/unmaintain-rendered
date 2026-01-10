```yaml
number: 12690
title: "C419 should not flag set comprehensions for `sum`"
type: issue
state: closed
author: dylwil3
labels:
  - bug
assignees: []
created_at: 2024-08-05T16:33:15Z
updated_at: 2024-08-06T02:30:59Z
url: https://github.com/astral-sh/ruff/issues/12690
synced_at: 2026-01-10T11:09:54Z
```

# C419 should not flag set comprehensions for `sum`

---

_Issue opened by @dylwil3 on 2024-08-05 16:33_

As was pointed out in a [recent comment](https://github.com/astral-sh/ruff/pull/12657#issuecomment-2267564369), set comprehension is not always as safe to remove as list comprehension in function calls because sets deduplicate elements.

For example:

```python
sum({x for x in [1,1,1]})
```

is not the same as

```python
sum(x for x in [1,1,1])
```

(Incidentally the violation message says "list comprehension" here when it ought to say "set comprehension").

[Playground link](https://play.ruff.rs/c1030c61-729c-4c33-9f29-40eab88c39ef)

I propose that we organize the builtins covered by this rule as either being `duplication-invariant` or not, and only suggest removing set comprehensions in that case. At the moment, the only _non_ duplication-invariant rule is `sum` (but more builtins may be added later).

Incidentally, later we may have to also account for whether the builtin function is symmetric (i.e. invariant under permutation of the inputs), but maybe it's best to address that when the time comes (all currently supported builtins are symmetric).


---

_Label `bug` added by @charliermarsh on 2024-08-06 02:18_

---

_Comment by @charliermarsh on 2024-08-06 02:18_

Thanks!

---

_Closed by @charliermarsh on 2024-08-06 02:30_

---

_Closed by @charliermarsh on 2024-08-06 02:30_

---
