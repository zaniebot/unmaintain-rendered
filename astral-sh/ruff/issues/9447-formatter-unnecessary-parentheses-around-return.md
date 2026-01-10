```yaml
number: 9447
title: "Formatter: Unnecessary parentheses around return type annotations"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-01-09T08:22:51Z
updated_at: 2024-09-20T07:23:55Z
url: https://github.com/astral-sh/ruff/issues/9447
synced_at: 2026-01-10T11:09:51Z
```

# Formatter: Unnecessary parentheses around return type annotations

---

_Issue opened by @MichaReiser on 2024-01-09 08:22_

Ruff adds unnecessary parentheses around return type annotations that are either:

* guaranteed to break like multiline strings
* Come with their own set of parentheses

```python
def test() -> """test
""": pass

def test() -> [aaa, bbbb,]: pass

def test() -> lists(a, b,): pass
```

**Ruff**

```python
def test() -> (
    """test
"""
):
    pass


def test() -> ([
    aaa,
    bbbb,
]):
    pass


def test() -> (
    lists(
        a,
        b,
    )
):
    pass

```

**Black**

```python
def test() -> """test
""":
    pass


def test() -> [
    aaa,
    bbbb,
]:
    pass


def test() -> lists(
    a,
    b,
):
    pass
```

---

_Label `bug` added by @MichaReiser on 2024-01-09 08:22_

---

_Label `formatter` added by @MichaReiser on 2024-01-09 08:22_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2024-01-09 08:22_

---

_Comment by @icp1994 on 2024-07-26 06:45_

I know this is somewhat of an edge case, but any workaround (tweaks in settings) for this?

Off-topic: maybe issues from [here](https://github.com/astral-sh/ruff/milestones/Formatter:%20Stable) could be moved to a new `Formatter: Post Stable` milestone for easier tracking.

---

_Removed from milestone `Formatter: Stable` by @dhruvmanila on 2024-07-26 06:51_

---

_Closed by @MichaReiser on 2024-09-20 07:23_

---
