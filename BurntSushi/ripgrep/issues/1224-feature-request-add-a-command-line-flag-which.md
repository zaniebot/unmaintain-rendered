```yaml
number: 1224
title: "Feature request: Add a command-line flag which takes a file containing a list of files (one per line)."
type: issue
state: closed
author: dilijev
labels:
  - question
assignees: []
created_at: 2019-03-22T01:53:12Z
updated_at: 2019-04-08T12:29:12Z
url: https://github.com/BurntSushi/ripgrep/issues/1224
synced_at: 2026-01-12T16:13:23Z
```

# Feature request: Add a command-line flag which takes a file containing a list of files (one per line).

---

_@dilijev_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

By unzipping https://github.com/BurntSushi/ripgrep/releases/download/0.10.0/ripgrep-0.10.0-x86_64-pc-windows-msvc.zip

#### What operating system are you using ripgrep on?

Windows 10.0.17763.379 (x64)

#### Describe your question, feature request, or bug.

Please add a command-line flag which takes a file containing a list of files (one per line). (Bonus: allow any delimiter like xargs, but this shouldn't be necessary.)

The `xargs`-based solution described in the post below is "fine" but requires that users have `xargs` (not necessarily the case with Windows) and know how to use it. It's also a bit awkward.

https://unix.stackexchange.com/questions/494595/how-can-i-search-a-specific-list-of-files-with-ripgrep



---

_Comment by @dilijev on 2019-03-22 01:54_

I've been looking for an excuse to dig into some rust code (I've done some tutorials but am very beginner with rust). If this is an idea that folks think is worthwhile I'm willing to dig into implementing this sometime in the next few weeks.

---

_Comment by @okdana on 2019-03-22 02:01_

You probably should **not** actually do this, but it is possible... 🤔

```
% print foo > file{1..4}.txt
% print -rl file{1..3}.txt > myfiles.txt
% RIPGREP_CONFIG_PATH=myfiles.txt \rg -e foo
file1.txt
1:foo

file2.txt
1:foo

file3.txt
1:foo
```

---

_Comment by @BurntSushi on 2019-03-22 12:14_

I'm very hesitant to add a flag like this because this really should be handled by whatever shell you're using. I grant that Windows doesn't have `xargs` natively, but I'm not experienced enough with Windows to know whether there is a typical alternative to this. IMO, if `xargs` is available, then it is a fine solution to this problem because it is completely standard.

---

_Label `question` added by @BurntSushi on 2019-03-22 12:14_

---

_Comment by @dilijev on 2019-03-22 20:56_

Seems like powershell has built-in response-file-style "splatting". @array -> expands the array as arguments. There's probably a basic pattern to do that directly from a file.
https://stackoverflow.com/questions/1303921/passing-around-command-line-args-in-powershell-from-function-to-function/1304146#1304146

---

_Comment by @BurntSushi on 2019-04-08 12:29_

I'm going to close this because I don't think there's enough justification to add a new flag for this.

---

_Closed by @BurntSushi on 2019-04-08 12:29_

---
