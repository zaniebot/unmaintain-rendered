---
number: 15307
title: UV_BUILD_CONCURRENCY not working
type: issue
state: open
author: mo22
labels:
  - bug
  - question
assignees: []
created_at: 2025-08-15T13:23:09Z
updated_at: 2025-08-17T10:50:58Z
url: https://github.com/astral-sh/uv/issues/15307
synced_at: 2026-01-07T13:12:19-06:00
---

# UV_BUILD_CONCURRENCY not working

---

_Issue opened by @mo22 on 2025-08-15 13:23_

### Summary

Hi, my goal is to have uv build only a single package at a time to prevent out of memory bugs.

```
export UV_CONCURRENT_BUILDS=1
uv sync --locked
```

However uv spawns multiple build processes (bdist_wheel, setuptools)
```
pstree $$
uv─┬─python───python───python───cmake───ninja───sh───x86_64-linux-gn───cc1plus
   ├─python───cmake───gmake───gmake───gmake───sh───c++───cc1plus
   └─22*[{uv}]
```

```
uv self version
uv 0.8.8
uname -a
Linux ubuntu 6.8.0-71-generic #71-Ubuntu SMP PREEMPT_DYNAMIC Tue Jul 22 16:52:38 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
```

Am I misunderstanding the setting or might this be a bug?


### Platform

Linux 6.8.0-71-generic x86_64 GNU/Linux

### Version

uv 0.8.8

### Python version

Python 3.12.3

---

_Label `bug` added by @mo22 on 2025-08-15 13:23_

---

_Comment by @zanieb on 2025-08-15 13:25_

I don't see `UV_BUILD_CONCURRENCY` in our code base. Do you mean [UV_CONCURRENT_BUILDS](https://docs.astral.sh/uv/reference/environment/#uv_concurrent_builds)?

---

_Label `question` added by @zanieb on 2025-08-15 13:36_

---

_Comment by @mo22 on 2025-08-15 13:39_

> I don't see `UV_BUILD_CONCURRENCY` in our code base. Do you mean [UV_CONCURRENT_BUILDS](https://docs.astral.sh/uv/reference/environment/#uv_concurrent_builds)?

you are completely right - I am setting `UV_CONCURRENT_BUILDS`, still the same issue. Sorry for the confusion.

---

_Comment by @zanieb on 2025-08-15 14:06_

Can you share a small MRE? 

---

_Comment by @mo22 on 2025-08-15 19:39_

Will try as soon as I find time.

For now, looking at `./crates/uv-build-frontend/src/lib.rs` in the current version, it looks as if every SourceBuild creates its own PythonRunner instance that takes the (correct) concurrency as an argument - but the number of SourceBuild instances itself is not limited.

---

_Comment by @mo22 on 2025-08-17 10:50_

Hi @zanieb,

I've prepared a MRE at https://github.com/mo22/uv-concurrency-test

```
uv sync --verbose
DEBUG running bdist_wheel
DEBUG running bdist_wheel
DEBUG running build
DEBUG pkg1 build start
DEBUG running build
DEBUG pkg2 build start
```

the `pkg2 build start` should print only after `pkg1 build end` is printed.

Cause seems to be that the PythonRunner instance with the concurrency semaphore is created for every SourceDist and not passed down globally.

Maybe BuildContext should provide the PythonRunner shared?

---
