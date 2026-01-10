```yaml
number: 2244
title: "Add `PIP_COMPATIBILITY.md` to document known deviations from `pip`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/pip-compat
created_at: 2024-03-06T16:19:14Z
updated_at: 2024-03-07T04:34:29Z
url: https://github.com/astral-sh/uv/pull/2244
synced_at: 2026-01-10T14:54:43Z
```

# Add `PIP_COMPATIBILITY.md` to document known deviations from `pip`

---

_Pull request opened by @charliermarsh on 2024-03-06 16:19_

## Summary

Closes https://github.com/astral-sh/uv/issues/2023.


---

_Label `documentation` added by @charliermarsh on 2024-03-06 16:19_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-03-06 16:21_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-06 16:21_

---

_Review requested from @konstin by @charliermarsh on 2024-03-06 16:21_

---

_Comment by @charliermarsh on 2024-03-06 16:22_

Still draft, missing one or two things, and need to adjust the README.

---

_Review comment by @konstin on `PIP_COMPATIBILITY.md`:131 on 2024-03-06 16:23_

I'd skip the original intent part, back there dependency resolution wasn't a thing in python so PEP 440 doesn't account for it, and instead move the link to the pip behavior paragraph above.

---

_Marked ready for review by @charliermarsh on 2024-03-06 16:36_

---

_Comment by @charliermarsh on 2024-03-06 16:37_

Okay, ready for review. The goal here is for this to something we can link to when folks have questions about sub-items in the document.

---

_Review comment by @konstin on `PIP_COMPATIBILITY.md`:161 on 2024-03-06 16:40_

I'd also add that pip makes no guarantees about the order (e.g. https://github.com/pypa/pip/issues/5045#issuecomment-369521345)

---

_Review comment by @konstin on `PIP_COMPATIBILITY.md`:184 on 2024-03-06 16:42_

I'd add a word of warning here about the user and global env here, no isolation between different projects and different cli tools and the global env on linux can break system packages. 

---

_Review comment by @konstin on `PIP_COMPATIBILITY.md`:210 on 2024-03-06 16:43_

:100:

---

_Review comment by @konstin on `PIP_COMPATIBILITY.md`:196 on 2024-03-06 16:45_

I'd move this higher, this is a common confusion for newcomers

---

_Review comment by @konstin on `PIP_COMPATIBILITY.md`:276 on 2024-03-06 16:47_

I'd make this more concrete: Please add `>=` bounds to your packages.

---

_@konstin approved on 2024-03-06 16:48_

:scroll: :scroll: :scroll:

---

_@zanieb approved on 2024-03-06 16:52_

Nice!

---

_Review comment by @zanieb on `PIP_COMPATIBILITY.md`:276 on 2024-03-06 16:53_

Isn't this clear in the example?

---

_@zanieb reviewed on 2024-03-06 16:53_

---

_@notatallshaw reviewed on 2024-03-06 17:31_

---

_Review comment by @notatallshaw on `PIP_COMPATIBILITY.md`:116 on 2024-03-06 17:31_

FYI, I expanded on this a bit here: https://github.com/astral-sh/uv/issues/1641#issuecomment-1981402429

It's not important for uv, but certainly anything stronger than "generally" would be wrong, could even be "sometimes".

Anyway, this is great and reads well.

---

_Review comment by @BurntSushi on `PIP_COMPATIBILITY.md`:141 on 2024-03-06 17:59_

Bold! Hah. Are we sure it's possible? Or rather, _feasibly_ possible?

---

_@BurntSushi approved on 2024-03-06 18:06_

---

_@charliermarsh reviewed on 2024-03-06 21:28_

---

_Review comment by @charliermarsh on `PIP_COMPATIBILITY.md`:116 on 2024-03-06 21:28_

Thanks!

---

_Merged by @charliermarsh on 2024-03-06 21:33_

---

_Closed by @charliermarsh on 2024-03-06 21:33_

---

_Branch deleted on 2024-03-06 21:33_

---

_@kdebrab reviewed on 2024-03-07 01:31_

---

_Review comment by @kdebrab on `PIP_COMPATIBILITY.md`:7 on 2024-03-07 01:31_

`uv pip install`?

---

_@charliermarsh reviewed on 2024-03-07 04:34_

---

_Review comment by @charliermarsh on `PIP_COMPATIBILITY.md`:7 on 2024-03-07 04:34_

Thanks! https://github.com/astral-sh/uv/pull/2262

---
