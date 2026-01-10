```yaml
number: 8910
title: "New rule category `XP`, implement `not-array-agnostic-numpy` (`XP001`)"
type: pull_request
state: closed
author: lucascolley
labels:
  - plugin
assignees: []
draft: true
base: main
head: xp001
created_at: 2023-11-29T17:04:50Z
updated_at: 2024-03-28T17:12:45Z
url: https://github.com/astral-sh/ruff/pull/8910
synced_at: 2026-01-10T22:47:01Z
```

# New rule category `XP`, implement `not-array-agnostic-numpy` (`XP001`)

---

_Pull request opened by @lucascolley on 2023-11-29 17:04_

## Summary

Closes gh-8615

- New rule category `XP`
- New rule `NotArrayAgnosticNumPy`

Aiming for violations for all non-standard NumPy functions and methods and uses of them. Some of these will have standard aliases or alternatives. We should try to catch as many of the 'compatible' and 'strictness' changes of https://numpy.org/doc/stable/reference/array_api.html as possible (the 'breaking' changes should be caught separately - by `NP201` or perhaps `NP202` is also needed).

## Test Plan

_WIP_

---

_Label `plugin` added by @zanieb on 2023-11-29 17:05_

---

_Review comment by @lucascolley on `crates/ruff_linter/src/registry.rs`:211 on 2023-11-29 17:08_

```suggestion
    /// [Array-agnosticism](https://github.com/data-apis/array-api)
```

---

_@lucascolley reviewed on 2023-11-29 17:09_

---

_@lucascolley reviewed on 2023-12-06 18:15_

---

_Review comment by @lucascolley on `crates/ruff_linter/src/rules/xp/rules/not_array_agnostic_numpy.rs`:374 on 2023-12-06 18:15_

```suggestion
                        " is not in the array API standard"
```

---

_Review comment by @lucascolley on `crates/ruff_linter/src/rules/xp/rules/not_array_agnostic_numpy.rs`:331 on 2023-12-06 20:02_

```suggestion
```

---

_@lucascolley reviewed on 2023-12-06 20:02_

---

_Comment by @lucascolley on 2023-12-06 23:42_

Hey @charliermarsh, here's where I'm at currently. I've included the raw naming differences as replacements so far.

### To-do:
- Include the standard extension modules to check against `numpy.linalg` and `numpy.fft`
- Start work on tests

### Are these things possible?
- Detecting methods of `numpy.ndarray` e.g. we want to change `a.astype(...)` to `np.astype(a, ...)` when `a` is of type `numpy.ndarray`
- Detecting whether certain arguments are passed e.g. if `numpy.transpose(a)` is used without the `axes` keyword then it should be replaced with `numpy.permute_dims(a, axes=range(a.ndim)[::-1])`. If it is provided, then a straight swap for `permute_dims` is needed
- Detecting the dtypes used with operators e.g.

> "Operators (like +) with Python scalars only accept matching scalar types - For example, `<int32 array> + 1.0` is not allowed.

- Changing positional arguments to keyword arguments and vice versa

---

_Assigned to @charliermarsh by @zanieb on 2024-03-12 18:52_

---

_Comment by @MichaReiser on 2024-03-28 16:51_

Thank you @lucascolley for this rule proposal and implementation. I really like the idea of it. I think such a rule would be very helpful for our users. Unfortunately, Ruff doesn't have a good concept today for very opinionated rules or rules that intentionally restrict the allowed APIs. Ruff also lacks a good default rule set, and, unfortunately, many users assume that `ALL` is our recommended rule set. That means users using `--select ALL` would automatically opt into this new restricting rule, and they do not necessarily have the experience to judge whether the pattern flagged by this rule is indeed a "problem" for them or if they are better off by ignoring the rule. We want to hold off from merging restricting rules until #1774 is complete. We're actively working on it, and we hope to have a good place for rules like the one you proposed here. 

I'm sorry that it took us so long to give that feedback to you, and I hope we can get this rule landed once #1774 is completed. Thanks again.


---

_Closed by @MichaReiser on 2024-03-28 16:51_

---

_Comment by @lucascolley on 2024-03-28 17:02_

Thanks for the reply @MichaReiser , no worries! Happy to return to this once gh-1774 is resolved. I agree that this should be explicitly opt-in at all times.

> That means users using --select ALL would automatically opt into this new restricting rule

Does that include `preview` rules? If so, should it not?

---

_Comment by @MichaReiser on 2024-03-28 17:12_

> Does that include preview rules? If so, should it not?

It does include preview rules when running ruff with `preview = true` (you have to opt into preview). We considered accepting rules as preview only but we decided against it because accepting rules without a clear path to stabilization.



---
