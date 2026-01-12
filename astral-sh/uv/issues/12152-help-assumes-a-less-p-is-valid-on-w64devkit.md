```yaml
number: 12152
title: Help assumes a less -P is valid on w64devkit installation
type: issue
state: closed
author: m-z-b
labels:
  - bug
assignees: []
created_at: 2025-03-13T18:46:35Z
updated_at: 2026-01-09T15:00:05Z
url: https://github.com/astral-sh/uv/issues/12152
synced_at: 2026-01-12T16:00:56Z
```

# Help assumes a less -P is valid on w64devkit installation

---

_@m-z-b_

### Summary

On Windows, the `uv help` command uses a `less -P prompt` to paginate output if a `less` program is installed. 

Unfortunately the BusyBox version of less (installed by W64Devkit and perhaps others) does not support this option, resulting in an error message for less being displayed instead of any help output. 

i.e. `uv help python` 

Results in a usage message for less.

It's not clear if the `uv help` prompt at the end of each page is useful, so one simple resolution would be to not use the -P prompt option. 



### Platform

Windows W64devkit

### Version

uv 0.6.6

### Python version

N/A

---

_Label `bug` added by @m-z-b on 2025-03-13 18:46_

---

_Comment by @BonkaBonka on 2025-06-13 17:13_

This is also an issue on Alpine Linux for the same reason - unless you install `less` explicitly, it uses busybox's less.

---

_Comment by @zanieb on 2025-06-13 17:24_

Thanks for the reports!

I wonder if there's an easy way for us to detect this before invoking `less`? Or we could capture the failure and retry with no errors?

If you set `PAGER` to a value with arguments, we'll exclude the defaults, e.g. `PAGER="less -R" uv help python` will drop the `-P`.

---

_Comment by @EliteTK on 2026-01-09 15:00_

Fixed for now as a side-effect of #16908.

---

_Closed by @EliteTK on 2026-01-09 15:00_

---
