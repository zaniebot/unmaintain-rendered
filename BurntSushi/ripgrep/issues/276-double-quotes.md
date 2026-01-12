```yaml
number: 276
title: Double quotes?
type: issue
state: closed
author: ci70
labels: []
assignees: []
created_at: 2016-12-08T15:52:42Z
updated_at: 2017-02-09T18:09:02Z
url: https://github.com/BurntSushi/ripgrep/issues/276
synced_at: 2026-01-12T16:13:21Z
```

# Double quotes?

---

_@ci70_

This seems to fail in Windows?

`rg "(?<=\/url\?q=)[^"]*" --mmap c:\test.txt`
Error parsing regex near '(?<=\/u' at character offset 2: Unrecognized flag: '<'. (Allowed flags: i, m, s, U, u, x.)

---

_Comment by @BurntSushi on 2016-12-08 15:55_

ripgrep doesn't support lookaround.

Otherwise, you need to figure out how to escape quotes in your shell. I don't know how to do it on Windows (and I don't know which shell you're using).

---

_Comment by @ci70 on 2016-12-08 15:57_

Okay that sucks a lot. You should mention this in the documentation.

---

_Closed by @BurntSushi on 2016-12-08 15:59_

---

_Comment by @ci70 on 2016-12-08 16:00_

CLEARLY not the best grep replacement out there.

---

_Comment by @BurntSushi on 2016-12-08 18:44_

It is mentioned: https://doc.rust-lang.org/regex/regex/index.html

It probably should be mentioned more prominently. It is discussed at length in my blog post about ripgrep: http://blog.burntsushi.net/ripgrep/#regex-engine

Please try to keep unstructured criticism to a minimum.

---

_Comment by @leeoniya on 2017-02-09 17:53_

@BurntSushi is it possible to use unicode escapes? I'm trying to work around this issue in windows' `cmd` via `\u0022` and getting:

```
Error parsing regex near '[ \?\u0022' at character offset 21: Unrecognized escape sequence: '\u'.
```

---

_Comment by @BurntSushi on 2017-02-09 18:09_

@leeoniya Please open new issues for different questions.

[Unicode escapes are possible.](https://doc.rust-lang.org/regex/regex/index.html#escape-sequences) Use `rg '\x{0022}'` for example.

---
