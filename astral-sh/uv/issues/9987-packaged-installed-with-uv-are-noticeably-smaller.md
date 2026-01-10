---
number: 9987
title: packaged installed with uv are noticeably smaller than when installing with pip
type: issue
state: closed
author: streamnsight
labels:
  - question
assignees: []
created_at: 2024-12-18T00:50:37Z
updated_at: 2024-12-18T05:11:14Z
url: https://github.com/astral-sh/uv/issues/9987
synced_at: 2026-01-10T01:24:48Z
---

# packaged installed with uv are noticeably smaller than when installing with pip

---

_Issue opened by @streamnsight on 2024-12-18 00:50_

Packaged installed with uv are noticeably smaller than when installing with pip

Trying to understand why and what's the difference here. Don't package all comes from pypi anyway? Why would it be so different?
I'm looking at 30% to 50% size difference.

examples:
pandas: 53MB with uv, 65MB with pip
cryptography 13MB with uv, 20MB with pip
oci: 189MB with uv, 363MB with pip

any insight?
Thanks

---

_Comment by @notatallshaw on 2024-12-18 02:21_

Can you explain how you are seeing these size differences?

One big difference is that pip caches the wheels (the zipped up package) and installs the packages into each environment, whereas uv installs the packages intoo a shared location and by default hardlinks from the environment to the shared location and doesn't cache the wheels.

So uv has space savings in two ways, no wheel cache, duplicate installs refer to one location. However if you've only installed once and you've cleared the cache, they should be about the same size.

---

_Comment by @zanieb on 2024-12-18 02:50_

It could also be bytecode? https://docs.astral.sh/uv/pip/compatibility/#bytecode-compilation

---

_Label `question` added by @zanieb on 2024-12-18 02:50_

---

_Comment by @notatallshaw on 2024-12-18 03:21_

Yes, that would also make a difference, I did I recently bring this up on pip side to ask if compiling bytecode should be defaulted to off and the conclusion was it should remain defaulted to on: https://github.com/pypa/pip/issues/12920.

And FWIW in all my production build environments I do end up setting `UV_COMPILE_BYTECODE=1`.

---

_Comment by @streamnsight on 2024-12-18 05:11_

> Can you explain how you are seeing these size differences?

I am building docker images with the code, so I am looking to see how I can make images smaller. 

I've been using 
`du -sh <site-packages/package_name>` to check the size.
especially I was trying to reduce my footprint and see what packages are really large, and that's how I noticed the difference.

Indeed, when I install with the `UV_COMPILE_BYTECODE=1` flag, it takes significantly longer and the size jumps back up to the size with pip. :-/

So, flag OFF sounds like a good idea for size and speed, but not good for service boot time, esp. since a docker image would always start from a fresh file layer. 

That's unfortunate as that kind of defeats the purpose of `uv` for me (speed), while size was an added benefit.



---

_Closed by @streamnsight on 2024-12-18 05:11_

---
