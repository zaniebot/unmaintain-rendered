---
number: 16914
title: "`uv help` paging breaks when colors are enabled when using `busybox less` compiled without support for `-R`"
type: issue
state: open
author: EliteTK
labels:
  - bug
assignees: []
created_at: 2025-12-01T19:21:04Z
updated_at: 2025-12-01T20:07:06Z
url: https://github.com/astral-sh/uv/issues/16914
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv help` paging breaks when colors are enabled when using `busybox less` compiled without support for `-R`

---

_Issue opened by @EliteTK on 2025-12-01 19:21_

### Summary

Busybox `less` complains and exits (#16015) when passed an unsupported option. I imagine it's quite rare to see in the wild, but it can be compiled without support for `-R` which results in the same error if uv picks it up as the pager and treats it like "normal" `less`. The `LESS` environment variable can be modified to pass `-R` in a way which is ignored by busybox `less` if it encounters it, but this results in escape sequences getting garbled.

Not sure the best way to fix this specifically, but broadly speaking, it may be worth making `uv` fall back to not using the pager if it fails to run.

Example:

```bash session
$ uv help run
less: invalid option -- 'R'
BusyBox v1.37.0.git (2025-12-01 19:10:38 GMT) multi-call binary.

Usage: less [-EFIMmNSh~] [FILE]...

View FILE (or stdin) one screenful at a time

	-E	Quit once the end of a file is reached
	-F	Quit if entire file fits on first screen
	-I	Ignore case in all searches
	-M,-m	Display status line with line numbers
		and percentage through the file
	-N	Prefix line number to each line
	-S	Truncate long lines
	-~	Suppress ~s displayed past EOF
```

### Platform

Linux 6.12.59_1 x86_64 GNU/Linux (but not platform specific)

### Version

uv 0.9.13

### Python version

_No response_

---

_Label `bug` added by @EliteTK on 2025-12-01 19:21_

---

_Comment by @zanieb on 2025-12-01 20:07_

Yeah if the pager fails just falling back to the unpaged output seems totally fine too.

---
