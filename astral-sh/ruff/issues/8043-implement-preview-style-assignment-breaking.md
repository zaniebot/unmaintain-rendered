```yaml
number: 8043
title: Implement preview style assignment breaking
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-10-18T11:36:32Z
updated_at: 2023-10-20T07:08:49Z
url: https://github.com/astral-sh/ruff/issues/8043
synced_at: 2026-01-12T15:54:47Z
```

# Implement preview style assignment breaking

---

_@konstin_

Black (and our) stable style:
```python
def f():
    """Black's `Preview.prefer_splitting_right_hand_side_of_assignments`"""
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[
        bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
    ] = cccccccc.ccccccccccccc.cccccccc

    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[
        bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
    ] = cccccccc.ccccccccccccc().cccccccc

    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[
        bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
    ] = cccccccc.ccccccccccccc(d).cccccccc

    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb] = (
            cccccccc.ccccccccccccc(d).cccccccc + e
    )

    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb] = (
            cccccccc.ccccccccccccc.cccccccc + e
    )
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb] = (
            cccccccc.ccccccccccccc.cccccccc
            + eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee
    )

    self._cache: dict[
        DependencyCacheKey, list[list[DependencyPackage]]
    ] = collections.defaultdict(list)
    self._cached_dependencies_by_level: dict[
        int, list[DependencyCacheKey]
    ] = collections.defaultdict(list)
```
Preview style, which consistently breaks the right side first:
```python
def f():
    """Black's `Preview.prefer_splitting_right_hand_side_of_assignments`"""
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb] = (
        cccccccc.ccccccccccccc.cccccccc
    )

    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb] = (
        cccccccc.ccccccccccccc().cccccccc
    )

    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb] = (
        cccccccc.ccccccccccccc(d).cccccccc
    )

    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb] = (
        cccccccc.ccccccccccccc(d).cccccccc + e
    )

    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb] = (
        cccccccc.ccccccccccccc.cccccccc + e
    )
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb] = (
        cccccccc.ccccccccccccc.cccccccc
        + eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee
    )

    self._cache: dict[DependencyCacheKey, list[list[DependencyPackage]]] = (
        collections.defaultdict(list)
    )
    self._cached_dependencies_by_level: dict[int, list[DependencyCacheKey]] = (
        collections.defaultdict(list)
    )
```

---

_Label `formatter` added by @konstin on 2023-10-18 11:36_

---

_Comment by @MichaReiser on 2023-10-18 22:03_

Is this the same as https://github.com/astral-sh/ruff/issues/8043 or any other issue in https://github.com/astral-sh/ruff/issues/6935? If not, could you add it to the umbrella issue?

---

_Comment by @konstin on 2023-10-19 09:23_

Thanks, updated #6975

---

_Closed by @konstin on 2023-10-19 09:23_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-20 07:08_

---
