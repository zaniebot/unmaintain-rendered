```yaml
number: 14201
title: Install PyPy/GraalPy into ~/.local/bin as pypy/graalpy
type: pull_request
state: open
author: RazerM
labels:
  - enhancement
  - breaking
  - preview
assignees: []
base: main
head: feature/python-install-basename
created_at: 2025-06-22T23:47:56Z
updated_at: 2025-10-12T21:17:02Z
url: https://github.com/astral-sh/uv/pull/14201
synced_at: 2026-01-10T06:36:15Z
```

# Install PyPy/GraalPy into ~/.local/bin as pypy/graalpy

---

_Pull request opened by @RazerM on 2025-06-22 23:47_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #13565

## Test Plan

Running the install and uninstall commands.


---

_@RazerM reviewed on 2025-06-22 23:49_

---

_Review comment by @RazerM on `crates/uv-python/src/installation.rs`:399 on 2025-06-22 23:49_

FYI this duplicates some code found in `ManagedPythonInstallation::executable`: https://github.com/astral-sh/uv/blob/a82c210cabde1bb4422639d93ae2791a827c0bc9/crates/uv-python/src/managed.rs#L365-L369

---

_Review requested from @zanieb by @konstin on 2025-06-24 19:56_

---

_Review request for @zanieb removed by @konstin on 2025-06-24 19:56_

---

_Review requested from @Gankra by @konstin on 2025-06-24 19:56_

---

_Review requested from @zanieb by @konstin on 2025-06-24 19:56_

---

_Label `enhancement` added by @konstin on 2025-06-24 19:57_

---

_Comment by @Gankra on 2025-06-26 14:41_

It would be a pretty big breaking change to just do this by default. I would think if we wanted to do this we'd want to default to installing it as *both* python3.12 and pypy3.12, with an option to opt out of the "default" name. This kind of change needs a lot of design work.

---

_Label `preview` added by @zanieb on 2025-06-26 14:43_

---

_Label `preview` added by @zanieb on 2025-06-26 14:43_

---

_Comment by @zanieb on 2025-06-26 14:44_

The feature is in preview, so we can make changes like this though I agree we need to do some design work to be certain we should. I'm not sure if we _should_ ever install PyPy or GraalPy as `python`, is that common? We'd definitely need to do some research to understand common patterns there.

---

_Comment by @RazerM on 2025-06-26 20:12_

> I'm not sure if we _should_ ever install PyPy or GraalPy as `python`, is that common?

I agree, and I don't think it's common. I'm not aware of *any* way to install PyPy (I have no experience with GraalPy) which would put it on your `PATH` as `python...` instead of `pypy...`, which is what my comment in #13565 about what `brew` and `apt` do was referencing.

See also https://doc.pypy.org/en/latest/build.html#installation (emphasis mine):

> To install PyPy system wide on unix-like systems, it is recommended to put the whole hierarchy alone (e.g. in /opt/pypy) and **put a symlink to the pypy executable into /usr/bin or /usr/local/bin**.


---

_@zanieb reviewed on 2025-06-27 23:32_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:399 on 2025-06-27 23:32_

That other code seems wrong, I think we should just do https://github.com/astral-sh/uv/pull/14337

Here, I think we should probably do the same thing and make use of https://github.com/astral-sh/uv/blob/2966471db2dfafa71230cdedfa7f0cd124de02b7/crates/uv-python/src/implementation.rs#L74-L81


---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:399 on 2025-06-28 03:03_

Oh funny, that doesn't work because of `cpython`, I guess we need a special case for that 😭 I'll push a commit to that pull request for it

---

_@zanieb reviewed on 2025-06-28 03:03_

---

_Comment by @RazerM on 2025-07-24 13:53_

Was this overlooked when the feature was stabilized for 0.8, or you decided `pypy-` should be installed as `python-`?

---

_Comment by @zanieb on 2025-09-25 16:26_

We just didn't get to this, but we should.

---

_Added to milestone `v0.9.0` by @zanieb on 2025-09-25 16:26_

---

_Label `breaking` added by @zanieb on 2025-09-25 16:26_

---

_Removed from milestone `v0.9.0` by @zanieb on 2025-10-12 21:17_

---

_Added to milestone `v0.10.0` by @zanieb on 2025-10-12 21:17_

---
