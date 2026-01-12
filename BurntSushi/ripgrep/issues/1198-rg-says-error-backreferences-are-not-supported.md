```yaml
number: 1198
title: "rg says \"error: backreferences are not supported\" about \"\\0\""
type: issue
state: closed
author: waterhouse
labels:
  - wontfix
assignees: []
created_at: 2019-02-12T00:50:30Z
updated_at: 2019-02-12T00:54:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1198
synced_at: 2026-01-12T16:13:23Z
```

# rg says "error: backreferences are not supported" about "\0"

---

_@waterhouse_

#### What version of ripgrep are you using?

$ ~/ripgrep-0.10.0-x86_64-unknown-linux-musl/rg --version
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### Describe your question, feature request, or bug.

If you have "\0" (i.e. ASCII NUL) in a search string, rg dies with "error: backreferences are not supported".
(I am trying to find files with a long run of NUL characters.)
You can work around it by supplying --pcre2, but (a) I suspect that triggers a slower search and (b) it's confusing and a user shouldn't have to do that.

#### If this is a bug, what are the steps to reproduce the behavior?
```
$ echo meh > a.txt
$ head -c 100 /dev/zero > b.txt
$ ~/ripgrep-0.10.0-x86_64-unknown-linux-musl/rg -a -l '\0' *.txt
regex parse error:
    \0
    ^^
error: backreferences are not supported

Consider enabling PCRE2 with the --pcre2 flag, which can handle backreferences
and look-around.
$ ~/ripgrep-0.10.0-x86_64-unknown-linux-musl/rg --pcre2 -a -l '\0' *.txt
b.txt
```
#### If this is a bug, what is the expected behavior?

It did the right thing on a previous version, actually:
```
$ rg -a -l '\0' *.txt
b.txt
$ rg --version
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX
```
I guess this is a side effect of #268 .  I wouldn't quite call \0 an octal escape code, and it's not a backreference either since those are 1-indexed.

---

_Label `wontfix` added by @BurntSushi on 2019-02-12 00:54_

---

_Comment by @BurntSushi on 2019-02-12 00:54_

Yes, it is a side effect of #268. `\0` is definitely an octal escape code---the `\NNN` is its syntax.

There are a variety of ways you can match a NUL byte: `\x00`, `\x{0}`, `\u0000`, `\u{0}`, `\U00000000` or `\U{0}`. This is documented in the regex syntax: https://docs.rs/regex/1.1.0/regex/#escape-sequences

---

_Closed by @BurntSushi on 2019-02-12 00:54_

---
