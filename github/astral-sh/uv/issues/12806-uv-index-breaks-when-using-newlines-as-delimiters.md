---
number: 12806
title: UV_INDEX breaks when using newlines as delimiters
type: issue
state: closed
author: winstonallo
labels:
  - bug
  - compatibility
assignees: []
created_at: 2025-04-10T12:52:38Z
updated_at: 2025-04-10T20:22:18Z
url: https://github.com/astral-sh/uv/issues/12806
synced_at: 2026-01-07T13:12:18-06:00
---

# UV_INDEX breaks when using newlines as delimiters

---

_Issue opened by @winstonallo on 2025-04-10 12:52_

### Summary

Hey, we are migrating our Dockerfiles from pip to uv (lifechanging by the way), and when assigning the value of our `PIP_EXTRA_INDEX_URL` to `UV_INDEX`, we noticed that URLs were being merged:

```sh
DEBUG No cache entry for: https://****/00000/packages/pypi/simple/internal-package
DEBUG No cache entry for: https://****/11111/packages/pypi/simplehttps://****/22222/packages/pypi/simple/internal-package
```
The difference is that 00000 and 11111 are separated by a normal whitespace, while 11111 and 22222 are separated by a newline. `pip` has no issue with newline-separated indices, so it might be cool to support them as well to make the jump to uv even easier.

If this is something you would like to support, feel free to assign me this and I will be happy to make a PR! `clap::Arg`'s `value_delimiter` only supports single characters I believe ([here](https://github.com/astral-sh/uv/blob/main/crates/uv-cli/src/lib.rs#L4900)), but I could write a custom parsing function :)

Sorry in advance if this was reported before 😅 

### Platform

Debian Bookworm

### Version

0.6.14

### Python version

3.12

---

_Label `bug` added by @winstonallo on 2025-04-10 12:52_

---

_Assigned to @winstonallo by @Gankra on 2025-04-10 13:54_

---

_Comment by @Gankra on 2025-04-10 13:54_

Would definitely appreciate a patch for this, cleaner migration is always great.

---

_Label `compatibility` added by @Gankra on 2025-04-10 13:55_

---

_Referenced in [astral-sh/uv#12820](../../astral-sh/uv/pulls/12820.md) on 2025-04-10 18:32_

---

_Closed by @Gankra on 2025-04-10 20:22_

---

_Closed by @Gankra on 2025-04-10 20:22_

---
