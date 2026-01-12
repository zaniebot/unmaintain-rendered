```yaml
number: 14008
title: "Warn users on `~=` python version specifier"
type: pull_request
state: merged
author: aaron-ang
labels:
  - error messages
assignees: []
merged: true
base: main
head: compat-version
created_at: 2025-06-12T22:07:58Z
updated_at: 2025-07-02T21:42:14Z
url: https://github.com/astral-sh/uv/pull/14008
synced_at: 2026-01-12T16:10:58Z
```

# Warn users on `~=` python version specifier

---

_@aaron-ang_

Close #7426

## Summary

Picking up on #8284, I noticed that the `requires_python` object already has its specifiers canonicalized in the `intersection` method, meaning `~=3.12` is converted to `>=3.12, <4`. To fix this, we check and warn in `intersection`.

## Test Plan

Used the same tests from #8284.


---

_Marked ready for review by @aaron-ang on 2025-06-13 01:11_

---

_@aaron-ang reviewed on 2025-06-13 05:28_

---

_Review comment by @aaron-ang on `crates/uv/tests/it/lock.rs`:4559 on 2025-06-13 05:28_

How do I hardcode `0` as the patch version? The test converts the patch version to `[X]`, for example `~=3.12.0` to `~=3.12.[X]`.

---

_@zanieb reviewed on 2025-06-13 12:45_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:4559 on 2025-06-13 12:45_

The test suite is agnostic to patch versions by default, to allow testing against a wider range of versions downstream.

Do you need the patch version to be represented in this snapshot?

---

_@zanieb reviewed on 2025-06-13 12:49_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:4559 on 2025-06-13 12:49_

I see, you want the literal `.0` to show. There are a couple things

1. We should consider updating https://github.com/astral-sh/uv/blob/a748336a0983aee884eb91600b8ebc24fc850e0f/crates/uv/tests/it/common/mod.rs#L571-L576 to only filter the actual patch version used in the test, rather than an arbitrary `\d` — that seems separate from this change.

2. You can select a specific patch version from https://github.com/astral-sh/uv/blob/56ce40b0f4139539f345987f7d5bf88236ae2909/.python-versions then request it in test context construction and add the `#[cfg(feature = "python-patch")]` feature to the test

---

_Label `error messages` added by @zanieb on 2025-06-13 12:56_

---

_Review comment by @aaron-ang on `crates/uv/tests/it/lock.rs`:4559 on 2025-06-13 18:26_

I couldn't figure out a straightforward way to get the patch version in `crates/uv/tests/it/common/mod.rs`, so I went with the second option.

---

_@aaron-ang reviewed on 2025-06-13 18:26_

---

_Comment by @aaron-ang on 2025-06-13 23:21_

rebased with `main`

---

_Review requested from @zanieb by @aaron-ang on 2025-06-13 23:22_

---

_Comment by @aaron-ang on 2025-06-23 15:10_

hi @zanieb, any updates on this?

---

_@zanieb approved on 2025-06-27 20:47_

---

_Comment by @zanieb on 2025-06-27 20:48_

Thanks for you patience :)

---

_Merged by @zanieb on 2025-06-27 20:48_

---

_Closed by @zanieb on 2025-06-27 20:48_

---

_Comment by @ashb on 2025-07-02 14:27_

Any chance we could revert this @zanieb @aaron-ang?

As discussed in https://github.com/astral-sh/uv/pull/14335#issuecomment-3027249935 this handling and warning of using `~=3.12` goes counter to the documented Python Version Specifier behaviour, and this is warning rejects something that is 

1. Valid
2. Well defined and specified, and not actually ambiguous.
3. Was already behind handled as per the spec and in line with other tools

---

_Comment by @zanieb on 2025-07-02 14:35_

I've opened an issue for discussion so we can centralize things instead of commenting across pull requests: https://github.com/astral-sh/uv/issues/14422

---

_Comment by @notatallshaw on 2025-07-02 15:49_

> As discussed in [#14335 (comment)](https://github.com/astral-sh/uv/pull/14335#issuecomment-3027249935) this handling and warning of using `~=3.12` goes counter to the documented Python Version Specifier behaviour, and this is warning rejects something that is
> 
>     1. Valid
> 
>     2. Well defined and specified, and not actually ambiguous.
> 
>     3. Was already behind handled as per the spec and in line with other tools

Unfortunately 1 is true, but 2 and 3 are incorrect. 

The spec on [pyproject.toml](https://packaging.python.org/en/latest/specifications/pyproject-toml/#requires-python) says:

>     Corresponding core metadata field: Requires-Python
>
> The Python version requirements of the project.

And the [core metadata field](https://packaging.python.org/en/latest/specifications/core-metadata/#core-metadata-requires-python) says:

> This field specifies the Python version(s) that the distribution is compatible with. Installation tools may look at this when picking which version of a project to install.
> 
> The value must be in the format specified in Version specifiers.

Note that it says *format* of a Version specifier, not semantics of a Version specifier.

The semantics of what requires-python in the pyproject.toml and Requires-Python in the core metadata field are not specified. And this has led to different tools implementing them differently. There are multiple discussions threads on this:

* https://discuss.python.org/t/requires-python-and-pre-release-python-versions/62959
* https://discuss.python.org/t/requires-python-upper-limits/12663

It clear from the discussions that:

* The intention was that requires-python should only specify the minimum 
* The intention was requires-python should only specify the language version (e.g. it should not encode CPython patch information).

I believe the pylock.toml specification explicitly encodes these intentions in it's specification of requires-python.

It should also be noted that NO tool treats requires-python nor Requires-Python exactly like a PEP 440 Version specifier. 

I was hoping to write a PEP this year on standardizing this, but I've found so many different use cases I'm concerned it's not easily possible. 

I have no objections to uv handling requires-python on how it best fits it's user. But be aware this is not defined by the specification and interoperability between different tools will continue to be an issue. 

---

_Comment by @potiuk on 2025-07-02 21:42_

> Unfortunately 1 is true, but 2 and 3 are incorrect.

Thanks @notatallshaw -> TIL. That's a very sad state of affairs.

---
