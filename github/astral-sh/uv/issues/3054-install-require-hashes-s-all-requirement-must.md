---
number: 3054
title: "`install --require-hashes`'s \"all requirement must have a hash\" error can be better"
type: issue
state: open
author: bluetech
labels:
  - error messages
assignees: []
created_at: 2024-04-16T07:26:01Z
updated_at: 2024-04-16T16:25:03Z
url: https://github.com/astral-sh/uv/issues/3054
synced_at: 2026-01-07T13:12:17-06:00
---

# `install --require-hashes`'s "all requirement must have a hash" error can be better

---

_Issue opened by @bluetech on 2024-04-16 07:26_

This is in continuation of #3052.

When `uv pip install --require-hashes` is used and a dependency is pinned but doesn't specify a hash, this error is emitted:

```
error: In `--require-hashes` mode, all requirement must have a hash, but none were provided for: setuptools==69.5.1
```

In comparison, pip emits this error:

```
ERROR: Hashes are required in --require-hashes mode, but they are missing from some requirements. Here is a list of those requirements along with the hashes their downloaded archives actually had. Add lines like these to your requirements files to prevent tampering. (If you did not enable --require-hashes manually, note that it turns on automatically when any package has a hash.)
    setuptools==69.5.1 --hash=sha256:c636ac361bc47580504644275c9ad802c50415c7522212252c033bd15f301f32
```

The pip error message is more helpful because it gives the expected hash and allows to quickly copy/paste the line.

BTW: the uv error has a minor typo "all requirement" instead of "all requirement**s**"

---

Version: uv 0.1.32

---

_Renamed from "`install --require-hashes`'s "all requirement must have a hash" can be better" to "`install --require-hashes`'s "all requirement must have a hash" error can be better" by @bluetech on 2024-04-16 08:17_

---

_Label `error messages` added by @charliermarsh on 2024-04-16 13:20_

---

_Comment by @charliermarsh on 2024-04-16 13:20_

Thanks.

---

_Comment by @charliermarsh on 2024-04-16 13:20_

I assume they only show this variant of the error when the requirement is pinned to an exact version?

---

_Comment by @bluetech on 2024-04-16 16:25_

> I assume they only show this variant of the error when the requirement is pinned to an exact version?

Yes, only when pinned but missing hash.

---
