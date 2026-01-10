---
number: 9149
title: D102 vs pylint counterpart C0116 (undocumented-public-method)
type: issue
state: open
author: sigma67
labels:
  - docstring
  - type-inference
assignees: []
created_at: 2023-12-15T11:58:18Z
updated_at: 2024-07-26T01:35:30Z
url: https://github.com/astral-sh/ruff/issues/9149
synced_at: 2026-01-10T01:22:48Z
---

# D102 vs pylint counterpart C0116 (undocumented-public-method)

---

_Issue opened by @sigma67 on 2023-12-15 11:58_

As noted in https://github.com/astral-sh/ruff/issues/970#issuecomment-1341901539 , in #970 D102 is different from its pylint counterpart C0116.

 It only requires that a function has docstring in the class hierarchy and does not require the overriding method to have a docstring (only the base method).

``` python
class Foo:
  def validate(self):
    """Doc."""
    ...

class Bar(Foo):
  def validate(self):
    ...
```

I'm creating a separate issue to give this topic some visibility and to discuss it.

---

_Renamed from "D103 vs pylint counterpart C0116 (undocumented-public-function)" to "D102 vs pylint counterpart C0116 (undocumented-public-method)" by @sigma67 on 2023-12-15 12:01_

---

_Comment by @sigma67 on 2023-12-15 12:04_

The same applies to `D105` when overriding a base class' magic method, which already has a docstring. In pylint `C0116` this is ok, in `D105` it's not.

---

_Comment by @sigma67 on 2023-12-18 12:14_

@charliermarsh Perhaps you can comment if this is intended or could be changed? This would not be a breaking change, more of a "softening" of the rule to be more compatible with repositories moving to ruff from pylint.

CC @hmc-cs-mdrissi original report

---

_Comment by @charliermarsh on 2023-12-18 16:44_

@sigma67 - The challenge here is that, unlike Pylint, Ruff doesn't do analysis _across_ files right now. So we could support this _within_ a file, but we wouldn't be able to detect that the `validate` on `Bar` is an implementation of a parent interface if `Foo` is defined in another file.

One valid solution here is that we do ignore `D102` if you mark the method with `typing.override` or `typing_extensions.override`, which signals to type checkers that the method is intended to override a parent method.

---

_Comment by @sigma67 on 2023-12-18 19:09_

@charliermarsh  I see, thanks for the clarification and the hint regarding `typing.override`, that's quite helpful.

Are there future plans for cross-file analysis?

---

_Comment by @charliermarsh on 2023-12-18 19:12_

@sigma67 -- Yes, it will absolutely be part of Ruff in the future. It's a big project though so will take some time :)

---

_Label `type-inference` added by @charliermarsh on 2024-07-26 01:35_

---

_Label `docstring` added by @charliermarsh on 2024-07-26 01:35_

---
