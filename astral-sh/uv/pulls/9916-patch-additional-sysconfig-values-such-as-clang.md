```yaml
number: 9916
title: Patch additional sysconfig values such as clang at install time
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: patch-clang
created_at: 2024-12-15T15:15:31Z
updated_at: 2025-02-08T18:04:55Z
url: https://github.com/astral-sh/uv/pull/9916
synced_at: 2026-01-10T11:10:34Z
```

# Patch additional sysconfig values such as clang at install time

---

_Pull request opened by @samypr100 on 2024-12-15 15:15_

## Summary

Minor follow up to https://github.com/astral-sh/uv/pull/9905 to patch `clang` with `cc`.

Implements the replacements used in [sysconfigpatcher](https://github.com/bluss/sysconfigpatcher/blob/main/src/sysconfigpatcher.py#L54), namely

```python
DEFAULT_VARIABLE_UPDATES = {
    "CC": WordReplace("clang", "cc"),
    "CXX": WordReplace("clang++", "c++"),
    "BLDSHARED": WordReplace("clang", "cc"),
    "LDSHARED": WordReplace("clang", "cc"),
    "LDCXXSHARED": WordReplace("clang++", "c++"),
    "LINKCC": WordReplace("clang", "cc"),
    "AR": "ar",
}
```

## Test Plan

Added an additional test. Tested local python installs.

Related traces
```
TRACE Updated `AR` from `/tools/clang-linux64/bin/llvm-ar` to `ar`
TRACE Updated `CC` from `clang -pthread` to `cc -pthread`
TRACE Updated `CXX` from `clang++ -pthread` to `c++ -pthread`
TRACE Updated `BLDSHARED` from `clang -pthread -shared -L/tools/deps/lib` to `cc -pthread -shared -L/tools/deps/lib`
TRACE Updated `LDSHARED` from `clang -pthread -shared -L/tools/deps/lib` to `cc -pthread -shared -L/tools/deps/lib`
TRACE Updated `LDCXXSHARED` from `clang++ -pthread -shared` to `c++ -pthread -shared`
TRACE Updated `LINKCC` from `clang -pthread` to `cc -pthread
```

## Pending Discussion Items

https://github.com/astral-sh/uv/pull/9905#issuecomment-2543879587

---

_Comment by @zanieb on 2024-12-15 16:47_

I would be surprised if we replaced `clang` with `cc` on a system with `clang` but not `cc`. Is that silly?

---

_Comment by @samypr100 on 2024-12-15 17:09_

> I would be surprised if we replaced `clang` with `cc` on a system with `clang` but not `cc`. Is that silly?

Fair, on the other hand I rather find `cc` to be more pervasive than `clang`. Even on cases where say `gcc` is not installed but `clang` is, `cc` would often point to `clang`.

I think this approach would also help systems that have `gcc` installed (or others) but not `clang` as `cc` would be interopable with either, unless we really want users under all cases to have `clang` installed on their system which is harder in some environments where you can't control the toolchains provided.

---

_Comment by @charliermarsh on 2024-12-15 18:29_

@mitsuhiko did say that this was the most impactful of the sysconfig patches so tagging him.

---

_Comment by @zanieb on 2024-12-16 17:02_

A relevant comment at https://github.com/indygreg/python-build-standalone/issues/210#issuecomment-2029007939 too

---

_Marked ready for review by @samypr100 on 2024-12-17 02:06_

---

_Comment by @samypr100 on 2024-12-17 02:16_

> A relevant comment at [indygreg/python-build-standalone#210 (comment)](https://github.com/indygreg/python-build-standalone/issues/210#issuecomment-2029007939) too

Good insights, at least from my perspective I see a unique distinction between `python-build-standalone` goals and uv with regards to these types of modifications. For example, I can see the strong desire in `python-build-standalone` to maintain ground truth / pristine nature of the builds (avoid modifying anything beyond what's absolutely necessary) for a lot of reasons. Given that `uv` is a consumer of `python-build-standalone`, I think uv is the ideal place to make these type of minor modifications to the downloads in order to achieve the desired level of interopability with the end user's system.

---

_Assigned to @charliermarsh by @zanieb on 2024-12-17 02:40_

---

_Comment by @charliermarsh on 2024-12-17 19:44_

What if we test for clang, and rewrite to gcc if it's not present?

---

_Comment by @zanieb on 2024-12-17 19:52_

Why prefer `clang` / `gcc` over `cc`? Just wondering. I think it _is_ pretty common for it to be linked?

---

_Comment by @charliermarsh on 2024-12-17 19:59_

Oh sorry. I guess `cc` is fine. What's the problem with just using `cc`?

---

_Comment by @zanieb on 2024-12-17 20:02_

Is there one? haha

It might break my system, I alias that to `cargo check` 😭 but I kind of imagine build commands don't invoke a shell.

I think `cc` is really reasonable, honestly.

---

_@zanieb approved on 2024-12-17 20:03_

---

_Label `enhancement` added by @zanieb on 2024-12-17 20:03_

---

_@charliermarsh approved on 2024-12-17 20:08_

---

_Merged by @charliermarsh on 2024-12-17 20:09_

---

_Closed by @charliermarsh on 2024-12-17 20:09_

---

_Branch deleted on 2025-02-08 18:04_

---
