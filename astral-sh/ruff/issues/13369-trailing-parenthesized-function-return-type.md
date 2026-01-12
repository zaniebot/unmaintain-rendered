```yaml
number: 13369
title: Trailing parenthesized function return type comment becomes leading comment
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-09-16T13:54:43Z
updated_at: 2024-09-18T06:26:07Z
url: https://github.com/astral-sh/ruff/issues/13369
synced_at: 2026-01-12T15:54:53Z
```

# Trailing parenthesized function return type comment becomes leading comment

---

_@MichaReiser_

```python
def foo(
    arg: (  # comment with non-return annotation
        int
        # comment with non-return annotation
    ),
):
    pass


def foo(
    arg: (  # comment with non-return annotation
        int
        | range
        | memoryview
        # comment with non-return annotation
    ),
):
    pass

def foo(arg: (
        int
        # only after
)):
    pass
```

**Output**

```python
def foo(
    # comment with non-return annotation
    # comment with non-return annotation
    arg: (int),
):
    pass


def foo(
    # comment with non-return annotation
    # comment with non-return annotation
    arg: (int | range | memoryview),
):
    pass


def foo(
    # only after
    arg: (int),
):
    pass

```

Found by going through the tests in https://github.com/psf/black/pull/4453

---

_Label `bug` added by @MichaReiser on 2024-09-16 13:54_

---

_Label `formatter` added by @MichaReiser on 2024-09-16 13:54_

---

_Closed by @MichaReiser on 2024-09-18 06:26_

---
