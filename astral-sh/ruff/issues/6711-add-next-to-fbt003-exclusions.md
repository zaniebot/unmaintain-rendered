```yaml
number: 6711
title: "Add `next` to FBT003 exclusions"
type: issue
state: closed
author: Garrett-R
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-08-21T04:38:32Z
updated_at: 2023-08-21T15:30:18Z
url: https://github.com/astral-sh/ruff/issues/6711
synced_at: 2026-01-12T15:54:46Z
```

# Add `next` to FBT003 exclusions

---

_@Garrett-R_

Following on from [this discussion](https://github.com/astral-sh/ruff/discussions/6302) and [this PR](https://github.com/astral-sh/ruff/pull/6307), it might be good to add the Python builtin `next` to the exclusions list for [FBT003](https://beta.ruff.rs/docs/rules/boolean-positional-value-in-call/).

Consider this code:

```python
next(my_generator, False)
```

The rule is upset about `False` here, but there is no keyword arg you can use.  Trying to use `default=False` yields: `TypeError: next() takes no keyword arguments`.



---

_Label `accepted` added by @charliermarsh on 2023-08-21 14:17_

---

_Label `bug` added by @charliermarsh on 2023-08-21 14:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-21 14:44_

---

_Closed by @charliermarsh on 2023-08-21 14:56_

---

_Comment by @Garrett-R on 2023-08-21 15:30_

Heyooo!  :rocket: 

---
