```yaml
number: 17406
title: Bad error message trying to install abi3 wheel on a free threaded venv
type: issue
state: closed
author: AngheloAlf
labels:
  - good first issue
  - error messages
assignees: []
created_at: 2026-01-11T22:37:21Z
updated_at: 2026-01-17T12:41:36Z
url: https://github.com/astral-sh/uv/issues/17406
synced_at: 2026-01-17T13:11:01Z
```

# Bad error message trying to install abi3 wheel on a free threaded venv

---

_@AngheloAlf_

### Summary

I tried looking for a similar issue but I couldn't find one.

The error message for trying to install a CPython GIL wheel on a free threaded wheel is confusing because it does not show anywhere the installation failed because the current venv is free threaded.

```bash
$ uv pip install dist/crunch64-0.6.1-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl 
Resolved 1 package in 21ms
error: Failed to determine installation plan
  Caused by: A path dependency is incompatible with the current platform: dist/crunch64-0.6.1-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl

hint: The wheel is compatible with CPython 3.7 (`cp37`), but you're using CPython 3.14 (`cp314`)
```

I only encountered this problem because I got an error in CI where the CI failed while trying to test the built wheel by installing it in a freshly created venv, but `uv venv -p 3.14` installed `3.14t` instead of `3.14` (gil one) because the `t` one was available in the `PATH` while the gil one wasn't.
Trying to figure out this issue was annoying because I couldn't reproduce it locally until I figured out the problem was caused by the discrepancy between the free threaded and gil.

So this is a pretty niche issue since most people won't be installing wheels directly, but I still think improving the error message could be helpful.

To reproduce the problem just try to install any Python GIL wheel in a 3.14t venv:

```bash
$ uv venv -p 3.14t
$ source .venv/bin/activate
$ uv pip install ./any_wheel_for_gil_python.whl 
```


### Example


I think this error should mention somewhere that the current venv is using a free threaded Python, the most basic thing would be something among the lines of

```
hint: The wheel is compatible with CPython 3.7 (`cp37`), but you're using CPython 3.14t (`cp314t`)
```
 

---

_Label `enhancement` added by @AngheloAlf on 2026-01-11 22:37_

---

_Label `enhancement` removed by @konstin on 2026-01-12 10:36_

---

_Label `good first issue` added by @konstin on 2026-01-12 10:36_

---

_Label `error messages` added by @konstin on 2026-01-12 10:36_

---

_Renamed from "Improve the error message for trying to install a CPython GIL wheel on a free threaded venv" to "Bad error message trying to install abi3 wheel on a free threaded venv" by @konstin on 2026-01-12 10:37_

---

_Comment by @konstin on 2026-01-12 10:37_

That's a really bad error message, we should fix that

---

_Comment by @nooscraft on 2026-01-12 11:13_

I'm going to take this one

---

_Comment by @nooscraft on 2026-01-12 12:12_

@konstin @zanieb - https://github.com/astral-sh/uv/pull/17415 
please review, thanks

---

_Closed by @zanieb on 2026-01-15 16:58_

---

_Comment by @AngheloAlf on 2026-01-17 12:41_

Thanks a lot! The error message is a lot clearer now!


<img width="1205" height="167" alt="Image" src="https://github.com/user-attachments/assets/fea1b813-b9a5-46c8-a3fa-d655ddfe79d6" />

```sh
$ cargo run -- pip install ./crunch64-0.6.1-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.28s
     Running `target/debug/uv pip install ./crunch64-0.6.1-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`
Resolved 1 package in 5ms
error: Failed to determine installation plan
  Caused by: A path dependency is incompatible with the current platform: crunch64-0.6.1-cp37-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.whl

hint: The wheel uses the stable ABI (`abi3`), but you're using free-threaded CPython 3.14 (`cp314t`), which is incompatible
```

---
