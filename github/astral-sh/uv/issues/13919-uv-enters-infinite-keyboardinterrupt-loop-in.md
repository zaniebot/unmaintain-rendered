---
number: 13919
title: "uv enters infinite `KeyboardInterrupt` loop in interactive shell"
type: issue
state: closed
author: piehld
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-06-09T15:23:05Z
updated_at: 2025-06-16T13:44:28Z
url: https://github.com/astral-sh/uv/issues/13919
synced_at: 2026-01-07T13:12:18-06:00
---

# uv enters infinite `KeyboardInterrupt` loop in interactive shell

---

_Issue opened by @piehld on 2025-06-09 15:23_

### Summary

When I am in the interactive Python shell (`uv run python`) and type `Ctrl` + `c` to issue a `KeyboardInterrupt`, the shell enters an infinite loop that's impossible to get out of. Below is a GIF demonstrating the behavior:

![Image](https://github.com/user-attachments/assets/741c3fb7-dd4e-4544-8967-9429470db1fc)

I believe this relates to these other issues: https://github.com/astral-sh/uv/issues/12108 and https://github.com/astral-sh/uv/issues/13429. However, I didn't see this particular example being shared, so I wanted to provide a simple demonstration of the issue I'm observing. Please feel free to close this as a duplicate, but thought this example case might helpful.

### Platform

macOS 15.4, Darwin 24.4.0 arm64

### Version

uv 0.7.9 (Homebrew 2025-05-30)

### Python version

Python 3.12.10

---

_Label `bug` added by @piehld on 2025-06-09 15:23_

---

_Comment by @zanieb on 2025-06-09 15:30_

Interesting, I can reproduce this — but it doesn't happen on 3.13

---

_Comment by @piehld on 2025-06-09 15:46_

Oh, interesting. That would probably explain why I wasn't seeing this before, since I recently had to downgrade to 3.12 for a project.

---

_Label `help wanted` added by @zanieb on 2025-06-09 16:33_

---

_Comment by @zanieb on 2025-06-09 16:33_

I'd definitely appreciate help understanding why this differs across Python versions. I think we'll need to know that before we can try to fix it.

---

_Referenced in [astral-sh/uv#13925](../../astral-sh/uv/pulls/13925.md) on 2025-06-09 16:47_

---

_Comment by @oconnor663 on 2025-06-10 20:27_

One of the culprits here is libedit. CPython uses GNU readline by default, but [our standalone builds substitude libedit on Linux](https://gregoryszorc.com/docs/python-build-standalone/20211017/quirks.html#use-of-libedit-on-linux). (You can reproduce this by building CPython from source with `./configure --with-readline=edit`.) Separately, Python 3.13 is [replacing the default REPL implementation](https://docs.python.org/3/whatsnew/3.13.html#a-better-interactive-interpreter), so you only get this behavior under 3.13 if you link it to libedit _and_ set `PYTHON_BASIC_REPL=1`.

What libedit is doing is reacting to `SIGINT` (at least) by signaling its entire process group. Here's [the line of code where it does that](https://salsa.debian.org/debian/libedit/-/blob/08da76b7e6d925652df0bfc49b085446bbd993bc/src/sig.c#L110) (`0` in the first argument position [means the entire pgroup](https://man7.org/linux/man-pages/man2/kill.2.html)):

```c
```c
/* sig_handler():
 *	This is the handler called for all signals
 *	XXX: we cannot pass any data so we just store the old editline
 *	state in a private variable
 */
static void
sig_handler(int signo)
{
        // ...
	// [many lines omitted]
        // ...
	(void) kill(0, signo);
	errno = save_errno;
}
```

Libedit does this regardless of whether you signal it with ^C in the terminal (which already signals the whole pgroup) or with e.g. `kill -INT $PID`. I _believe_ the original intention was that libedit would clear the terminal's `ISIG` mode flag, so that ^C would be delivered as a character instead of as a signal, in which case libedit would need to take responsibility for sending the group signal. However, as far as I can tell, current (within the last decade?) versions of libedit do _not_ clear `ISIG`, at least not by default. This behavior might be vestigial, or there only to support an uncommon libedit mode? It's hard to tell, in part because libedit isn't on GitHub :p

This interacts poorly with our current `SIGINT` forwarding (which incidentally is [about to change](https://github.com/astral-sh/uv/pull/13925)):

https://github.com/astral-sh/uv/blob/90a7208a73a2050fe06ff37db11bbc60a7919ef9/crates/uv/src/commands/run.rs#L165-L169

1. `uv` gets `SIGINT` from ^C. (Currently we [ignore the first one we see](https://github.com/astral-sh/uv/blob/90a7208a73a2050fe06ff37db11bbc60a7919ef9/crates/uv/src/commands/run.rs#L160-L163), so this only applies to subsequent ones.)
2. We wait 200ms.
3. We forward `SIGINT` to the child.
4. In this case the child is running libedit and signals its entire pgroup, which includes `uv`.
5. Repeat.

This sequence of events isn't the whole picture, because for example `SIGINT` is _also_ delivered to the child in step 1, which means `uv` probably gets a second `SIGINT` immediately. Depending on who goes first, this could end up getting "coalesced" with the first one, or it could kick us into the infinite loop after a single Ctrl-C. I think the first case is more common, but I also think I see the second case happen sometimes? Really hard to say.

What _should_ we be doing here? I'm not sure I fully understand all the end user cases that led us to write this code, but my current guess is that we should always forward `SIGINT` when the child's PGID has changed, and never otherwise. (This is separate from `SIGTERM`, which both requires special Docker / PID 1 handling, and also isn't usually delivered to a group.)

---

_Referenced in [astral-sh/python-build-standalone#652](../../astral-sh/python-build-standalone/pulls/652.md) on 2025-06-11 03:09_

---

_Closed by @zanieb on 2025-06-11 13:28_

---

_Comment by @piehld on 2025-06-11 14:11_

Wow, @oconnor663 and @zanieb thank you so much for the thorough investigation and super quick patch! I look forward to trying it out in the next Brew release.

Thank you again!

---

_Referenced in [astral-sh/uv#13983](../../astral-sh/uv/issues/13983.md) on 2025-06-12 13:42_

---

_Comment by @piehld on 2025-06-16 13:44_

Just confirmed—I upgraded to `uv 0.7.13 (Homebrew 2025-06-12)`, and the `Ctrl` + `c` keyboard interrupt works as expected now. Thank you again for the immediate fix!

---

_Referenced in [astral-sh/uv#13429](../../astral-sh/uv/issues/13429.md) on 2025-06-23 15:22_

---
