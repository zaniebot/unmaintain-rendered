```yaml
number: 1140
title: Add an option to override stdin autodetection?
type: issue
state: closed
author: jugglerchris
labels:
  - question
assignees: []
created_at: 2018-12-14T09:31:19Z
updated_at: 2019-04-29T23:42:08Z
url: https://github.com/BurntSushi/ripgrep/issues/1140
synced_at: 2026-01-12T16:13:23Z
```

# Add an option to override stdin autodetection?

---

_@jugglerchris_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Github binary release (ripgrep-0.10.0-x86_64-unknown-linux-musl.tar.gz)

#### What operating system are you using ripgrep on?

Ubuntu 18.04.1

#### Describe your question, feature request, or bug.

My editor spawns processes with a pipe on stdin, in case you want to script input to it; this means that with default options `rg foo` detects the pipe and sits there waiting for something on stdin, instead of searching from cwd.  I think it would help to have an option to override (in both directions) the special treatment of stdin (detecting pipe/file/other).

Not a high priority - I can explicitly add "." if no filename supplied, but it would be simpler to just pass an option.

---

_Comment by @BurntSushi on 2018-12-14 12:09_

I'd rather not, and instead just get stdin detection correct. Running `rg foo` in my editor works fine. If your editor is unconditionally attaching something to stdin, then that's probably a bug on that the editor's end (or with the way you're invoking the subprocess).

---

_Closed by @BurntSushi on 2018-12-14 12:09_

---

_Label `question` added by @BurntSushi on 2018-12-14 12:09_

---

_Comment by @jugglerchris on 2019-01-24 10:04_

This editor has a simple spawn API which always attaches pipes to fds 0-2 in case you want to send/receive data.  I think `rg`'s stdin detection is correct in this case - it's just that in this case the default magic behaviour isn't what I want.  :-)

But as I said it's not a big deal - it's easy to work around with a trivial shell script.

While I'm here, thanks for ripgrep - a fantastic tool and great demonstration of what Rust can do!

---

_Comment by @dwang20151005 on 2019-04-29 20:31_

I also ran into this, because in OCaml one of the standard ways to spawn a new process (`Async.Process`) also attaches a pipe to stdin.  It is unfortunately not an option to use a different library; that would be akin to abandoning glibc or something.

I tried passing `.` but that caused all filenames in the output to be prefixed with `./`, which I did not want.  

Instead, I wrapped the `rg` invocation in a `bash` script that redirects `/dev/null` to stdin.

Magic is great when it works, but when it doesn't, it's nice if I can explicitly ask for the behavior I know I want.  Any chance we could get that override flag?

---

_Comment by @BurntSushi on 2019-04-29 20:48_

> Any chance we could get that override flag?

No. Use `./` as a file path to search if you want to be explicit. If the leading `./` bothers you, then strip it. Either that, or figure out how to launch a process in Ocaml without a pipe attached (or attach it to a file). There really ought to be a way to do one of those things.

---

_Comment by @dwang20151005 on 2019-04-29 21:38_

Yep, I've wrapped my `rg` invocation in a bash script that redirects stdin to `/dev/null`, so this isn't blocking me.

It just seems odd that this is the recommended way to communicate to ripgrep that its input is not on stdin.  I'm mocking out my environment s.t. this heuristic will conclude that there's nothing interesting on stdin.  Why not let me say so explicitly?

---

_Comment by @BurntSushi on 2019-04-29 21:53_

> Why not let me say so explicitly?

You can. `rg foo ./` or by not connecting a pipe.

This conversation is going in circles. If there's nothing new to add, then please drop it.

---

_Comment by @dwang20151005 on 2019-04-29 23:42_

Okay, I will stop arguing for this.

Thanks for the response, and for ripgrep.

---
