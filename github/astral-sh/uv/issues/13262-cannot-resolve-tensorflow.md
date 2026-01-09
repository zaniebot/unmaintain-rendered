---
number: 13262
title: "Cannot resolve `tensorflow`"
type: issue
state: closed
author: cbrnr
labels:
  - error messages
assignees: []
created_at: 2025-05-02T10:17:01Z
updated_at: 2025-05-02T10:54:38Z
url: https://github.com/astral-sh/uv/issues/13262
synced_at: 2026-01-07T13:12:18-06:00
---

# Cannot resolve `tensorflow`

---

_Issue opened by @cbrnr on 2025-05-02 10:17_

### Summary

Consider the following script `hello.py`:

```python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "tensorflow",
# ]
# ///

print("Hello, world!")
```

Running this with `uv run hello.py` results in the following error:

```
  × No solution found when resolving script dependencies:
  ╰─▶ Because all versions of tensorflow have no wheels with a matching Python ABI tag (e.g., `cp313`) and you require
      tensorflow, we can conclude that your requirements are unsatisfiable.

      hint: Pre-releases are available for `tensorflow` in the requested range (e.g., 2.19.0rc0), but pre-releases
      weren't enabled (try: `--prerelease=allow`)

      hint: You require CPython 3.13 (`cp313`), but we only found wheels for `tensorflow` (v2.19.0) with the following
      Python ABI tags: `cp39`, `cp310`, `cp311`, `cp312`
```

I do not understand this error, since I am requiring Python >= 3.12 and not 3.13.

### Platform

macOS 15.4.1

### Version

uv 0.7.2

### Python version

_No response_

---

_Label `bug` added by @cbrnr on 2025-05-02 10:17_

---

_Comment by @konstin on 2025-05-02 10:24_

Requiring `>=3.12` supports Python versions 3.12 and up. Currently, Python 3.12 and 3.13 have stable releases, so uv may select either. For example, if the `python` in your `PATH` is a Python 3.13, uv will use this over Python 3.12.

If you want to use a specific Python version, you can use `uv run -p 3.12`. 

---

_Label `bug` removed by @konstin on 2025-05-02 10:24_

---

_Label `question` added by @konstin on 2025-05-02 10:24_

---

_Comment by @cbrnr on 2025-05-02 10:43_

Thanks for the quick response! It's not that I *want* to use a specific Python version, but I have to 😄. I was confused because if there is *no* Python installed, uv will download and use 3.13 (the latest), and therefore fail. If there is *only* 3.12 installed (via `uv python install 3.12`), everything works. However, if I have 3.12 *and* 3.13 installed, uv will always pick 3.13 and fail. I find this to be not ideal, because the result depends on the state of the user's system. Also, the error message said that I required 3.13, which I certainly did not. Not sure if it's just me, but maybe either the message or uv's behavior could be improved?

---

_Label `question` removed by @konstin on 2025-05-02 10:45_

---

_Label `error messages` added by @konstin on 2025-05-02 10:45_

---

_Comment by @konstin on 2025-05-02 10:49_

The error message can definitely be improved here to hint the user to request a different Python version.

It's hard to determine what the right default Python is: Is it the lowest version, because the developer wants to ensure that the package doesn't use newer features and because wheels are likely available when the package was created? Or is it the latest version, as we generally prefer the latest version for packages, too, and after some time wheels may only be available for newer version? uv tries to strike a compromise by reusing a compatible installed, preferring the `python` on PATH, looking for installations in a deterministic order and only installing an additional Python version if required.

---

_Comment by @cbrnr on 2025-05-02 10:54_

Yep, makes sense, for the sake of reproducibility it's best to lock everything (including the Python version) anyways. Thanks!

---

_Closed by @cbrnr on 2025-05-02 10:54_

---
