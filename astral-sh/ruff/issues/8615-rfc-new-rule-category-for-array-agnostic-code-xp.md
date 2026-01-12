```yaml
number: 8615
title: "RFC: new rule category for array-agnostic code (XP)"
type: issue
state: open
author: lucascolley
labels:
  - rule
assignees: []
created_at: 2023-11-11T12:41:57Z
updated_at: 2024-03-28T16:52:23Z
url: https://github.com/astral-sh/ruff/issues/8615
synced_at: 2026-01-12T15:54:48Z
```

# RFC: new rule category for array-agnostic code (XP)

---

_@lucascolley_

## Context

[NumPy is planning to add support for the array API standard in NumPy 2.0](https://github.com/numpy/numpy/issues/25076) (NEP pending), and other array libraries are expected to follow if this work goes through.

This means that NumPy's API will become a superset of [the array API standard specification](https://data-apis.org/array-api/latest/API_specification/index.html), which will allow users to write 'array-agnostic' code to the standard specification and have it work with NumPy.

The converse, however, is not true, since NumPy's API will be a _proper_ superset; a user cannot write code to the NumPy API and have it work with any standard-compliant array library. This is for two reasons:

1) For some functionality, NumPy provides multiple ways to achieve it, while the standard only requires one (or a proper subset) of those ways.

2) NumPy provides some functionality which is not present in the standard.

'Array consumer' libraries are interested in array-agnostic code as it has the potential to open up their code to new devices and greater interoperability. A ruff rule could help automate part of the conversion process described in my blog post, [The Array API Standard in SciPy](https://labs.quansight.org/blog/scipy-array-api).

## Proposal

Add a new (indefinitely preview? certainly opt-in) rule category, `XP`, to fix/flag code which is not array-agnostic. The first of these rules, `XP001`, would do so for code using NumPy. The rule would assume that code has already been migrated to NumPy 2.0, and would fill in the gaps as noted above:

- For cases of (1), fix the code where there is a drop in alternative (e.g. `np.shape(a)` -> `a.shape`).
- For cases of (2), flag that the user either needs to find an alternative way of achieving the same functionality, or write their own array-agnostic version (e.g. in SciPy we wrote an array-agnostic version of `np.cov`).

## Use Case

Context: a hypothetical future where PyTorch is compliant with the array API standard (as they plan to be).

Problem: The user has code which uses NumPy 1.x. They want their code to work with PyTorch tensors for speedy GPU reasons, or for interoperability with a package built around PyTorch tensors.

Solution:

1. Migrate to NumPy 2.0 using @mtsokol 's `NPY201`.
2. Convert to array-agnostic code using the proposed `XP001`.

_(2.5: write `np` as `xp` and do a whole bunch of work on their test suite etc.)_

3. Swap `import numpy as xp` for `import torch as xp` _(side note: or get the namespace directly from inputs)_.

Voilà!

## Additional Information

- The idiomatic way to write array-agnostic code is with the array library spelled `xp`. A find-and-replace of `np` with `xp` is strongly recommended, but I don't know whether this would fit into ruff's part of the story.

Please let me know any thoughts on whether this is something which could belong in ruff, or how to go about implementing it. I'm new to Rust, but would be happy to get started on a PR when I find the time! Cheers 👋 

---

_Comment by @charliermarsh on 2023-11-11 15:07_

Thanks so much for this clear write-up! It's a neat idea and I have no specific opposition to including these rules other than ensuring that we can actually support them in a reliable way (now, or if not, then in the future).

For example, one clarifying question: am I right that converting `np.shape(a)` to `a.shape` is only valid if `a` is a NumPy array? Whereas, e.g., `np.shape([])` is valid, but `[].shape` is not? If so, it may just mean that the rules need to be marked as unsafe and clearly documented, but I want to make sure I understand the limitations, since we can't rigorously infer whether or not `a` is a NumPy array right now. (We _can_ infer if it's something else, like a list or set or dict, at least in some limited cases.)

---

_Label `rule` added by @charliermarsh on 2023-11-11 15:07_

---

_Comment by @lucascolley on 2023-11-11 15:16_

> am I right that converting np.shape(a) to a.shape is only valid if a is a NumPy array? Whereas, e.g., np.shape([]) is valid, but [].shape is not?

Yep that sounds right to me.

> it may just mean that the rules need to be marked as unsafe and clearly documented, but I want to make sure I understand the limitations, since we can't rigorously infer whether or not a is a NumPy array right now.

Yeah, this may mean that the `fix` capabilities will be quite limited. Even still, a rule which just flagged all of the places that need changes will be useful. The current method being used in SciPy is to just throw [`numpy.array_api`](https://numpy.org/devdocs/reference/array_api.html) arrays (a strict minimal implementation of the standard) at the test suite and see what fails. Having a static check would improve the experience for SciPy, but would be especially useful for other packages who would like to see how much work it would be to convert, without having to commit to modifying their test suite first.

---

_Comment by @lucascolley on 2023-11-11 15:20_

cc @jpivarski who was the first to think about the limitations on the fix capabilities on the Scientific Python discord.

---

_Comment by @charliermarsh on 2023-11-11 15:27_

Yeah, these seem easy enough to flag, and we can use our `unsafe` concepts to appropriately frame the fixes.

Are there any types we can leverage here? E.g., will there be a type annotation for a compatible array? Such that if the user marks a function argument as "implementing the spec", we can make the fix with confident? (E.g., `def f(x: np.ndarray)` -- I don't know if that's exactly correct for NumPy but hopefully conveys the idea.)

---

_Comment by @lucascolley on 2023-11-11 15:38_

> Are there any types we can leverage here? E.g., will there be a type annotation for a compatible array?

I'll hand over to @mtsokol @rgommers who can hopefully answer more accurately than I can :)

---

_Comment by @rgommers on 2023-11-12 10:31_

> Are there any types we can leverage here? E.g., will there be a type annotation for a compatible array?

There will be type annotations, but ... that's a little complicated. In end user code, I'd expect that they write for a specific library, so their annotation will be (e.g.) `numpy.NDArray` or `torch.Tensor`. In library code is where I'd expect to find array-type-agnostic code. Since different array types do not have a common class hierarchy, that can only be annotated via a typing `Protocol`. There's a mostly complete PR for that, but not yet merged: https://github.com/data-apis/array-api/pull/589. So it'll be quite a while before that will be used regularly.

---

_Comment by @rgommers on 2023-11-12 11:02_

On this idea in general: it is hard for it to be complete (as the `shape` example above shows), but I like it in principle and think it could be quite useful. I'd suggest targeting rules first that are going to be robust 1:1 replacements. Often those correspond to "numpy best practices" anway. Example: `np.rollaxis` is a legacy function that will remain (not deprecated) but is effectively superceded by `np.moveaxis`, as explained in the `rollaxis` docstring. That replacement should always work, because both take the same "array-like" input. Here's a PR that did the replacements for SciPy: https://github.com/scipy/scipy/pull/14820. Other examples are `amax`/`amin` (should be `max`/`min`) and `alltrue` (should be `all`).

There are quite a few more that will be helpful when adding array API support, but are functions very recently added to numpy (or still to be added), so it's probably too early to add them in Ruff proper. Examples: some matmul/dot/norm type functions; e.g., numpy has 7 dot functions while the array API 4 which are more orthogonal, and `norm` -> `vector_norm`/`matrix_norm`, `arccos` & co -> C99 names (`acos` & co).

---

_Comment by @lucascolley on 2023-11-12 11:11_

> There are quite a few more that will be helpful when adding array API support, but are functions very recently added to numpy (or still to be added), so it's probably too early to add them in Ruff proper.

Yeah, my intention would be to work on this gradually in the background (probably using it personally from source) but only look for a release once we are really looking to push for widespread adoption.

---

_Comment by @MichaReiser on 2024-03-28 16:52_

I really like the idea of this rule but we need to wait until #1774 is complete to have a better "home" for the rule.

---
