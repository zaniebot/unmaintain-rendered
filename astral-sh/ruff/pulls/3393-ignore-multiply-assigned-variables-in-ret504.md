```yaml
number: 3393
title: Ignore multiply-assigned variables in RET504
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/multi-assign
created_at: 2023-03-08T00:25:53Z
updated_at: 2023-03-09T19:56:51Z
url: https://github.com/astral-sh/ruff/pull/3393
synced_at: 2026-01-12T15:55:12Z
```

# Ignore multiply-assigned variables in RET504

---

_@charliermarsh_

This PR acts on the suggestion from #2950 to avoid RET504 errors when a variable is assigned to multiple times. As-is, we have too many false positives like:

```py
def get_queryset(option_1, option_2):
    queryset = Model.filter(a=1)
    if option_1:
        queryset = queryset.filter(b=2)
    if option_2:
        queryset = queryset.filter(c=3)
    return queryset
```

Instead, this rule will now only flag more trivial cases of "assign-then-return", like:

```py
def get_queryset():
    queryset = Model.filter(a=1)
    return queryset
```

It's possible that this is _too_ conservative, since we no longer flag cases like:

```py
def get_queryset():
    queryset = Model.filter(a=1)
    queryset = queryset.filter(c=3)
    return queryset
```

But, I actually think that's reasonable -- if you're multiply-assigning, you're often doing something like a fluent interface or a conditional assignment, and in those cases, it actually seems totally fine (preferable, even) to separate the operation chain from the return.

Closes #2950.


---

_Label `bug` added by @charliermarsh on 2023-03-08 00:25_

---

_Merged by @charliermarsh on 2023-03-09 00:11_

---

_Closed by @charliermarsh on 2023-03-09 00:11_

---

_Branch deleted on 2023-03-09 00:11_

---

_Comment by @henryiii on 2023-03-09 19:56_

Note: the "fix" for case one was to write:

```python
def get_queryset(option_1, option_2):
    queryset = Model.filter(a=1)
    if option_1:
        queryset = queryset.filter(b=2)
    if option_2:
        return queryset.filter(c=3)
    return queryset
```

Which wasn't obvious at first, and made it seem like it was misbehaving. After seeing it in place (usually `option1` and `option2` are not as symmetric as they seem above), I actually like it a bit better - it makes parsing the logic a bit simpler, as there's less overwriting of the same variable.

And

```python
def get_queryset():
    queryset = Model.filter(a=1)
    return queryset.filter(c=3)
```

is _way_ better, IMO, as it completely skips overwriting this variable. This "fluent" style won't work with static typing checking most of the time either, so I think it's fine to disable this check if you are using it and really want the extra line. I mean, if you don't like a check you don't have to use it? `¯\_(ツ)_/¯`

---
