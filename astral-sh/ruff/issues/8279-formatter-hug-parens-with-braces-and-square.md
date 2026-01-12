```yaml
number: 8279
title: "Formatter: `hug_parens_with_braces_and_square_brackets` preview style"
type: issue
state: closed
author: konstin
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-10-27T10:37:30Z
updated_at: 2023-12-19T00:17:31Z
url: https://github.com/astral-sh/ruff/issues/8279
synced_at: 2026-01-12T15:54:48Z
```

# Formatter: `hug_parens_with_braces_and_square_brackets` preview style

---

_@konstin_



Port the `hug_parens_with_braces_and_square_brackets` preview style added to black in [PR1](https://github.com/psf/black/pull/3964) and [PR1]([second PR](https://github.com/psf/black/pull/3992)) to ruff formatter:

For better readability and less verticality, _Black_ now pairs parantheses ("(", ")")
with braces ("{", "}") and square brackets ("[", "]") on the same line for single
parameter function calls. For example:

```python
foo(
    [
        1,
        2,
        3,
    ]
)
```

will be changed to:

```python
foo([
    1,
    2,
    3,
])
```

---

_Label `formatter` added by @konstin on 2023-10-27 10:37_

---

_Comment by @MichaReiser on 2023-10-27 10:38_

Is this preview style?

---

_Comment by @konstin on 2023-10-27 10:39_

yes

---

_Label `preview` added by @MichaReiser on 2023-10-27 10:40_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-27 10:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-27 19:33_

---

_Comment by @zanieb on 2023-10-29 05:20_

😬 I do not prefer this style.

---

_Comment by @ju1ius on 2023-10-29 18:15_

> 😬 I do not prefer this style.

😬 I do prefer this style.

---

_Comment by @Mogost on 2023-10-30 06:47_

Don't forget to share your opinions in the black repo when the time comes. 

https://github.com/psf/black/issues/3989#issuecomment-1783196332

> Last year I feel we may not have gotten enough feedback during this period, leading to some controversy over certain decisions, so I hope we'll be able to get more early feedback this year. If Ruff or its users could provide feedback on any preview-style features, I'd be happy.



---

_Comment by @henriholopainen on 2023-10-31 08:25_

FYI: https://github.com/psf/black/pull/3992

---

_Comment by @konstin on 2023-10-31 11:14_

@henriholopainen That's what's implemented in https://github.com/astral-sh/ruff/pull/8293/files#diff-b84e5729fb09571d5fd2e3fcba65612a57765cd6351ba6388b674208b79877ffR48-R60, right?

---

_Renamed from "Formatter: Improved multiline dictionary and list indentation for sole function parameter" to "Formatter: `hug_parens_with_braces_and_square_brackets` preview style" by @MichaReiser on 2023-11-29 00:28_

---

_Comment by @charliermarsh on 2023-12-01 02:11_

This is now supported via https://github.com/astral-sh/ruff/pull/8293, though we don't yet apply these rules recursively (intentionally).

---

_Comment by @ofek on 2023-12-05 18:15_

> we don't yet apply these rules recursively

Good choice, please never do so

---

_Comment by @charliermarsh on 2023-12-19 00:17_

We're gonna continue without applying these rules recursively.

---

_Closed by @charliermarsh on 2023-12-19 00:17_

---
