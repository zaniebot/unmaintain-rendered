---
number: 7457
title: "Formatter instability: Return type annotation"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-09-17T14:52:34Z
updated_at: 2023-09-20T20:20:23Z
url: https://github.com/astral-sh/ruff/issues/7457
synced_at: 2026-01-10T01:22:47Z
---

# Formatter instability: Return type annotation

---

_Issue opened by @MichaReiser on 2023-09-17 14:52_

## Input

```python
def get_dashboards_hierarchy(
) -> Dict[Type['BaseDashboard'], List[Type['BaseDashboard']]]:
    """Get hierarchy of dashboards classes.

    Returns:
        Dict of dashboards classes.
    """
    dashboards_hierarchy = {}
```

## Format 1

```python
def get_dashboards_hierarchy() -> Dict[
    Type["BaseDashboard"], List[Type["BaseDashboard"]]
]:
    """Get hierarchy of dashboards classes.

    Returns:
        Dict of dashboards classes.
    """
    dashboards_hierarchy = {}
```

## Format 2
```python
def get_dashboards_hierarchy() -> (
    Dict[Type["BaseDashboard"], List[Type["BaseDashboard"]]]
):
    """Get hierarchy of dashboards classes.

    Returns:
        Dict of dashboards classes.
    """
    dashboards_hierarchy = {}
```

Black uses the second style.

Identified by #7445

---

_Label `bug` added by @MichaReiser on 2023-09-17 14:52_

---

_Label `formatter` added by @MichaReiser on 2023-09-17 14:52_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-17 14:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-20 16:18_

---

_Comment by @charliermarsh on 2023-09-20 16:18_

I think Black always parenthesizes the return type (when breaking) if there are no arguments... Is my best guess, from testing. Will play around with it.

---

_Comment by @MichaReiser on 2023-09-20 16:19_

> I think Black always parenthesizes the return type (when breaking) if there are no arguments... Is my best guess, from testing. Will play around with it.

You're fearless! 

---

_Comment by @charliermarsh on 2023-09-20 16:52_

Oh I think we actually handle this correctly (we special-case empty parameters _already_), there's just a bug in the empty detection...

---

_Referenced in [astral-sh/ruff#7550](../../astral-sh/ruff/pulls/7550.md) on 2023-09-20 17:02_

---

_Closed by @charliermarsh on 2023-09-20 20:20_

---
