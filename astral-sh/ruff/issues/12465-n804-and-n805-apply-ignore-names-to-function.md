```yaml
number: 12465
title: "N804 and N805 apply `ignore-names` to function, rather than parameter names (`self`, `cls`)"
type: issue
state: closed
author: njzjz
labels:
  - needs-decision
assignees: []
created_at: 2024-07-22T21:15:37Z
updated_at: 2024-07-24T22:08:24Z
url: https://github.com/astral-sh/ruff/issues/12465
synced_at: 2026-01-12T15:54:51Z
```

# N804 and N805 apply `ignore-names` to function, rather than parameter names (`self`, `cls`)

---

_@njzjz_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I want to use [`N804`](https://docs.astral.sh/ruff/rules/invalid-first-argument-name-for-class-method/) to correct `def setUpClass(self):` in my repository. However, I found that when the class method name is `setUpClass`, no error is reported.

ruff v0.5.4
https://play.ruff.rs/f96de0de-0667-4e6b-88c1-fe229ad7ca5a
```py
import unittest

class TestXXX(unittest.TestCase):
    @classmethod
    def setUpClass(self):
        # no error reported
        pass


class TestXXX:
    @classmethod
    def setUpClss(self):
        # N804: First argument of a class method should be named `cls`
        pass
```

I don't understand why N804 ignores `setUpClass`. There is no documentation for this behavior.


---

_Comment by @njzjz on 2024-07-22 21:24_

Oh, I just understand that the default [ignore-names](https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names) contains `setUpClass`. However, N804 does not rename `setUpClass`. It just renames `self` to `cls`.

---

_Comment by @charliermarsh on 2024-07-22 21:30_

Is the request that `setUpClass` be renamed to `set_up_class`?

---

_Comment by @njzjz on 2024-07-22 21:34_

> Is the request that `setUpClass` be renamed to `set_up_class`?

No, N804 should just rename

```py
@classmethod
def setUpClass(self):
```

to 

```py
@classmethod
def setUpClass(cls):
```

---

_Comment by @charliermarsh on 2024-07-22 22:31_

Ok, I think I understand what you're saying: `setUpClass` should _not_ be ignored by N804 despite being in `ignore-names`, because the rule is about the parameter (`self`) rather than the function (`setUpClass`). We've had this behavior from the start (#2855), so it may not even be intentional.

It'd be reasonable to change, but it also means that there's no way to ignore methods that _do_ deviate from the pattern in that way.

I'd be nice to get more opinions on this. Maybe @AlexWaygood? What behavior would you expect here?


---

_Label `needs-decision` added by @charliermarsh on 2024-07-22 22:31_

---

_Renamed from "N804 ignores `setUpClass` methods" to "N804 and N805 apply `ignore-names` to function, rather than parameter names (`self`, `cls`)" by @charliermarsh on 2024-07-22 22:31_

---

_Comment by @AlexWaygood on 2024-07-23 14:11_

This is an interesting question! I'd be curious to see the ecosystem results on an experimental PR that ignored `ignore-names` for N804. I don't feel like I have any sense of how many users might be overriding `ignore-names` in their config to systematically ignore all N804 violations for all methods that have a specific name across a codebase.

It _does_ feel a bit silly to me that we ignore N804 violations by default if the method is called `setUpClass`, because that's _obviously_ not the motivation for including that entry in the `ignore-names` default list. But I can also see @charliermarsh's point that if N804 just ignores the `ignore-names` setting entirely then there's no setting available to users that they can use to tweak how N804 works.

Possibly we could start ignoring the setting for N804 in preview mode only, and see if anybody complains. If we do get any complaints, we could add a new dedicated setting that could be separately configured for determining the behaviour of rules like N804 that complain about parameter names rather than class or method names.

---

_Comment by @charliermarsh on 2024-07-23 15:58_

Yeah the current behavior is strange. Though it's also strange to imagine putting `self` or `cls` in `ignore-names`, since it's effectively disabling the rules. We should change it though and see if the ecosystem checks look good.

---

_Comment by @AlexWaygood on 2024-07-23 16:02_

Right, I don't think it makes sense to have N804 be disabled if N804 sees `cls` or `self` in `ignore-names` either. What would make sense to me would be if we had a separate setting -- `ignore-weird-parameter-names-for-these-methods` (bikeshedding permitted, I thought about it for 0 seconds ;) -- that could be used to configure N804 separately to the `N*` rules that are configured via `ignore-names`. But I just don't really know if users actually need to configure N804 in the first place.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-24 21:29_

---

_Closed by @charliermarsh on 2024-07-24 22:08_

---
