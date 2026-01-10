---
number: 2306
title: No validation for --python-platform
type: issue
state: open
author: Jeremiah-England
labels:
  - configuration
  - diagnostics
assignees: []
created_at: 2026-01-02T18:51:02Z
updated_at: 2026-01-03T17:20:21Z
url: https://github.com/astral-sh/ty/issues/2306
synced_at: 2026-01-10T01:51:14Z
---

# No validation for --python-platform

---

_Issue opened by @Jeremiah-England on 2026-01-02 18:51_

### Summary

I tried testing a coworker's issue with `--python-platform windows` and was at first confused why nothing changed. I really needed to use `--python-platform win32`.

I think the command should probably fail for invalid platforms, or at least log a warning. 

### Version

ty 0.0.8

---

_Label `cli` added by @mtshiba on 2026-01-02 21:13_

---

_Comment by @MichaReiser on 2026-01-03 17:18_

The challenge with validating [`sys.platform`](https://docs.python.org/3/library/sys.html#sys.platform) is that there's no complete list of all values. The Python documentation only covers some well-known platforms, but there may be others. 

However, I do think it does make sense for us to add a warning for some values where we can be fairly certain that they're wrong:

* any spelling of a known value that doesn't use the correct casing, e.g. `Linux` instead of `linux`
* `windows` instead of `win32` or macos (any casing) instead of `darwin`. 

The only part that's a bit tricky is that there won't be any way to suppress the warning in case a user indeed meant `Linux` (instead of `linux`). We shouldn't overthink this case today and figure out a solution when we get a user report with a use case where `Linux` (or `windows`) is the desired value.

---

_Label `cli` removed by @MichaReiser on 2026-01-03 17:20_

---

_Label `configuration` added by @MichaReiser on 2026-01-03 17:20_

---

_Label `diagnostics` added by @MichaReiser on 2026-01-03 17:20_

---
