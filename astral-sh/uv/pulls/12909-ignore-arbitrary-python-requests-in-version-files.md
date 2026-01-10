```yaml
number: 12909
title: Ignore arbitrary Python requests in version files
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: release/070
head: zb/pin-arbitrary
created_at: 2025-04-15T21:11:11Z
updated_at: 2025-04-19T19:14:20Z
url: https://github.com/astral-sh/uv/pull/12909
synced_at: 2026-01-10T11:10:40Z
```

# Ignore arbitrary Python requests in version files

---

_Pull request opened by @zanieb on 2025-04-15 21:11_

Closes https://github.com/astral-sh/uv/issues/12605


---

_Label `breaking` added by @zanieb on 2025-04-15 21:11_

---

_Renamed from "zb/pin arbitrary" to "Ignore arbitrary Python requests in version files" by @zanieb on 2025-04-15 21:12_

---

_Comment by @zanieb on 2025-04-15 21:12_

Arguably, we may want to roll this out in preview with a warning first? I don't know why anyone would rely on this behavior though.

---

_Added to milestone `v0.7.0` by @zanieb on 2025-04-15 21:12_

---

_Comment by @zanieb on 2025-04-15 22:00_

The test failures look like a problem, I'll need to look into that. See https://github.com/astral-sh/uv/pull/12925

---

_@jtfmumm reviewed on 2025-04-18 12:48_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-18 12:48_

We currently treat an empty version file as "there is no Python pin"? I think I'd find that surprising, but agree it could be fixed in a separate PR.

---

_Comment by @jtfmumm on 2025-04-18 12:50_

> Arguably, we may want to roll this out in preview with a warning first? I don't know why anyone would rely on this behavior though.

I assume since this is on the `v0.7.0` milestone we're going to skip preview. In that case, looks good.

---

_@jtfmumm approved on 2025-04-18 12:50_

---

_@zanieb reviewed on 2025-04-18 16:29_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-18 16:29_

Yeah, but... it is sort of reasonable if you want to stop discovery of a parent pin for some reason? I'm not sure what the use-case is for an empty file is. I would expect us to continue in _this_ case where we declare a file invalid, e.g., so we fall back to the global config or whatever — but I'm not sure if it's worth trying to implement? At the very least, this shouldn't be a regression because uv is completely unusable with `pyenv-virtualenv` today.

---

_Merged by @zanieb on 2025-04-18 16:45_

---

_Closed by @zanieb on 2025-04-18 16:45_

---

_Branch deleted on 2025-04-18 16:45_

---

_@jtfmumm reviewed on 2025-04-18 16:46_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-18 16:46_

Yeah that makes sense

---

_@zanieb reviewed on 2025-04-18 16:47_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-18 16:47_

- #12972

---

_@edmorley reviewed on 2025-04-19 11:42_

---

_Review comment by @edmorley on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-19 11:42_

> At the very least, this shouldn't be a regression because uv is completely unusable with `pyenv-virtualenv` today.

Personally I think `pyenv-virtualenv`'s design choice to overload the `.python-version` file to point at a specific venv rather than specifying the actual Python version is an anti-pattern. It feels like the venv location should be stored somewhere else (or at the least, as a line in `.python-version` that's a code comment, alongside the actual version). 

The problem with permitting `.python-version` files that contain something other than a version, is that for non-local workflows (eg GitHub Actions, production build systems eg on a PaaS), the `.python-version` venv path won't be valid, and they won't know what Python version to use. (In our Python buildpacks we validate the contents of `.python-version` and reject any that aren't a valid version.)

---

_@zanieb reviewed on 2025-04-19 16:16_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-19 16:16_

> Personally I think pyenv-virtualenv's design choice to overload the .python-version file to point at a specific venv rather than specifying the actual Python version is an anti-pattern. It feels like the venv location should be stored somewhere else (or at the least, as a line in .python-version that's a code comment, alongside the actual version).

I strongly agree.

---

_@zanieb reviewed on 2025-04-19 16:16_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-19 16:16_

It sounds like you're aligned with this change though? Since we're being stricter about `.python-version` file contents?

---

_Review comment by @edmorley on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-19 16:56_

Yeah - I was commenting mainly since it seemed like #12972 was considering relaxing the restrictions - though I probably should have commented there instead :-) 

---

_@edmorley reviewed on 2025-04-19 16:56_

---

_@zanieb reviewed on 2025-04-19 19:13_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-19 19:13_

I think it's a matter of what we do when we encounter an invalid `.python-version` file. Here, we'll ignore an arbitrary name instead of treating it as an interpreter request — but it'll still break discovery of a possibly correct `.python-version` file. In some sense, this is less strict, because now you can use uv with bad `.python-version` files, but... also you can't create them with uv and we won't respect them 🤷‍♀️ 

---

_@zanieb reviewed on 2025-04-19 19:14_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:284 on 2025-04-19 19:14_

In an ideal world, we'd probably error — but I want to enable `pyenv-virtualenv` users to transition to uv for the rest of their workflow.

---
