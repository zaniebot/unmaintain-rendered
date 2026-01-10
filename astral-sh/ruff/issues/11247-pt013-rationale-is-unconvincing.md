```yaml
number: 11247
title: "[PT013] Rationale is unconvincing."
type: issue
state: closed
author: randolf-scholz
labels: []
assignees: []
created_at: 2024-05-02T16:39:12Z
updated_at: 2024-08-29T07:25:46Z
url: https://github.com/astral-sh/ruff/issues/11247
synced_at: 2026-01-10T11:09:53Z
```

# [PT013] Rationale is unconvincing.

---

_Issue opened by @randolf-scholz on 2024-05-02 16:39_

[Rule PT013](https://docs.astral.sh/ruff/rules/pytest-incorrect-pytest-import/) seems arbitrary. The rationale states:

> pytest should be imported as import pytest and its members should be accessed in the form of pytest.xxx.yyy for consistency and to make it easier for linting tools to analyze the code.

But the same reasoning *"make it easier for linting tools to analyze the code"* could be applied to any library.

What are some concrete linting tools this applies to? If it makes no difference to `ruff`, should this be enabled by default in `ruff`?

---

_Comment by @charliermarsh on 2024-05-03 00:46_

Yeah, I'm going to remove that piece from the rationale -- it makes no difference to Ruff. I don't want to go as far as to remove or deprecate the rule, since it _can_ be useful to enforce this convention (and I did some code search -- several large projects _do_ enable this rule), but we should reframe the rationale.

(Note that this rule isn't enabled by default in Ruff; you have to enable the `PT` category or `PT013`.)

---

_Closed by @charliermarsh on 2024-05-03 00:51_

---

_Comment by @randolf-scholz on 2024-05-03 09:17_

> and I did some code search -- several large projects do enable this rule

But is it cargo cult programming, or is there an actual good reason for it nowadays?

The rest of the PT-rules all look very sensible.

---

_Comment by @DanielYang59 on 2024-08-29 06:38_

Thanks for the helpful discussion, I happen to have similar question.

>  I don't want to go as far as to remove or deprecate the rule, since it _can_ be useful to enforce this convention (and I did some code search -- several large projects _do_ enable this rule)

Not sure if we could/should set some exceptions for `approx` for example ([even pytest document use `from pytest import approx`](https://docs.pytest.org/en/stable/reference/reference.html#pytest.approx)), or only check certain patterns that is considered unconventional (for example `from pytest import raises/mark/fixture/...`). Because `approx` is heavily and repeatedly used for numerical comparisons https://github.com/materialsproject/pymatgen/pull/4022, and `pytest.approx` could be very verbose under such scenario.


---
