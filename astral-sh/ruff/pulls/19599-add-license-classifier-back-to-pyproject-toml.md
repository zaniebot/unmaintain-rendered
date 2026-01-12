```yaml
number: 19599
title: Add license classifier back to pyproject.toml
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: revert-19499-PEP639
created_at: 2025-07-28T13:46:49Z
updated_at: 2025-08-04T18:59:30Z
url: https://github.com/astral-sh/ruff/pull/19599
synced_at: 2026-01-12T15:56:43Z
```

# Add license classifier back to pyproject.toml

---

_@MichaReiser_

Reverts astral-sh/ruff#19499

@AlexWaygood brought to light that the same change caused a lot of churn for `typing_extensions` users: 

> Just as an FYI, we received a lot of confused bug reports at typing_extensions after we made a similar change, due to some tooling not yet supporting PEP 639
> 
> * https://github.com/python/typing_extensions/issues/576
> * https://github.com/python/typing_extensions/issues/563
> * https://github.com/python/typing_extensions/issues/562
> * https://github.com/python/typing_extensions/issues/559
> * https://github.com/python/typing_extensions/pull/584


Given that this causes significant churn without a very clear benefit for users, we decided to wait with this change until the ecosystem is further along.


---

_@konstin reviewed on 2025-07-28 13:50_

---

_Review comment by @konstin on `pyproject.toml`:26 on 2025-07-28 13:50_

We only need this line for backwards compatibility with pip-licenses, the rest can be PEP 639: https://github.com/raimon49/pip-licenses/blob/3fbf20f9d757d2b9298dfc97af1bd3a987124c98/piplicenses.py#L603-L612

---

_Review requested from @konstin by @MichaReiser on 2025-07-28 13:58_

---

_@konstin approved on 2025-07-28 16:30_

---

_@AlexWaygood reviewed on 2025-07-28 16:37_

---

_Review comment by @AlexWaygood on `pyproject.toml`:26 on 2025-07-28 16:37_

```suggestion
    "License :: OSI Approved :: MIT License",  # for compatibility with tooling such as pip-licenses
```

in case some eager contributors try to remove this, since it's officially deprecated

---

_Renamed from "Revert "Use PEP 639 license information for Ruff itself instead of classifier"" to "Add license classifier back to pyproject.toml" by @AlexWaygood on 2025-07-28 16:38_

---

_Merged by @MichaReiser on 2025-07-28 19:58_

---

_Closed by @MichaReiser on 2025-07-28 19:58_

---

_Branch deleted on 2025-07-28 19:58_

---

_Comment by @vivodi on 2025-07-31 07:53_

> Given that this causes significant churn without a very clear benefit for users, we decided to wait with this change until the ecosystem is further along.

However, `typing_extensions` has not reverted this change.

[PEP 639](https://peps.python.org/pep-0639/#deprecate-license-classifiers) **explicitly recommends removing the license classifier**. Ruff should lead by example in adopting PEP 639 and encourage tools such as `pip-license` to follow suit.

Given its wide adoption, Ruff, like `typing_extensions`, bears a responsibility to help advance the Python ecosystem. @MichaReiser @AlexWaygood 


---

_Comment by @DimitriPapadopoulos on 2025-07-31 09:29_

@AlexWaygood I understand the [`typing_extensions`](https://github.com/python/typing_extensions) issues  are caused by [`pip-licenses`](https://github.com/raimon49/pip-licenses) missing support for PEP 639 (https://github.com/raimon49/pip-licenses/issues/225 and https://github.com/raimon49/pip-licenses/pull/213), aren't they?

But then the **Test Plan** in https://github.com/astral-sh/ruff/pull/19624#issue-3275015612 seems to suggest that `uv build --sdist` might not add the `LICENSE` file to the package. @ntBre Could you shed some light on the actual problem with #19499?

---

_Comment by @MichaReiser on 2025-07-31 09:37_

> But then the Test Plan in https://github.com/astral-sh/ruff/pull/19624#issue-3275015612 seems to suggest that uv build --sdist might not add the LICENSE file to the package. @ntBre Could you shed some light on the actual problem with https://github.com/astral-sh/ruff/pull/19499?

This is the reason why we reverted the entire change and it requires a fix to maturin first.

---

_Comment by @DimitriPapadopoulos on 2025-07-31 11:19_

Are you certain this is an issue with maturin? In theory, maturin ≥ 1.9 is supposed to support PEP 639:
https://www.maturin.rs/changelog.html

As far as I can understand, this is an issue with [pip-licenses](https://github.com/raimon49/pip-licenses) which is unable to find the licensing information in package metadata. Are you really implying the licensing information is missing from package metadata?

According to https://github.com/raimon49/pip-licenses/blob/master/README.md#option-from:
> this tool finds the license from [Trove Classifiers](https://pypi.org/classifiers/) or package Metadata.

Change #19499 does two things:
1. Removing the classifier, which means pip-licenses will be unable to find licensing information in classifiers and should fall back to package metatdata.
2. Modifying licensing declaration in `pyproject.toml` which modifies package metadata. I must admit I don't know which exact changes are applied by maturin, but see below the changes applied by setuptools.

Using setuptools, a `pyproject.toml` file looking like this:
```toml
license = {text = "GPL-3.0-or-later"}
classifiers = [
    "License :: OSI Approved :: GNU General Public License v3 (GPLv3)",
]
```
yields a `PKG-INFO` like this one, with the `LICENSE` file found without explicit declaration:
```
License: GPL-3.0-or-later
Classifier: License :: OSI Approved :: GNU General Public License v3 (GPLv3)
License-File: LICENSE
```
while such a `pyproject.toml` file:
```toml
license = "GPL-3.0-or-later"
```
yields a `PKG-INFO` like this one:
```
License-Expression: GPL-3.0-or-later
License-File: LICENSE
```

I believe pip-licences does not support the switch from `License` to `License-Expression` in package metadata, which is caused by the switch from `license = {text = "..."}` to `license = "..."` in `pyproject.toml`. However, this could be mitigated by keeping the Trove classifier for compatibility. How does this sound?

---

_Comment by @zanieb on 2025-07-31 11:51_

A maturin maintainer identified it as a bug https://github.com/PyO3/maturin/compare/konsti/add-license-files-to-sdist

---

_Comment by @DimitriPapadopoulos on 2025-08-01 14:26_

Once the maturin bug is fixed, hopefully in release 1.9.3, perhaps #19499 can be reconsidered as #19661.

It turns out [pip-licences](https://github.com/raimon49/pip-licenses) is not much maintained and has been forked into [pip-licenses-cli](https://github.com/stefan6419846/pip-licenses-cli) which does support PEP 639. Not sure it's worth taking it into account.

---

_Comment by @konstin on 2025-08-04 18:59_

Maturin 1.9.3 with the fix for source dist license file inclusions is out

---
