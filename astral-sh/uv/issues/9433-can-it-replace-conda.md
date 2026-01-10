---
number: 9433
title: Can it replace conda?
type: issue
state: closed
author: MonolithFoundation
labels:
  - duplicate
assignees: []
created_at: 2024-11-26T02:59:44Z
updated_at: 2024-11-26T18:29:02Z
url: https://github.com/astral-sh/uv/issues/9433
synced_at: 2026-01-10T01:24:40Z
---

# Can it replace conda?

---

_Issue opened by @MonolithFoundation on 2024-11-26 02:59_

The slowest thing is not pip, it's conda.

---

_Comment by @FishAlchemist on 2024-11-26 03:30_

There have been previous issues discussing conda, but given the current support for it, I don't see it as a priority in the near future.
* https://github.com/astral-sh/uv/issues/1703

It seems that uv doesn't support conda packages yet, so it might be too early to completely replace it.


---

_Comment by @MonolithFoundation on 2024-11-26 03:38_

Thank you for your reply. Judging from the description of uv, it appears that some C++ shared object (.so) library dependencies cannot be properly handled in uv. This is a significant strength of conda. Literally, some Python libraries such as torch, librosa, and sox-python, etc., require their own.sos. Some even need a more appropriate libstdc++ ABI. In this regard, what is the current support status of uv?

If this can be supported, wouldn't conda can move into trash?

---

_Comment by @konstin on 2024-11-26 15:51_

Shared libraries in packages are supported through the manylinux standard, which is widely used by machine learning libraries.

---

_Comment by @chrisrodrigue on 2024-11-26 18:21_

It would be awesome if uv could install a `.conda` package into a venv.

On Windows, and likely other platforms, Anaconda bundles hundreds of these `.conda` packages in the `anaconda3/pkgs` directory. If one could use this directory as a package source (index) for uv, that would be sweet. uv makes environments way faster than conda.

In the meantime, it looks like `conda convert` could be used to turn the conda packages into wheels, which `uv` knows how to work with. 

I’m not sure what the exact sequence of commands should look like.





---

_Comment by @zanieb on 2024-11-26 18:28_

Let's discuss this in https://github.com/astral-sh/uv/issues/1703 instead

---

_Closed by @zanieb on 2024-11-26 18:28_

---

_Label `duplicate` added by @zanieb on 2024-11-26 18:29_

---
