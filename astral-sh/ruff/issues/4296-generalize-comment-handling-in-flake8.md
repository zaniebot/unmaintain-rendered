```yaml
number: 4296
title: "Generalize comment handling in `flake8-comprehension` fixes"
type: issue
state: open
author: dhruvmanila
labels:
  - fixes
assignees: []
created_at: 2023-05-09T01:08:47Z
updated_at: 2023-05-09T03:37:31Z
url: https://github.com/astral-sh/ruff/issues/4296
synced_at: 2026-01-12T15:54:44Z
```

# Generalize comment handling in `flake8-comprehension` fixes

---

_@dhruvmanila_

I've provided a few examples but there will be a few more. As mentioned that whenever the parenthesis or brackets are being dropped, that's where this will happen. It seems to be happening in most of comprehension fixes.

I believe we should have a generalized way of resolving this.

---

## C404

```python
dict(
    # first comment
    [
        # second comment
        (i, i)
        for i in range(3)
    ]
)

{
    # first comment
    i: i
        for i in range(3)
}
```

## C405

```python
set(
    # some comment
    (1, 2)
)

{1, 2}
```

## C406

```python
dict(
    # first comment
    [
        # second comment
        (1, 2)
    ]
)

{
    # first comment
    1: 2
}
```

## C409

```python
tuple(
    # some comment
    [1, 2, 3, 4]
)

(1, 2, 3, 4)
```

## C410

```python
list(
    # some comment
    [1, 2, 3, 4]
)

[1, 2, 3, 4]
```

## C411

```python
list(
    # comment
    [i * i for i in x]
)

[i * i for i in x]
```

_Originally posted by @dhruvmanila in https://github.com/charliermarsh/ruff/issues/4099#issuecomment-1539198573_
            

---

_Renamed from "Generalize comment handling in `flake8-comprehension` autofix" to "Generalize comment handling in `flake8-comprehension` fixes" by @dhruvmanila on 2023-05-09 01:09_

---

_Label `autofix` added by @charliermarsh on 2023-05-09 03:37_

---
