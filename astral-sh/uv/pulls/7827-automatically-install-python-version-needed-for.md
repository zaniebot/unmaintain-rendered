```yaml
number: 7827
title: Automatically install Python version needed for tool
type: pull_request
state: closed
author: tfsingh
labels: []
assignees: []
base: main
head: tfsingh/install-err
created_at: 2024-10-01T05:32:30Z
updated_at: 2024-12-02T05:32:01Z
url: https://github.com/astral-sh/uv/pull/7827
synced_at: 2026-01-10T11:59:59Z
```

# Automatically install Python version needed for tool

---

_Pull request opened by @tfsingh on 2024-10-01 05:32_

This PR adds support for automatically installing the Python version required for a given tool, should it not already be installed.

(Tests I added fails, working on it)

Closes #6381

---

_@tfsingh reviewed on 2024-10-01 06:09_

---

_Review comment by @tfsingh on `crates/uv/src/commands/tool/install.rs`:398 on 2024-10-01 06:09_

I'm looking for the best way to derive the necessary Python version error from a [NoSolutionError](https://github.com/astral-sh/uv/blob/1602b5c8d7c63ef175e0fe1bb5088c6e9f8f26b1/crates/uv-resolver/src/error.rs#L108) instance (potentially via a combination of an explicit call to fmt/regex?). Will keep digging, but open to thoughts on how best to approach this.

---

_@zanieb reviewed on 2024-10-01 14:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:398 on 2024-10-01 14:09_

Python requirements are tracked as incompatibilities in the resolver https://github.com/astral-sh/uv/blob/fe88c108136f689c54bc5a9c15df472433d36312/crates/uv-resolver/src/resolver/mod.rs#L2223-L2234 so you should have a `External::FromDependencyOf` with a `PubGrubPackageInner::Python` package in the derivation tree. I'm not sure this is the best method, but should be feasible.

---

_@zanieb reviewed on 2024-10-01 19:47_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:207 on 2024-10-01 19:47_

If you're going to sort and take one value in the end shouldn't you just track the lowest version in an `Option<Version>` in `find_required_python_versions` instead of collecting a `Vec`?

---

_Review comment by @tfsingh on `crates/uv-resolver/src/error.rs`:207 on 2024-10-01 20:20_

That def makes more sense for these purposes — I think I originally thought it best to make the helper return a vector in case there were multiple missing versions but:
1. I'm not certain if that's possible?
2. Even if it is, it's unlikely there'd be a case where someone doesn't just want the maximum

I'll go ahead and make this change

---

_@tfsingh reviewed on 2024-10-01 20:20_

---

_Comment by @tfsingh on 2024-10-01 21:21_

@zanieb Is there precedent for installing Python versions via an override of `UV_PYTHON_DOWNLOADS`/`UV_PYTHON_PREFERENCE` in tests? I think it might be unnecessary to write formal tests for this as it's more of an enhancement, and a test that does a Python installation might worsen already long CI times. What do you think?

---

_Comment by @zanieb on 2024-10-01 21:43_

Yeah we need to figure out a testing strategy for that but haven't yet. The idea is to setup `UV_PYTHON_INSTALL_MIRROR` to a local cache of fake interpreters or something.

---

_Comment by @tfsingh on 2024-10-01 22:00_

Got it, that makes a lot of sense — thanks again for the feedback!

---

_Marked ready for review by @tfsingh on 2024-10-01 22:00_

---

_@zanieb reviewed on 2024-10-01 22:08_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:222 on 2024-10-01 22:08_

We shouldn't have to collect here, right?

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:238 on 2024-10-01 22:08_

This clone seems avoidable

---

_@zanieb reviewed on 2024-10-01 22:08_

---

_@tfsingh reviewed on 2024-10-01 22:30_

---

_Review comment by @tfsingh on `crates/uv-resolver/src/error.rs`:222 on 2024-10-01 22:30_

Fixed

---

_@tfsingh reviewed on 2024-10-01 22:31_

---

_Review comment by @tfsingh on `crates/uv-resolver/src/error.rs`:238 on 2024-10-01 22:31_

TIL .take() :)

---

_Review requested from @zanieb by @tfsingh on 2024-10-01 22:34_

---

_Closed by @tfsingh on 2024-12-02 05:32_

---
