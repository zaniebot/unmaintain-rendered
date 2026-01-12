```yaml
number: 5581
title: False positive for PERF401 when conditional depends on list being appended
type: issue
state: closed
author: dosisod
labels:
  - bug
assignees: []
created_at: 2023-07-07T06:49:57Z
updated_at: 2023-07-09T19:53:29Z
url: https://github.com/astral-sh/ruff/issues/5581
synced_at: 2026-01-12T15:54:45Z
```

# False positive for PERF401 when conditional depends on list being appended

---

_@dosisod_

The following code cannot be re-written as a comprehension. It's a pretty common idiom (from my experience) to remove duplicates from an array while maintaining the order, keeping the first duplicate.

```python
copy: list[int] = []

for i in [1, 1, 2, 2, 3, 3]:
    if i not in copy:
        copy.append(i)

print(copy)  # [1, 2, 3]
```

Since `copy` is being appended to, you cannot use it the conditional branch of a comprehension:

```python
copy = [i for i in [1, 1, 2, 2, 3, 3] if i not in copy]  # error, copy is not defined
```

If `i` is a hashable type, you could use this instead of a comprehension:

```python
nums = [1, 1, 2, 2, 3, 3]
copy = list(dict.fromkeys(nums))

print(copy)  # [1, 2, 3]
```

Though if you're iterating over non-hashable types (which is the case for me), `dict.fromkeys()` won't work.

Ruff output from first example:

```
$ ruff file.py
file.py:5:9: PERF401 Use a list comprehension to create a transformed list
```

Ruff version:

```
$ ruff --version
ruff 0.0.277
```

---

_Label `bug` added by @charliermarsh on 2023-07-07 13:58_

---

_Comment by @charliermarsh on 2023-07-07 13:58_

Hmm, makes sense. We can fix this.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-07-08 05:58_

---

_Closed by @charliermarsh on 2023-07-09 19:53_

---
