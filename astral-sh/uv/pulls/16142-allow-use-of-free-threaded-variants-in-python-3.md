```yaml
number: 16142
title: Allow use of free-threaded variants in Python 3.14+ without explicit opt-in
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/ft-314
created_at: 2025-10-06T20:45:14Z
updated_at: 2025-10-11T16:45:54Z
url: https://github.com/astral-sh/uv/pull/16142
synced_at: 2026-01-12T16:12:07Z
```

# Allow use of free-threaded variants in Python 3.14+ without explicit opt-in

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/15739
Closes #12445

---

_Marked ready for review by @zanieb on 2025-10-07 03:39_

---

_Review comment by @geofft on `crates/uv-python/src/discovery.rs`:1685 on 2025-10-07 16:10_

Is it intentional that the logic above isn't present on these two lines? If I understand correctly, the implication is `uvx python3.13t` and `uvx python3.13d` will both use a python3.13td if that's the only one you can find?

---

_@geofft reviewed on 2025-10-07 16:11_

If Python 3.14 is not installed, is there any chance that `uvx python3.14` will install a freethreading build instead of a normal one because they now both satisfy the request?

---

_@zanieb reviewed on 2025-10-07 16:14_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1685 on 2025-10-07 16:14_

Yeah, that's the same "backwards compatible" idea captured in the TODO that debug builds don't require opt-in.

---

_@zsol approved on 2025-10-07 16:15_

---

_Comment by @zanieb on 2025-10-07 16:15_

> If Python 3.14 is not installed, is there any chance that `uvx python3.14` will install a freethreading build instead of a normal one because they now both satisfy the request?

No, that's defined by the ordering of downloads. This only changes logic for whether discovered interpreters match a request.

---

_Comment by @zanieb on 2025-10-07 16:15_

It does mean that if you do `uvx python@3.14` and you have a free-threaded interpreter on your PATH before a GIL-enabled interpreter, we'll use the free-threaded one now.

---

_@konstin approved on 2025-10-07 16:20_

---

_Merged by @zanieb on 2025-10-07 16:24_

---

_Closed by @zanieb on 2025-10-07 16:24_

---

_Branch deleted on 2025-10-07 16:24_

---

_Comment by @geofft on 2025-10-07 16:24_

> No, that's defined by the ordering of downloads. This only changes logic for whether discovered interpreters match a request.

It looks like crates/uv-python/download-metadata.json currently orders them as plain, freethreaded, freethreaded+debug, and debug. ... but on prod uv, `uvx python3.14d` grabs debug, not freethreaded+debug, not sure why. If I already have the freethreaded+debug version installed, then it uses that. I think this behavior is fine, but I don't totally understand why....

```
$ uvx python3.14d
cpython-3.14.0rc3+debug-linux-x86_64-gnu (download) ------------------------------ 28.05 MiB/49.79 MiB
^C
$ uvx python3.14td
cpython-3.14.0rc3+freethreaded+debug-linux-x86_64-gnu (download) ------------------------------ 8.01 MiB/55.64 MiB
^C
$ uvx python3.14td
[let it download without interrupting]
Python 3.14.0rc3 free-threading build (main, Sep 18 2025, 19:46:00) [Clang 20.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
$ uvx python3.14d
[no download]
Python 3.14.0rc3 free-threading build (main, Sep 18 2025, 19:46:00) [Clang 20.1.4 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

---

_Comment by @zanieb on 2025-10-07 17:38_

For future reference, the ordering is defined at https://github.com/astral-sh/uv/blob/8f3583a6e63800b770fec3b7be9b754be9d65602/crates/uv-python/src/installation.rs#L580-L590

---

_Comment by @ido123net on 2025-10-10 11:32_

@zanieb is there a way now to explicitly use the GIL enable version if I already installed the free-threaded variant?

Because for 3.13, I need to explicitly add the `t`, but for 3.14, I have no option to add anything for the GIL version.

---

_Comment by @zanieb on 2025-10-10 14:11_

We should prefer the GIL version, e.g., if you do `uv python install 3.14 3.14t` we'll always prefer the GIL version. Otherwise, we'll take the one that's first in `PATH`. Does that help? What's your setup?

---

_Comment by @ido123net on 2025-10-10 14:51_

Thanks @zanieb 
Thats helps.

But still, if for example I do:
```shell
❯ uv sync -p3.14t
Using CPython 3.14.0
Removed virtual environment at: .venv
Creating virtual environment at: .venv
...

❯ uv sync -p3.14 
Resolved 26 packages in 6ms
Audited 22 packages in 1ms

❯ uv run python -VV      
Python 3.14.0 free-threading build (main, Oct  7 2025, 15:52:23) [Clang 20.1.4 ]
```

I have no way to return to the GIL version, without going through different version in between.

---

_Comment by @zsol on 2025-10-10 16:39_

> I have no way to return to the GIL version, without going through different version in between.

One way would be to clear the venv:
```
uv venv --clear
uv sync -p 3.14
```

---

_Comment by @zanieb on 2025-10-10 16:40_

I'll add a way to explicitly request a GIL-enabled version, but I don't know what the syntax will be yet.

---

_Comment by @ido123net on 2025-10-11 16:45_

Maybe environment variable is suitable here, like PREFER_GIL or something similar.

because I agree that it should be the default one, and there is no suffix that make sense..

---
