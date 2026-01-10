---
number: 16015
title: "`uv help tool run` fails due to unsupported `less -P` option"
type: issue
state: closed
author: mesaglio
labels:
  - bug
  - compatibility
assignees: []
created_at: 2025-09-24T13:09:49Z
updated_at: 2025-12-01T18:37:36Z
url: https://github.com/astral-sh/uv/issues/16015
synced_at: 2026-01-10T01:26:02Z
---

# `uv help tool run` fails due to unsupported `less -P` option

---

_Issue opened by @mesaglio on 2025-09-24 13:09_

#### Description
When running `uv help tool run`, the command fails because the system `less` binary does not support the `-P` option. This happens in environments where `less` is provided by BusyBox.

#### Steps to Reproduce
```bash
podman run --rm -it alpine
> echo -e "https://dl-cdn.alpinelinux.org/alpine/edge/main\nhttps://dl-cdn.alpinelinux.org
/alpine/edge/community" > /etc/apk/repositories
> apk update && apk add python3 uv
> uv -v help tool run
DEBUG uv 0.8.21
/usr/bin/less: unrecognized option: P
BusyBox v1.37.0 (2025-05-26 20:04:45 UTC) multi-call binary.

Usage: less [-EFIMmNSRh~] [FILE]...

View FILE (or stdin) one screenful at a time

	-E	Quit once the end of a file is reached
	-F	Quit if entire file fits on first screen
	-I	Ignore case in all searches
	-M,-m	Display status line with line numbers
		and percentage through the file
	-N	Prefix line number to each line
	-S	Truncate long lines
	-R	Remove color escape codes in input
	-~	Suppress ~s displayed past EOF
```

The issue seems to come from this section of the code where the `-P` flag is passed to less: https://github.com/astral-sh/uv/blob/ade2bdbd2a58c303b23976be3649ac1ed8796716/crates/uv/src/commands/help.rs#L136



### Platform

Linux 6.12.13-200.fc41.aarch64 aarch64 Linux

### Version

0.8.21

### Python version

Python 3.12.11

---

_Label `bug` added by @mesaglio on 2025-09-24 13:09_

---

_Label `compatibility` added by @zanieb on 2025-09-24 14:33_

---

_Comment by @zanieb on 2025-09-24 14:34_

Alas, I'm not sure what the best fix is there. I guess we can spawn `less` again if we see `unrecognized option: P` but that doesn't seem like a great option.

---

_Comment by @EliteTK on 2025-12-01 12:59_

For notes: Upon some review, it seems like there are a couple more issues:

- The prompt passed with `-P` is always bold, even if `NO_COLOR` is set.
- The prompt is passed as `-P 'uv help: the command you wanted help with'` but the part after the colon and the colon are both getting eaten by less (version 679). Seems like `:` is special, but I didn't look into it much further.
- Some versions of less are ignoring `-P`? At least after asking some colleagues it seems this doesn't work on mac?

Additionally, it's worth noting, busybox less can apparently be compiled without support for `-R`. Not sure how common that is, but there's another potential bug there.

The information which is put in `-P` seems genuinely helpful regardless of whether "normal" `less` is used or not. To me, it seems like a good option would just be to put this as the first line of the output.

Eventually, I think it would be cool if the help pages followed the relatively standard and familiar looking man page look, which is what `cargo` and `git` do. But for now, I've opened #16908 which addresses the `NO_COLOR` issue and avoids using `-P` entirely (which coincidentally means all pagers now print a heading).

---

_Comment by @zanieb on 2025-12-01 14:28_

Mostly as a note... the system `less` on macOS supports `-P` for me

```
❯ less --version
less 581.2 (POSIX regular expressions)
Copyright (C) 1984-2021  Mark Nudelman

less comes with NO WARRANTY, to the extent permitted by law.
For information about the terms of redistribution,
see the file named README in the less distribution.
Home page: https://greenwoodsoftware.com/less
❯ which less
/usr/bin/less
```

<img width="749" height="275" alt="Image" src="https://github.com/user-attachments/assets/160f7731-f6a9-4c18-84c9-be0383c1baf8" />

I'm on an older version though. It does eat the content after colon, which surprised me — I was confused we weren't showing the command :D

---

_Comment by @TBTY-Dev on 2025-12-01 14:44_

#16908 #16907 #16878 #13074 

---

_Referenced in [astral-sh/uv#16908](../../astral-sh/uv/pulls/16908.md) on 2025-12-01 18:11_

---

_Referenced in [astral-sh/uv#16913](../../astral-sh/uv/issues/16913.md) on 2025-12-01 18:36_

---

_Comment by @EliteTK on 2025-12-01 18:37_

For now, the prompt is gone, replaced with a title line when paging. But it may make a comeback in the future.

---

_Closed by @EliteTK on 2025-12-01 18:37_

---

_Referenced in [astral-sh/uv#16914](../../astral-sh/uv/issues/16914.md) on 2025-12-01 19:21_

---
