```yaml
number: 7649
title: Improve Python executable name discovery when using alternative implementations
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/executable-names-ref
created_at: 2024-09-23T20:17:37Z
updated_at: 2024-09-23T22:17:56Z
url: https://github.com/astral-sh/uv/pull/7649
synced_at: 2026-01-12T16:07:55Z
```

# Improve Python executable name discovery when using alternative implementations

---

_@zanieb_

There are two parts to this. 

The first is a restructuring and refactoring. We had some debt around expected executable name generation, which we address here by consolidating into a single function that generates a combination of names. This includes a bit of extra code around free-threaded variants because this was written on top of #7431 — I'll rebase that on top of this. 

The second addresses some bugs around alternative implementations. Notably, `uv python list` does not discovery executables with alternative implementation names. Now, we properly generate all of the executable names for `VersionRequest::Any` (originally implemented in https://github.com/astral-sh/uv/pull/7508) to properly show all the implementations we can find:

```
❯ cargo run -q -- python list --no-python-downloads
cpython-3.12.6-macos-aarch64-none     /opt/homebrew/opt/python@3.12/bin/python3.12 -> ../Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.11.10-macos-aarch64-none    /opt/homebrew/opt/python@3.11/bin/python3.11 -> ../Frameworks/Python.framework/Versions/3.11/bin/python3.11
cpython-3.9.6-macos-aarch64-none      /Library/Developer/CommandLineTools/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
pypy-3.10.14-macos-aarch64-none       /opt/homebrew/bin/pypy3 -> ../Cellar/pypy3.10/7.3.17/bin/pypy3
```

While doing both of these changes, I ended up changing the priority of interpreter discovery slightly. For example, given that the executables are in the same directory, do we query `python` or `python3.10` first when you request `--python 3.10`? Previously, we'd check `python3.10` but I think that was an incorrect optimization. I think we should always prefer the bare name (i.e. `python`) first. Similarly, this applies to `python` and an executable for an alternative implementation like `pypy`. If it's not compatible with the request, we'll skip it anyway. We might have to query more interpreters with this approach but it seems rare.


Closes https://github.com/astral-sh/uv/issues/7286 superseding https://github.com/astral-sh/uv/pull/7508

---

_Label `internal` added by @zanieb on 2024-09-23 20:17_

---

_Renamed from "Refactore `VersionRequest::executable_names`" to "Refactor `VersionRequest::executable_names`" by @zanieb on 2024-09-23 20:17_

---

_Marked ready for review by @zanieb on 2024-09-23 20:20_

---

_@charliermarsh reviewed on 2024-09-23 20:26_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:2541 on 2024-09-23 20:26_

Are these intentionally repeated?

---

_@zanieb reviewed on 2024-09-23 20:26_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:2541 on 2024-09-23 20:26_

No that's one of the cases I need to fix quick

---

_@charliermarsh reviewed on 2024-09-23 20:26_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:1609 on 2024-09-23 20:26_

Confirming my understanding: given the logic right above this, does that mean `--python 3.12a0` will major `3.12.1` (stable), as an example?

---

_@charliermarsh approved on 2024-09-23 20:27_

---

_@zanieb reviewed on 2024-09-23 20:34_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1609 on 2024-09-23 20:34_

Sorry I don't follow, I think there's something missing from your comment.

---

_Renamed from "Refactor `VersionRequest::executable_names`" to "Improve Python executable name discovery when using alternative implementations" by @zanieb on 2024-09-23 20:49_

---

_Label `internal` removed by @zanieb on 2024-09-23 20:54_

---

_Label `bug` added by @zanieb on 2024-09-23 20:54_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-23 21:12_

---

_@charliermarsh reviewed on 2024-09-23 21:13_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:1609 on 2024-09-23 21:13_

Will `--python 3.12a0` match a Python interpreter with version `3.12.0`?

---

_Comment by @charliermarsh on 2024-09-23 21:15_

So if the user runs `--python 3.10`, we query `python` for its metadata, and if it's a different version, we continue checking the next interpreter, etc.? I assume this was already true, and this PR just inverts the order such that `python` is first, per the summary. What's the motivation for that, though? When would it be incorrect?

---

_@charliermarsh reviewed on 2024-09-23 21:17_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:1618 on 2024-09-23 21:17_

I don't know if the borrow checker will let you, but if so, it might be a bit easier to write this with an iterator (like `names.extend(...)`) so that it's guaranteed that it doesn't loop infinitely (despite modifying `names`).

---

_@charliermarsh reviewed on 2024-09-23 21:18_

---

_Review comment by @charliermarsh on `crates/uv-python/src/implementation.rs`:33 on 2024-09-23 21:18_

What's the motivation for splitting these up? (Is it functional?)

---

_@charliermarsh reviewed on 2024-09-23 21:19_

---

_Review comment by @charliermarsh on `crates/uv-python/src/implementation.rs`:33 on 2024-09-23 21:19_

Oh I see, you use `long_names` elsewhere (per PR summary, to find `pypy`).

---

_@charliermarsh reviewed on 2024-09-23 21:20_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:1634 on 2024-09-23 21:20_

So this adds... `pypy`, `pypy3`, etc.?

---

_@zanieb reviewed on 2024-09-23 21:30_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1609 on 2024-09-23 21:30_

There should be no changes to interpreter match checks here, just the executable names we consider when querying. So yeah `--python 3.12a0` will result in us querying a Python executable with the name `python3.12` and `python3.12.0` etc. but I don't think we will use it if its version isn't actually `3.12.0a0`

---

_@zanieb reviewed on 2024-09-23 21:31_

---

_Review comment by @zanieb on `crates/uv-python/src/implementation.rs`:33 on 2024-09-23 21:31_

Yeah, e.g., we don't want to look for interpreters with executables named `cp` but `cp` is a valid request name when parsing so I had to separate them.

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1634 on 2024-09-23 21:32_

Yep! For each implementation.

---

_@zanieb reviewed on 2024-09-23 21:32_

---

_Comment by @zanieb on 2024-09-23 21:38_

> So if the user runs --python 3.10, we query python for its metadata, and if it's a different version, we continue checking the next interpreter, etc.? I assume this was already true, and this PR just inverts the order such that python is first, per the summary. What's the motivation for that, though? When would it be incorrect?

Yes, this was already true. The things that's changing here is the order of interpreters queried within a single directory. The motivation is consistency and simplicity. It's more complicated to explain and to implement if we want to change the ordering depending on the request given, especially as we add more variants of requests. Here, the ordering is consistent across all requests, we just include more names depending on the request. The motivation for having the original ordering was that it could be more performant to check `python3.10` before `python` if Python 3.10 was requested.

As an elucidating case, if you have `python` which is Python 3.10.1 and `python3.10` which is Python 3.10.2 and ran `python` in your shell you'd get 3.10.1. Generally the feedback we've gotten is that we should do our best to prioritize matching the experience of running `python` from the shell so I think it's more "correct" to use check `python` first but it's pretty ambiguous what the proper resolution order is there so I want to do what's simplest.

---

_@zanieb reviewed on 2024-09-23 21:41_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1618 on 2024-09-23 21:41_

I think I could rewrite some of these as iterators, but I fought with the borrow checker a lot in here and I'd prefer to keep it simple. `for i in 0..names.len()` seems guaranteed to never loop infinitely since the range is evaluated at the start.

---

_Comment by @zanieb on 2024-09-23 22:17_

I'm going to merge because it's blocking some other work, but if you have any concerns following this I'm happy to talk through them.

---

_Merged by @zanieb on 2024-09-23 22:17_

---

_Closed by @zanieb on 2024-09-23 22:17_

---

_Branch deleted on 2024-09-23 22:17_

---
