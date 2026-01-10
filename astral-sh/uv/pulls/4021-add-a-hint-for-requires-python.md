```yaml
number: 4021
title: "Add a hint for `Requires-Python`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/req-python
created_at: 2024-06-04T20:20:33Z
updated_at: 2024-06-04T20:52:15Z
url: https://github.com/astral-sh/uv/pull/4021
synced_at: 2026-01-10T13:54:02Z
```

# Add a hint for `Requires-Python`

---

_Pull request opened by @charliermarsh on 2024-06-04 20:20_

## Summary

As requested in the originating PR.


---

_Label `error messages` added by @charliermarsh on 2024-06-04 20:20_

---

_Marked ready for review by @charliermarsh on 2024-06-04 20:20_

---

_@zanieb reviewed on 2024-06-04 20:23_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:23_

This reads a little confusing to me, what does this mean? I must use `requires-python >=3.7.9`? Do I need the upper bound too? Perhaps "narrower" isn't intuitive enough for a user-facing message.

---

_@charliermarsh reviewed on 2024-06-04 20:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:25_

The simplest explanation is: `>=3.7` includes versions that aren't included in `>=3.7.9, <4`. You have to use a narrower range, or change the pygls version. What do you suggest?

---

_@zanieb reviewed on 2024-06-04 20:35_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:35_

Yeah that makes sense. I wonder if you should just say something more like that...

> The `Requires-Python` (>=3.7) defined in your `pyproject.toml` includes Python versions that are not supported by the resolved dependencies: pygls>=1.1.0,<=1.2.1 only supports Python >=3.7.9, <4. Use a narrower `Requires-Python` range or try a different `pygls` version.

---

_@zanieb reviewed on 2024-06-04 20:36_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:36_

Sorry I don't have any ideas I love here. I was earnestly confused by the current message and imagine users that don't understand constraint ranges will be very lost.

---

_@charliermarsh reviewed on 2024-06-04 20:37_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:37_

Totally fair. I will play with it.

---

_@zanieb reviewed on 2024-06-04 20:43_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:43_

Thank you! Some prompts that might be helpful: Is this expected to be common? Is the hint going to be displayed more than once in a failed resolution? How verbose can we be?

Separately... I worry about the upper bound, i.e. Poetry bounds to `< 4` by default — does that mean if you have a single package published with Poetry in your dependency tree that you are forced to have this upper bound in your project? I don't see discussion about this in https://github.com/astral-sh/uv/pull/3998. I know there are general complaints about Poetry's behavior here affecting the ecosystem, I'm guessing there's not much we can do about that though? Happy to move this part of the discussion elsewhere.

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:44_

It will only be displayed once (the fields are ignored when testing uniqueness).

---

_@charliermarsh reviewed on 2024-06-04 20:44_

---

_@charliermarsh reviewed on 2024-06-04 20:45_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:45_

> Separately... I worry about the upper bound, i.e. Poetry bounds to < 4 by default — does that mean if you have a single package published with Poetry in your dependency tree that you are forced to have this upper bound in your project?

Yeah, basically. I don't think there's anything we can do about this.


---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:45_

I guess we could special-case things a bit, like... if you have an open-ended `Requires-Python` requirement, treat it as compatible with `< 4`. What do you think?

---

_@charliermarsh reviewed on 2024-06-04 20:45_

---

_@charliermarsh reviewed on 2024-06-04 20:46_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:46_

Or, alternatively, throw out any `< 4` on `Requires-Python`.

---

_@charliermarsh reviewed on 2024-06-04 20:46_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1008 on 2024-06-04 20:46_

Tracking here: https://github.com/astral-sh/uv/issues/4022

---

_Merged by @charliermarsh on 2024-06-04 20:52_

---

_Closed by @charliermarsh on 2024-06-04 20:52_

---

_Branch deleted on 2024-06-04 20:52_

---
