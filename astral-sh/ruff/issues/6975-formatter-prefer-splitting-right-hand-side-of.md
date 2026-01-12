```yaml
number: 6975
title: "Formatter: `prefer_splitting_right_hand_side_of_assignments` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-08-29T12:50:58Z
updated_at: 2023-12-13T03:43:25Z
url: https://github.com/astral-sh/ruff/issues/6975
synced_at: 2026-01-12T15:54:46Z
```

# Formatter: `prefer_splitting_right_hand_side_of_assignments` preview style

---

_@MichaReiser_

Implement Black's [improved linebreaks](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-line-breaks) preview style. The style is gated behind the [`prefer_splitting_right_hand_side_of_assignments`](https://github.com/psf/black/pull/3368) preview flag.

It seems that the right side now always gets parenthesized regardless if it makes the right fit or not (no longer has the optimisation to omit parentheses if the content still doesn't fit after adding the parentheses).

https://play.ruff.rs/36e390bc-b3d2-41b2-80d9-87608337727a

We may be able to get away by simply not returning `BestFit` if the expression is an assignment value.

I expect this change to improve the performance because there are fewer cases where we'll need to fall back to use Best Fitting.

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

_Label `formatter` added by @MichaReiser on 2023-08-29 12:53_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-08-29 12:53_

---

_Label `preview` added by @MichaReiser on 2023-09-15 09:50_

---

_Comment by @MichaReiser on 2023-10-30 09:33_

We'll need to make `best_fits_parenthesize` break the right before the left when using preview style, similar to regular groups. This can be accomplished by measuring the parenthesized content instead of the unparenthesized content inside of `fits_element`. See https://github.com/astral-sh/ruff/issues/8182#issuecomment-1784712397

We should add a `mode` to `BestFitsParenthesize` to avoid exposing the `preview` flag to the printer (preview is a language specific concept)

---

_Comment by @MichaReiser on 2023-10-31 10:32_

Some more details. https://github.com/psf/black/issues/4006

Overall, the assignment will be the odd one that uses a different parenthesizing layout (not applying the logic to omit parentheses if it exceeds the line width). This will be different from clause headers and `await`, `return`, etc where expressions should only be parenthesized if necessary. This can be implemented by returning `Multline` for the `NeedsParentheses` implementation that currently return `BestFit` if the parent is an assignment

---

_Renamed from "Improved line breaks" to "Formattter: `prefer_splitting_right_hand_side_of_assignments` preview style" by @MichaReiser on 2023-11-29 00:24_

---

_Renamed from "Formattter: `prefer_splitting_right_hand_side_of_assignments` preview style" to "Formatter: `prefer_splitting_right_hand_side_of_assignments` preview style" by @MichaReiser on 2023-11-29 03:08_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-11-29 04:31_

---

_Unassigned @MichaReiser by @MichaReiser on 2023-11-29 07:53_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-11-29 07:54_

---

_Closed by @MichaReiser on 2023-12-13 03:43_

---
