```yaml
number: 12446
title: "Pinning build dependencies such as `setuptools` in `uv.lock`"
type: issue
state: closed
author: Ben-grmbl
labels:
  - enhancement
assignees: []
created_at: 2025-03-24T19:06:28Z
updated_at: 2025-03-25T12:48:30Z
url: https://github.com/astral-sh/uv/issues/12446
synced_at: 2026-01-12T16:01:03Z
```

# Pinning build dependencies such as `setuptools` in `uv.lock`

---

_@Ben-grmbl_

### Summary

Currently, the `uv.lock` file specifies the exact versions and binary hashes for all direct and indirect dependencies. 

But apparently not the build dependencies. So executing `uv sync` on different machines, or at different times, may result in different builds, depending on the actual build dependencies used. 

Current example: [recent changes in setuptools ](https://setuptools.pypa.io/en/stable/history.html#v78-0-0) broke a number of builds, see e.g. https://github.com/astral-sh/uv/issues/12434 https://github.com/astral-sh/uv/issues/12440

  - **note**: workarounds for that specific problem with setuptools are discussed in #12440

If the specific version of setuptools was pinned in uv.lock, the build would be more reproducible, failing only if setuptools is upgraded, same as any other dependency

### Example

- `uv sync` would always use the version of build dependencies specified in `uv.lock`, similar to other dependencies

- `uv sync --upgrade` would update all dependencies, including build dependencies

- `uv sync --upgrade-package setuptools` would update specific build dependencies

- a new switch `uv sync --upgrade-build-dependencies` could be defined, to update all build dependencies

---

_Label `enhancement` added by @Ben-grmbl on 2025-03-24 19:06_

---

_Comment by @zanieb on 2025-03-24 19:16_

We definitely want to support for this, it's on the short-term roadmap.

---

_Comment by @Job-Heersink on 2025-03-24 19:25_

Currently our builds are failing due to a recent update in setuptools. As @Ben-grmbl mentioned, there is currently no way to specify the version of build dependencies in `uv.lock`.

Is there a known workaround for uv or is the only solution to go back to pip?

---

_Comment by @Ben-grmbl on 2025-03-24 19:44_

> Is there a known workaround for uv or is the only solution to go back to pip?

Workarounds for the current issue with setuptools are discussed in https://github.com/astral-sh/uv/issues/12440 (`--exclude-newer ` should work)

I think that pinning these dependencies has been a bit of a sticky spot for all package managers, including pip, poetry, uv, see e.g.

- https://discuss.python.org/t/no-way-to-pin-build-dependencies/29833
- https://discuss.python.org/t/pinning-build-dependencies/8363




---

_Comment by @alex on 2025-03-25 01:12_

I believe this is the same issue as https://github.com/astral-sh/uv/issues/7052

---

_Closed by @charliermarsh on 2025-03-25 01:27_

---

_Comment by @zanieb on 2025-03-25 01:49_

Thanks! Couldn't find that one :)

---

_Comment by @Ben-grmbl on 2025-03-25 11:17_

@charliermarsh I don't think this issue is a duplicate of https://github.com/astral-sh/uv/issues/7052

If I understand this correctly, that issue deals with generating a requirements.txt file that contains the versions for build dependencies, via `uv pip compile`
- so `uv pip compile ...` --> a requirements-[...].txt is generated, that contains current build dependency versions.

My request here would be to 
- write build dependency versions *and hashes* in the `uv.lock` file, after every `uv sync` 
- and to *re-use* those same build dependencies the next time `uv sync` is called.
- (I do not use `uv pip` currently, a requirements.txt file would not help me.)

So the issues seem related but distinct. 

Could I ask that this issue is reopened? Perhaps I can clarify my feature request some more to make it more clear.

Or should I write my use case as a comment into the other issue instead?


---

_Comment by @charliermarsh on 2025-03-25 12:48_

Yeah, that issue covers both uv.lock and uv pip compile.

---
