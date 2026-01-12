```yaml
number: 990
title: read stderr when running external commands
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2018-07-21T21:24:14Z
updated_at: 2018-09-05T03:18:57Z
url: https://github.com/BurntSushi/ripgrep/issues/990
synced_at: 2026-01-12T16:13:22Z
```

# read stderr when running external commands

---

_@BurntSushi_

Some recent features of ripgrep have caused it to defer to external commands to produce output that is then searched. @c-blake notes in #989 that this can be problematic if the external command writes a lot of content to stderr. Namely, since ripgrep connects a pipe to the command's stderr but doesn't actually read from it until the process is finished, then it is possible for the program to fill up the pipe's buffer and result in a deadlock: the child process is waiting to write to stderr while ripgrep is waiting for the child process to finish.

Here are some solutions to this that I'm aware of, including the ones @c-blake suggests:

1. Don't fix the problem and just require that external programs redirect stderr to somewhere else.
2. We can stop opening a pipe for stderr, which I believe will result in the external program inheriting ripgrep's stderr.
3. We can redirect stderr to something like `/dev/null`. (This is kind of a riff on (1) I think, because it will just cause folks to redirect stderr somewhere else.)
4. We can implement process execution in a way that reads from stdout and stderr such that no deadlock ever occurs (or available heap is exhausted).

Do there exist other solutions I'm missing?

I'm definitely not a huge fan of (3) since I think that makes failure modes harder to discover than they need be.

(1) is not bad, particularly if the buffer size is large enough by default such that we don't often see this bug in practice. But if the bug does occur, it will be pretty gnarly to diagnose and probably bewildering for end users.

(2) is decent too. I kind of like this option. It's simple. The only real downside is that the output from the external command and ripgrep's output become disassociated, which can make it difficult to understand the error message and correct it (especially if the file path isn't include in the error message). This could also be problematic for "chatty" programs that emit things to stderr, but I suspect such things can typically be quelled by enabling "quiet" mode or some such.

(4) probably provides the best user experience, but is probably also the worst in terms of implementation complexity. I suspect this would require spinning up a thread that reads from stderr. It's not terrible, and it's definitely something that we could encapsulate. We should also be able to do this once for all external commands after a bit of refactoring. This might even be nice as an external crate for others to use. (With that said, are there standard solutions to this problem? If so, is spinning up a new thread the standard portable solution? I guess `select/poll` might work on Linux, but I've never actually written that code before.)

---

_Label `bug` added by @BurntSushi on 2018-07-21 21:24_

---

_Comment by @c-blake on 2018-07-21 21:35_

It was pretty tricky to track down the full stderr pipe error...First I had to isolate which file was causing it, then I made it happen on just that one file, then I was staring at strace output until I realized what was going wrong.

---

_Closed by @BurntSushi on 2018-09-05 03:18_

---
