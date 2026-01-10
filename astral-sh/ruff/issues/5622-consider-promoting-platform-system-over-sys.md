```yaml
number: 5622
title: Consider promoting platform.system over sys.platform and os.name
type: issue
state: closed
author: NeilGirdhar
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-07-08T18:28:53Z
updated_at: 2025-03-27T02:39:03Z
url: https://github.com/astral-sh/ruff/issues/5622
synced_at: 2026-01-10T11:09:48Z
```

# Consider promoting platform.system over sys.platform and os.name

---

_Issue opened by @NeilGirdhar on 2023-07-08 18:28_

See [this explanation](https://stackoverflow.com/questions/1854/how-to-identify-which-os-python-is-running-on/58071295#58071295).

---

_Renamed from "Consider urging prefer platform.system over sys.platform and os.name" to "Consider promoting platform.system over sys.platform and os.name" by @NeilGirdhar on 2023-07-08 18:29_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:22_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:34_

---

_Comment by @real-or-random on 2025-03-26 12:29_

I disagree. Different methods have different purposes. As stated in the linked post, if you only care whether you're on posix or not, `os.name == "posix"` is the right thing to do.

---

_Comment by @NeilGirdhar on 2025-03-26 12:40_

> I disagree. Different methods have different purposes. As stated in the linked post, if you only care whether you're on posix or not, `os.name == "posix"` is the right thing to do.

Why not do `platform.system() in {'Linux', 'Darwin'}`?  It's easier to understand, it's easier to alter if what you're checking changes, and it doesn't depend on what future Python versions do with "OS specific modules".

---

_Comment by @real-or-random on 2025-03-26 13:11_

> Why not do `platform.system() in {'Linux', 'Darwin'}`?

This misses Android, iOS, and potentially FreeBSD.

It's just a different check. If I want to know how to interpret the [`os.popen` return code](https://docs.python.org/3/library/os.html#os.popen), I may want to know whether I'm on a POSIX system or not, and the straightforward way to check this is `os.name == "posix"`. 

---

_Comment by @NeilGirdhar on 2025-03-26 13:15_

Got it, thanks.  Does `sys.platform` have common uses too?  It seems like most people will just check if it `startswith` something they know?

---

_Comment by @MichaReiser on 2025-03-26 13:36_

`sys.platforms` is used and [understood by type checkers](https://typing.python.org/en/latest/spec/directives.html#version-and-platform-checking) whereas `os.platform` isn't (but some type checkers do support it, e.g. Pyright). 

A use case where you want to use `sys.platform` is when checking for specific APIs, e.g. https://docs.python.org/3/library/sys.html#sys.activate_stack_trampoline which is only available on linux and not all posix systems. 



---

_Closed by @MichaReiser on 2025-03-26 13:36_

---

_Comment by @NeilGirdhar on 2025-03-26 13:43_

> `sys.platforms` is used and [understood by type checkers](https://typing.python.org/en/latest/spec/directives.html#version-and-platform-checking) whereas `os.platform` isn't (but some type checkers do support it, e.g. Pyright).
> 
> A use case where you want to use `sys.platform` is when checking for specific APIs, e.g. https://docs.python.org/3/library/sys.html#sys.activate_stack_trampoline which is only available on linux and not all posix systems.

How would you check that?   Wouldn't `platform.system() == 'Linux'` be what you want?  As opposed to `sys.platform.startswith('linux')`, which seems much worse.

---

_Comment by @MichaReiser on 2025-03-26 15:35_

I'd use `sys.platform == "linux"`? `startswith` shouldn't be necessary anymore because of 

> Changed in version 3.3: On Linux, [sys.platform](https://docs.python.org/3/library/sys.html#sys.platform) doesn’t contain the major version anymore. It is always 'linux', instead of 'linux2' or 'linux3'.

---

_Comment by @NeilGirdhar on 2025-03-27 02:39_

Okay.  I'm not sure if it's possible, but I think it would be good to come up with canonical ways of accomplishing various tasks and have Ruff nudge users towards these ways.  If `sys.platform == 'linux'` is better than `platform.system() == 'Linux'`, (or vice versa), it would be good to nudge everyone to write the same idea in the same way.

---
