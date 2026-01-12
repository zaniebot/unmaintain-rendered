```yaml
number: 6761
title: Use windows registry to discover python
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: konsti/read-python-from-registry
created_at: 2024-08-28T15:47:17Z
updated_at: 2024-08-29T20:48:25Z
url: https://github.com/astral-sh/uv/pull/6761
synced_at: 2026-01-12T16:07:31Z
```

# Use windows registry to discover python

---

_@konstin_

Our current strategy of parsing the output of `py --list-paths` to get the installed python versions on windows is brittle (#6524, missing `py`, etc.) and it's slow (10ms last time i measured).

Instead, we should behave spec-compliant and read the python versions from the registry following PEP 514.

It's not fully clear which errors we should ignore and which ones we need to raise.

We're using the official rust-for-windows crates for accessing the registry.

Fixes #1521
Fixes #6524

---

_Label `bug` added by @konstin on 2024-08-28 15:47_

---

_Label `enhancement` added by @konstin on 2024-08-28 15:47_

---

_Label `windows` added by @konstin on 2024-08-28 15:47_

---

_@zanieb reviewed on 2024-08-28 15:54_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:314 on 2024-08-28 15:54_

Comment needs to be updated.

---

_@zanieb reviewed on 2024-08-28 15:55_

---

_Review comment by @zanieb on `crates/uv-python/src/py_launcher.rs`:54 on 2024-08-28 15:55_

What's a missing version? A registered Python without a version?

---

_@zanieb reviewed on 2024-08-28 15:57_

---

_Review comment by @zanieb on `crates/uv-python/src/py_launcher.rs`:78 on 2024-08-28 15:57_

Should we say like "Python interpreter in the registry is not executable"?

---

_@zanieb reviewed on 2024-08-28 15:59_

---

_Review comment by @zanieb on `crates/uv-python/src/py_launcher.rs`:84 on 2024-08-28 15:59_

We do have a print for this in the discovery code

https://github.com/astral-sh/uv/blob/7502a963e15ae40422f8386b64f8b6c72af22d82/crates/uv-python/src/discovery.rs#L534-L538

Is this different? Do we need it?

---

_@zanieb reviewed on 2024-08-28 16:01_

---

_Review comment by @zanieb on `crates/uv-python/src/py_launcher.rs`:102 on 2024-08-28 16:01_

Similar to above, I'd say like "Skipping Python interpreter ({}) with invalid registry version {s}: {err}"

We have a message like this elsewhere:

https://github.com/astral-sh/uv/blob/7502a963e15ae40422f8386b64f8b6c72af22d82/crates/uv-python/src/discovery.rs#L621

---

_Comment by @zanieb on 2024-08-28 16:03_

Awesome!

---

_@konstin reviewed on 2024-08-28 16:06_

---

_Review comment by @konstin on `crates/uv-python/src/py_launcher.rs`:54 on 2024-08-28 16:06_

A `None`

---

_@zanieb reviewed on 2024-08-28 16:11_

---

_Review comment by @zanieb on `crates/uv-python/src/py_launcher.rs`:54 on 2024-08-28 16:11_

Makes sense, can you make that clear in the comment though? I didn't learn what we did with missing versions until further down.

---

_Review comment by @zooba on `crates/uv-python/src/py_launcher.rs`:35 on 2024-08-28 18:18_

I think you mean `"PyLauncher"` here.

---

_@zooba reviewed on 2024-08-28 18:23_

As PEP 514 author, I'm happy to see this!

Do note that Store installs don't always show up properly in the registry due to how Windows handles its redirection (and it changes over time as Python chooses different redirection settings). For the `py.exe` launcher, we also do a [specific search](https://github.com/python/cpython/blob/61bef6245c4a32bf430d684ede8603f423d63284/PC/launcher2.c#L1744) for [known package IDs](https://github.com/python/cpython/blob/61bef6245c4a32bf430d684ede8603f423d63284/PC/launcher2.c#L1963) to locate Store installs. Not sure whether you want to copy that or not.

---

_Comment by @zanieb on 2024-08-28 18:24_

Thank you for taking a look @zooba!

---

_Comment by @konstin on 2024-08-29 12:32_

Thank you @zooba, I've added #6807 to also discover Microsoft Store Pythons.

---

_@zanieb approved on 2024-08-29 16:29_

Can you double check the documentation is updated? e.g., https://github.com/astral-sh/uv/blob/ae57d85dfb78ca53478bb503b937906dbf151ac3/docs/concepts/python-versions.md#L172

---

_Comment by @konstin on 2024-08-29 18:20_

Thanks for the reminder!

---

_@zanieb reviewed on 2024-08-29 18:29_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:267 on 2024-08-29 18:29_

Looks wrong
```suggestion
[`python-build-standalone` quirks](https://gregoryszorc.com/docs/python-build-standalone/main/quirks.html)
```

---

_@konstin reviewed on 2024-08-29 18:34_

---

_Review comment by @konstin on `docs/concepts/python-versions.md`:267 on 2024-08-29 18:34_

I ran prettier now

---

_@charliermarsh approved on 2024-08-29 20:01_

---

_Merged by @konstin on 2024-08-29 20:48_

---

_Closed by @konstin on 2024-08-29 20:48_

---

_Branch deleted on 2024-08-29 20:48_

---
